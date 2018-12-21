---
layout: post
title: "ios的缩放bug和微信vue单页面分享踩的坑"
date: 2018-12-30 16:40:52
comments: true
tags: 
	- code 前端 vue
categories:
	- JavaScript 
---
### ios的缩放bug
+ ios10以上的系统meta标签无法禁用缩放，目前好像没有完美的解决方案，只有暂时的[解决方案](https://juejin.im/post/5b46ec375188251ac9767094)
### vue单页实现微信分享踩到的坑
+ 下面这段代码要放在vue的[beforeRouteEnter](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB)勾子中，这样可以保证路由每次发生变化分享代码都会执行，否则被[kepp-alive](https://cn.vuejs.org/v2/api/#keep-alive)包裹的组件，分享代码不会执行，分享就会出现问题，分享的link地址最好自行拼接，不要用location.herf，因为微信分享出去会在地址后面追加参数，在被二次分享后就会出现问题。
```
wx.ready(function () {   //需在用户可能点击分享按钮前就先调用
    wx.updateAppMessageShareData({ 
        title: '', // 分享标题
        desc: '', // 分享描述
        link: `${window.location.origin}${window.location.pathname}#${this.$route.fullpath}`, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
        imgUrl: '', // 分享图标
        success: function () {
          // 设置成功
        }
});
```