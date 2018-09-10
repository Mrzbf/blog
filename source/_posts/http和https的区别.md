---
layout: post
title: "http和https的区别"
date: 2018-08-20 14:00:52
comments: true
tags: 
	- code 前端 
categories:
	- JavaScript 
---
1. [http和https的区别](https://blog.csdn.net/xionghuixionghui/article/details/68569282)
2. 由于浏览器的安全策略限制，在http站内只能使用http协议去请求接口，同样的在https站内只能使用https协议去请求接口，请求资源时不会被限制但控制台会有警告，所以网页中请求资源或者接口时,使用//,不要写死http或者https，这样在http站就会使用http协议,在https站内就会使用https协议
```html
<a href='//www.baidu.com'></a>

```