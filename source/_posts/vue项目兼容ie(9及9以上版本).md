---
title: vue兼容IE浏览器（9,10,11)
date: 2019-2-20 17:40:00
tags:
- vue
categories:
- javaScript
---
### vue兼容IE浏览器（9,10,11）
兼容性一直是前端的痛点，vue要兼容ie浏览器则需要做额外的配置，方法如下
+ 第一步，安装babel-polyfill插件，将es6代码转换成es5代码
    + npm i babel-polyfill -D 或者 npm i babel-polyfill --save-dev 
+ 第二步，在入口文件main.js中引入
```
import 'babel-polyfill'
```
+ 第三步，配置.babelrc文件
```json
{
  "presets": [
    [
      "env",
      {
        "modules": false,
        "useBuiltIns": "entry"
      }
    ],
    "stage-3"
  ]
}
```
+ 第四步(非必须)，如果上述操作后ie还不能正常打开，则需要看是否引入第三方的插件中是否有es6代码，存在则对配置做出如下改变，改变webpack.base.config.js中的配置,在include内中将第三方的代码配置进去
```js
 module: {
    rules:[  
        {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test'), resolve('node_modules/webpack-dev-server/client'), resolve('node_modules/vue-phoenix/src/')]
        }
      ]
}

```