---
title: Js判断浏览器进度条是否到底部
date: 2021-11-05 23:07:00
tags:
  - 前端篇
  - JavaScript
---

判断浏览器滚动条是否到达浏览器底部在判断前是需要了解几个关键词

- 滚动条到顶部的距离 scrollTop
- 当前窗口内容的可视区 windowHeight
- 滚动条内容的总高度 scrollHeight

判断到底部的等式是：scrollTop+windowHeight=scrollHeight

具体代码如下：

<!--more-->

```javascript
window.onscroll = function () {
      //变量scrollTop是滚动条滚动时，距离顶部的距离
      var scrollTop =
        document.documentElement.scrollTop || document.body.scrollTop;
      //变量windowHeight是可视区的高度
      var windowHeight =
        document.documentElement.clientHeight || document.body.clientHeight;
      //变量scrollHeight是滚动条的总高度
      var scrollHeight =
        document.documentElement.scrollHeight || document.body.scrollHeight;
      //滚动条到底部的条件
      if (scrollTop + windowHeight == scrollHeight) {
        //写后台加载数据的函数
        console.log(
          "距顶部"  scrollTop +"可视区高度" +windowHeight +"滚动条总高度" +scrollHeight
        );
      }
    };
```