---
layout: post
title: "vue-cli中关于webpack的配置"
date: 2019-03-29 16:00:00
comments: true
tags: 
	- vue-cli
	- webpack
categories:
	- JavaScript 
---
### vue-cli 2.0版本根据环境变量打包
1. 安装 cross-env 依赖
```
npm i cross-env -D
```
2.修改build.js
```
将process.env.NODE_ENV = 'production'注释
const spinner = ora(`building for ${process.env.NODE_ENV}...`)
```
3.修改config文件夹下的index.js
```
 build: {
    testEnv: require('./test.env'),
    prodEnv: require('./prod.env'),
    // Template for index.html
    index:
      process.env.env_config === 'prod'
        ? path.resolve(__dirname, '../dist/prod/index.html')
        : path.resolve(__dirname, '../dist/test/index.html'),

    // Paths
    assetsRoot:
      process.env.env_config === 'prod'
        ? path.resolve(__dirname, '../dist/prod')
        : path.resolve(__dirname, '../dist/test'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',

    /**
     * Source Maps
     */

    productionSourceMap: process.env.env_config !== 'prod',
    // https://webpack.js.org/configuration/devtool/#production
    devtool: '#source-map',

    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],

    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  }
```
4.修改webpack.prod.config.js文件
```
将const env = require('../config/prod.env')改为
const env = config.build[`${process.env.env_config}Env`]
```
5.修改config文件下prod.env.js的配置
```
module.exports = {
  NODE_ENV: '"production"',
   ENV_CONFIG: '"prod"',
   BASE_URL：''
}

```
6.在config文件夹下新增test.env.js,配置如下
```
module.exports = {
  NODE_ENV: '"testing"',
   ENV_CONFIG: '"test"',
   BASE_URL：''
}
```

7.在package.json中配置script
```
 "scripts": {
    "test": "cross-env NODE_ENV=testing ENV_CONFIG=test node build/build.js",
    "build": "cross-env NODE_ENV=production ENV_CONFIG=prod node build/build.js"
  },
```
8.运行相应的打包命令
```
npm run test //打包测试环境
npm run build //打包生产环境
```
### vue-cli3.0版本根据不同环境打包
#### vue-cli3支持界面操作,可选配置多,根据环境变量打包也相对来说简单许多。
1.在项目根目录中新建vue.config.js,配置对应参数,还有其他参数请参考[vue-cli3参数配置](https://cli.vuejs.org/zh/config/)
```
module.exports = {
  outputDir: process.env.ENV_CONFIG === 'prod' ? 'dist/pro' : 'dist/test',
  assetsDir: 'static',
  productionSourceMap: process.env.ENV_CONFIG !== 'production',
  css: {
    extract: process.env.NODE_ENV !== 'development'
  }
}

```
2.在文件根目录中新建.env.development 文件、.env.production和.env.test文件,填写对应的变量值,
注意：要在客户端中使用的环境变量必须以VUE_APP开头,在客户端中可以通过process.env.VUE_APP_YOURKEY来获取对应的值,不以VUE_APP只能在node环境下中使用
+ development
```
VUE_APP_BASE_URL ='http://api.dev.com'
NODE_ENV=development
NODE_ENV=development
```
+ test
```
VUE_APP_BASE_URL ='http://api.test.com'
NODE_ENV=production
```
+ production
```
VUE_APP_BASE_URL ='http://api.prod.com'
ENV_CONFIG=prod
ENV_CONFIG=test
NODE_ENV=production
```
3.在package.json文件中配置添加script,npm run build打包生产环境，npm run test打包正式环境
```
"scripts": {
    "build": "vue-cli-service build --mode production",
    "test": "vue-cli-service build --mode test",
  },
```


