---
title: JsQuestions-2_定时器
date: 2021-10-09 17:39:59
tags:
- Js-Questions
categories: 
  - JavaScript
---

~~~javascript
// 发生什么？
let config = {
  alert: setInterval(()=>{
      console.log('Alert!')
  }，1000)
}
config = null
~~~

- A：setInterval 的回调不会被调用
- B：setInterval 的回调会被调用一次
- C：setInterval 的回调会被每秒钟调用
- D：我们从没调用过config.alert()，config为null

<!--more-->

#### 解析：

##### 答案：C

一般情况下当我们将对象赋值为null，那些对象会被进行垃圾回收（garbage collected）因为已经没有对这些对象的引用了，然而，`setInterval`的参数是一个箭头函数（所以上下文绑定到对象config了），回调函数仍然保留着对config的引用，只要存在引用，对象就不会被垃圾回收，因为没有被垃圾回收，setInterval的回调每1000ms会被调用一次