---
layout: post
title: "vue-cli中关于webpack的配置"
date: 2019-04-05 15:00:00
comments: true
tags: 
	- vue-cli
	- webpack
categories:
	- JavaScript 
---
### wbpack [library参数](https://www.webpackjs.com/configuration/output/#output-library)和[libraryTarget参数](https://www.webpackjs.com/configuration/output/#output-librarytarget)
 当你使用一个 js 文件作为webpack构建入口而且包含具名导出，你的库会暴露为一个模块。也就是说你的库必须在 UMD 构建中通过 window.yourLib.default 访问，或在 CommonJS 构建中通过 const myLib = require('mylib').default 访问,你就需要使用到上面的参数，配置如下,不配置的画通过import引入或者require('yourfile.js').default引入的值会是一个空对象
```
module.exports = {
  entry: './your.js',
  output: {
    library: 'yourLib',
    libraryTarget: 'umd'
  },
}
```
### 使用一些loader的时候需要注意的点
当你引入的第三方的文件的时候，改文件已经被loader处理过，则loader的配置项应该去除这些文件，配置如下,同理如果未被loader处理则需引入这些文件
```
const path = require('path');
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')

function resolve(dir) {
  return path.join(__dirname, '..', dir)
}
module.exports = {
  entry: './main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'innoReportWxMpSdk.js',
    library: 'innoReprot',
    libraryTarget: 'umd'
  },
  module: {
    rules: [{
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')],
        exclude: [resolve('src/utils/some.js')
      },]
  },
  plugins: [
    new UglifyJsPlugin({
      uglifyOptions: {
        ecma: 5,
        warnings: false
      }
    })
  ]
};
```
### process.env的使用

demo1
```
created() {
    if(process.env.LINK_URL){
        location.href = process.env.LINK_URL
    }
  },
```
demo2
```
created() {
    location.href = process.env.LINK_URL
  },
```
两个文件的.env的配置一样
```
module.exports = {
  LINK_URL '"production"'
}
```
上面代码demo1在开发环境，或者本地通过node预览的环境运行不会出现问题，但发布到测试环境或者生产环境就会报错Uncaught TypeError: Cannot read property 'LINK_URL' of undefined,process.env是undefined,而demo2不会在任何环境下都不会出现问题，原因是process是node环境下才存在的全局对象，在浏览器环境下不存在该对象，而webpack在构建时会将process.env.linkUrl替换成env文件下对应的值，构建后process.env.linkUrl其实已经变成了http://www.baidu.com。