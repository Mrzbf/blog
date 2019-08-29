---
layout: post
title: "axios使用踩坑"
date: 2019-08-29 11:00:52
comments: true
tags: 
	- axios
categories:
	- JavaScript 
---
### content type设置为application/x-www-form-urlencoded时不起作用
[解决方案](https://github.com/axios/axios/issues/362)
```JavaScript
const qs = require('qs');
axios.post('/foo', qs.stringify({ 'bar': 123 });
```
### 拦截器错误信息的处理
```
// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
  // 这样打印可以在Chrome下看到数据结构
  console.log(JSON.parse(JSON.stringify(error))) 
  //这样打印在Chrome下是看不到数据结构的
  console.log(error)
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

