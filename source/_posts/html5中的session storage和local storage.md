---
title: html5中的session storage和local storage
date: 2021-07-19 22:48:08
tags:
  - local storage
  - session storage
categories: html5
---

随着互联网的快速发展,基于网页的应用越来越普遍,同时也变的越来越复杂,为了满足各种各样的需求,会经常性在本地存储大量的数据, HTML5规范提出了相关解决方案。

**本地存储特性**

1、数据存储在用户浏览器中

2、设置、读取方便、甚至页面刷新不失数据

3、容量较大, 储存大小约为5MB

4、只能存储字符串,可以将对象JSON.stringifyO编码后存储

<!--more-->

**先说一下sessionStorage和localStorage两者的区别**

前者是一个前端概念，可以将一部分数据在当前会话中保存下来，刷新页面数据依然存在，但关闭页面或者浏览器后数据会被清空。

后者本地存储除非被手动清除否则永久保存

这两者也有相同点，它们存放的数据大小一般为5MB且都是仅在客户端（即浏览器）中保存，不参与和服务器的通信

**window.sessionStorage**

1.生命周期为关闭浏览器窗口

2.在同一个窗口（页面）下数据可以共享

3.以键值对的形式储存使用

存储数据：window.sessionStorage（key，value）



保存数据语法：

```javascript
sessionStorage.setItem("key", "value");
```

读取数据语法：

```javascript
var lastname = sessionStorage.getItem("key");
```

删除指定键的数据语法：

```javascript
sessionStorage.removeItem("key");
```

删除所有数据：

```javascript
sessionStorage.clear();
```

**window.localStorage**

1.生命周期永久生效,除非手动删除否则关闭页面也会存在

2、可以多窗口(同一浏览器可以共享)

3.以键值对的形式存储使用

保存数据语法：

```javascript
localStorage.setItem("key", "value");
```

读取数据语法：

```javascript
var lastname = localStorage.getItem("key");
```

删除数据语法：

```javascript
localStorage.removeItem("key");
```