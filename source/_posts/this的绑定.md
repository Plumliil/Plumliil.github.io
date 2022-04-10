---
title: this的绑定
tags:
  - 前端篇
  - JavaScript
  - 读书笔记
date: 2022-04-08 23:38:43
---


在`javascript`中`this`绑定有以下几种方式

##### 默认绑定

~~~javascript
function foo(){
    console.log(this.a);
}
var a = 1;
foo(); // 1
~~~

以上绑定代码即为this的默认绑定,当函数没有其他绑定时,此时this指向window对象;

##### 隐式绑定

隐式绑定需要考虑的是调用位置是否有上下文对象.如下代码所示:

~~~javascript
function foo(){
    console.log(this.a);
}
var obj = { 
    a:'obj',
    foo:foo
}
obj.foo(); // obj
~~~

在上述代码中`foo`函数作为一个值被赋予到了`obj`对象的`foo`属性中,此时的`this`指向`obj`这个对象,调用时

打印的`this.a`为`obj.a`为obj;再如下代码

~~~javascript
function foo() {
  console.log(this.a);
}
var obj2 = {
  a: 'obj2',
  foo: foo
}
var obj1 = {
  a: 'obj1',
  obj2: obj2
}
obj1.obj2.foo(); // obj2
~~~

`foo`函数是`obj2`中`foo`属性的值,上述代码中`obj1`通过调用自己的`obj2`属性来调用`obj2.foo`,打印出的值是`obj2`,可见对对象属性引用链中只有上一层或者说是最后一层在调用位置起作用

###### 隐式丢失

​	在隐式绑定中存在隐式丢失这个问题,被隐式绑定的函数会丢失其隐式绑定对象,也就是说此时函数会应用默认绑定,从而把this绑定到全局对象或者`undefined`上,取决于是否为严格模式;

~~~javascript
function foo() {
    console.log(this.a);
}
function doFoo(fn) {
    fn();
}
var obj = {
    a: 'obj',
    foo: foo
}
var a = 'global this';
doFoo(obj.foo); // global this
doFoo.call(window,obj.foo); // global this
~~~

​	在上述代码中调用`doFoo`传入`obj.foo`此时`fn=obj.foo`(在AO对象中)可以看作一个隐式赋值,此时`doFoo`的`this`指向`window`,所以打印出来`golbal this`

​	如果把函数传入语言内置的函数而不是自己声明的函数,结果是一样的,比如`setTimeout`函数:

~~~javascript
setTimeout(obj.foo,100); // global this
// 伪代码如下
// function  setTimeout(fn,delay) {
//   // 等待delay
//   fn();
// }
~~~

##### 显示绑定

显示绑定可以借助`call`,`apply`函数完成,代码如下:

~~~javascript
function foo(){
    console.log(this.a)
}
var obj = {
    a:'obj'
}
foo.call(obj); // obj
foo.apply(obj); // obj
~~~

​	通过调用`call`或`apply`可以在调用`foo`时前置把它的`this`绑定到`obj`上;

​	在这里还有个概念`装箱`:如果传入了一个原始值(字符串类型,布尔类型或者数字类型)来当做his的绑定对象,这个原始值会被转换成它对应的对象形式(`new String()`...).

但显示绑定也无法解决丢失绑定的问题,此时显示绑定的一个变种硬绑定可以解决这个问题

###### 硬绑定

~~~JavaScript
function foo() {
  console.log(this.a);
}
var obj = {
  a: 'obj'
}
var bar = function () {
  return foo.call(obj)
}
bar();
~~~

​	在上述代码中创建了一个`bar`函数,每次调用`bar`都会在`obj`上调用`foo`这种绑定是一种显示强制绑定,称之为硬绑定.

​	硬绑定的典型应用场景就是创造一个包裹函数,负责接收参数并返回值,由于硬绑定是一种非常常用的模式,所以`es5`提供了内置方法`Function.prototype.bind`,应用方法如下

~~~javascript
function foo(something) {
  console.log(this.a + something);
}
var obj = {
  a: 'obj'
}
foo.bind(obj)('  something'); // obj somehing
~~~

`bind`会返回一个硬编码的新韩淑,它会把指定参数者只为`this`上下文并调用原始函数;

##### 

##### new绑定

~~~javascript
function foo(a){
    this.a = a;
}
var bar = new foo(2);
console.log(bar.a); // 2
~~~

使用`new`调用`foo`时会构造一个新对象并把它绑定到`foo`调用中的`this`上.`new`是最后一种可以影响函数调用时`this`绑定行为的方法.

**读《你不知道的JavaScript 上卷》笔记记录**