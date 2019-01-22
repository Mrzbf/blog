---
title: 阿里oss上传文件
date: 2019-01-22 14:00:52
tags:
- oss
- 
categories:
- javaScript
---
### 使用element的upload组件实现阿里oss上传
前端使用oss上传一般由服务端签名后直传，使用element的[upload](http://element-cn.eleme.io/#/zh-CN/component/upload)组件的demo如下
+ dom部分
```html
<el-upload
      :action="uploadUrl"
      :data="extraParams"
      class="component-uploadfile">
</el-upload>
```
+ js部分
```javaScript
export default {
    data () {
        return {
            uploadUrl:'',//阿里云的host地址，建议由服务端返回
            extraParams: {
                key: '', // 这个值是上传oss的文件加拼接上文件名，拼文件名的时候注意文件名不要重复，一般在文件名后拼接个时间戳即可
                policy: '',//服务端签名后返回的参数
                OSSAccessKeyId: '',//服务端签名后返回的参数
                success_action_status: 200,//上传成功的状码，一般都是200
                signature: ''//服务端签名后返回的参数
            },
        }
    }
}
```
以上就是最关键的参数配置，具体业务逻辑可自行实现
### 阿里oss自动部署
前端代码是部署在oss上的，如果每次都使用oss客户端手动上传就比较麻烦，程序猿的宗旨是能用代码解决的坚决不动手，oss也支持使用[node上传](https://help.aliyun.com/document_detail/32068.html?spm=a2c4g.11186623.6.926.5c3f3a7eOtcpBk)，写些js外加npm脚本就可以完美实现了，demo 如下
```js
const fs = require('fs')
const co = require('co')
const path = require('path')
const OSS = require('ali-oss') // oss的sdk
const files = []
const uploadFlagList = []
const OSS_ENV = process.argv.slice(2)[0]
const Ossconfig = {
  // 测试环境
  test: {
    accessKeyId: '', //阿里oss的id,
    accessKeySecret: '',//oss的秘钥
    bucket: '',// oss的域名
    region: '' //oss的区域
  },
  // 生产环境
  build: {
    accessKeyId: '',
    accessKeySecret: '',
    bucket: '',
    region: ''
  }
}
let dirName = `./dist/${OSS_ENV}`
let filePathRoot = path.resolve(__dirname, dirName)
const client = new OSS(Ossconfig[OSS_ENV]);
(() => {
  function readDirSync (filePath) {
    const filePaths = fs.readdirSync(filePath)
    filePaths.forEach((item) => {
      const curPath = `${filePath}/${item}`
      const info = fs.statSync(curPath)
      if (info.isDirectory()) {
        readDirSync(curPath)
      } else {
        files.push(curPath)
      }
    })
  }
  readDirSync(filePathRoot)

  co(function* () {
    try {
      for (let index = 0; index < files.length; index += 1) {
        const fileObj = files[index]
        const result = yield client.put(fileObj.replace(filePathRoot, ''), fileObj)
        //filePathRoot后的参数是上传到oss某个目录下,为空字符串则上传到根路径下
        uploadFlagList.push(result)
        if (!result) break
      }
      const uplaodFlag = uploadFlagList.find(item => item.res.statusCode != 200)
      if (uplaodFlag) {
        console.log('上传失败')
      } else {
        console.log('上传成功')
      }
    } catch (e) {
      console.log('上传失败,请查看日志: ', e)
    }
  })
})()

```
npm脚本配置 即package.json的配置
```json
"scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "lint": "eslint --ext .js,.vue src",
    "build": "node build/build.js",
    "test": "node build/build-test.js",
    "test1": "node build/build-test1.js",
    "buildTest": "node build/build-test.js &&  node uploadOss.js test",
    "buildTest1": "node build/build-test1.js &&  node uploadOss.js test1",
    "buildPro": "node build/build-test.js && node uploadOss.js build"
  },
```
配置完成后，运行（npm run buildTest或者npm run buildTest1） 对应的环境的脚本名即可，自动部署花的时间还是有点长的，因为会先打包一次，如果已经打包好，则 直接运行 node uploadOss.js加对应环境就行了。
