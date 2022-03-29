---
title: express跨域
tags:
  - Express
categories: 后端篇
date: 2022-03-28 23:03:15
---


### express搭建服务器跨域问题



##### 解决方案:

设置请求头为`"Access-Control-Allow-Origin", "*"`



具体代码

~~~
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With,Content-Type");
    res.header("Access-Control-Allow-Credentials", "true");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' Express 4.17.1');
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
  });
app.use('/api',Router)
~~~

记住:解决跨域问题的这个代码**必须**放在Api调用的**前边**,否则跨域代码不起作用