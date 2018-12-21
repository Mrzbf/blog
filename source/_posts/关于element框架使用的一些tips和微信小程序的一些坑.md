---
title: Js原生api scrollIntoView、element使用的小技巧、vue.nextTick()
date: 2018-11-19 17:40:00
tags:
- gitment
- 
categories:
- blog
---
### 关于element框架使用的一些tips
+ element的大部分组件都会有custom-class这个属性，可以利用custom-class这个类名来改变组件样式达到自己需要的效果，自定义的样式不能加scope，要放在全局样式内，使用custom-class来改变样式也可以防止改变的样式污染全局中其他用到的同一组件的样式。
### 微信小程序的坑
+ 微信小程序使用video组件播放视频的时候，会出现卡顿或者无法播放的问题，加一个custom-cache="{{true}}"即可解决，这个属性文档上没有，是从小程序开发社区中get到的。。
### 读接口数据时的注意点
```javascript
let data=res.data.questions.title
//这样取的话可能报错,当后端返回questions是个null或者undefined的时候就会报语法错误
let data=res.data&&res.data.questions&&res.data.questions.title
```
### 原生Js API scrollIntoView
[Element.scrollIntoView() ](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView)方法让当前的元素滚动到浏览器窗口的可视区域内，调用此方法要在dom加载完毕后。
### Vue.nextTick()
在[vue.nextTick()](https://stackoverflow.com/questions/47634258/what-is-nexttick-or-what-does-it-do-in-vuejs)的回调函数中可以获取数据更新后的最新dom,demo如下
```
<template>
  <section class="index">
    <div class="box" v-if='showStatus' ref='box'>
    
    </div>
    <button @click="chageBoxShowStatus">click</button>
  </section>
</template>

<script>
  export default {
    name: "index",
    data() {
      return {
        showStatus: false
      }
    },
    methods: {
      chageBoxShowStatus() {
        this.showStatus = true
        console.log(this.$refs.box);//这里会打印出undefined,后面的scrollIntoView()自然也调用不了
        this.$nextTick(()=>{
          console.log(this.$refs.box);//这里才会拿到box的dom对象,调用对应api
          this.$refs.box.scrollIntoView()
        })
      }
    }
  }
</script>

<style scoped>
  .box {
    position: relative;
    top: 1000px;
    width: 100px;
    height: 100px;
    background-color: red;
  }
</style>
```