---
title: vue中ref属性以及vue-cli项目中资源加载问题
date: 2019-03-02 17:00:00
tags:
- vue
- vue-cli
categories:
- javaScript
---
### refs属性
+ 在vue中建议不去操作dom,但vue中也提供了操作dom的途径，即[ref](https://cn.vuejs.org/v2/api/#vm-refs)属性，dom或者组件上注册过ref属性，在vue中即可通过this.$refs[属性值]去访问dom;
+ 以下是demo
```html
<section class="index">
    <div class="box" v-if="showStatus" ref="box"></div>
    <router-link :to="{name:'home'}">跳转</router-link>
    <button @click="chageBoxShowStatus">click</button>
    <helloWorld ref='hello'></helloWorld>
  </section>
```
```js
export default {
  name: "index",
  data() {
    return {
      showStatus: true,
  },
  components:{
    helloWorld:require('./HelloWorld').default
  },
  created() {
  },
  methods: {
    chageBoxShowStatus() {
      this.$refs.hello.msg='a'
    }
  },
  created() {
    console.log(this.$refs,111) // {}
  },
  mounted() {
    console.log(this.$refs,333）//{box:'<div class="box"></div>',hello:VueComponent}
    console.log(this,444)
  },
};
```
可以总结出如下几点
1. 在mounted钩子中才可以访问到$refs.
2. 当ref注册在dom上，直接this.$refs.属性值即可访问到dom,如果注册在组件上，则需要多一步this.$refs.属性值.$el去访问dom.
### vue-cli项目中图片加载的问题
+ 项目目录结构如下
![image](https://github.com/Mrzbf/Mrzbf.github.io/blob/master/assets/img/js/2019-03-01.png?raw=true)
如果index.vue中代码如下
```html
<template>
  <section class="index">
    <div class='box'></div>
    <img :src="src1" class='src1'></img>
    <img :src="src2" class='src2'></img>
    <img src='@/assets/imgs/1.png' class='src3'>
  </section>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
     src1:'@/assets/1.png',
     src2:require('@/assets/imgs/1.png'),
    };
  },
 
};
</script>

<style scoped>
.box {
  position: relative;
  width: 100px;
  height: 100px;
  background: url('~@/assets/logo.png') no-repeat;
  background-size: cover;
}
</style>
```
如上所示，src1的图片是加载不出来的,控制台会报错，这是因为在webpack中会将图片图片来当做模块来用，因为是动态加载的，所以url-loader将无法解析图片地址，然后打包之后导致路径没有被加工【被webpack解析到的路径都会被解析为/static/img/[filename].png，完整地址为localhost:8080/static/img/[filename].png】
### 图片资源放在static目录下
1. 如果项目发版后是放在网站根目录下，则上述情况无论是template或者js或者css中只需要将图片路径修改为"/static/fiePath/fileName.png"即可
2. 如果项目发版后是放在网站子目录下，则需要进行一些额外的配置以保证资源的正确加载
+ 第一步，config/index.js改成如下所示
```js
assetsPublicPath：'/' 改成 assetsPublicPath:'./'
```
+ 第二步，修改build/util.js中的内容
```js
if (options.extract) {
  return ExtractTextPlugin.extract({
    use: loaders,
    fallback: 'vue-style-loader',
    publicPath: '../../' // 这是打包后的目录结构中index.html相对于css的路径
  })
} else {
  return ['vue-style-loader'].concat(loaders)
}
```
+ 第三步，在代码中使用
  + 在template中 src属性使用相对路径即可
  + 在style标签中url路径使用相对路径即可
  + 在js中则有所区别,实例如下
```
<template>
  <section class="index">
    <div class='box'></div>
    <img :src="src1" class='src1'></img>
  </section>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
     src1:'./static/img/1.png',
    };
  },
 
};
</script>
```





