---
title: 'call，apply，bind的简单用法'
date: 2021-11-23 21:06:01
tags:
 - 前端篇
 - JavaScript	
---

## 简单了解一下call，apply以及bind的用法

### 首先说一下三者的共同之处
call、apply、bind作用是改变函数执行时的上下文，简单地说就是改变函数运行时的this指向，当我们函数中所需要的this指向不是当前的this时可以用这三种方法来改变this指向
### 其次来介绍一下这三种方法并说明这三种方法的作用
### call()
`call(绑定给this的值，，第一个参数，第二个参数)`

`call(this，agr1，arg2)`

<!--more-->

注意：call（）的传参，每个参数中间用 `,`来分割
~~~javascript
let obj1 = {
  name: 'obj1Name',
  setAge: function (age) {
    this.age = age
  }
}
let obj2 = {
  name: 'obj2Name'
}

obj1.setAge(18);
console.log(obj1); // {"name":"obj1Name","age":18}
// obj2.setAge(19); // obj2.setAge is not a function
console.log(obj2);
obj1.setAge.call(obj2, 20);
console.log(obj2); // {"name":"obj2Name","age":20}
~~~
obj2对象中没有setAge函数当需要用到setAge函数时可以通过call()改变obj1中setAge函数this的指向并传递参数，指向obj2，来给obj2设置age。
### apply()
`apply(绑定给this的值,[arg1,arg2])`

`apply(this，[agr1，arg2])`
~~~javascript
let obj1 = {
  name: 'obj1Name',
  setAge: function (age) {
    this.age = age
  }
}
let obj2 = {
  name: 'obj2Name'
}

obj1.setAge(18);
console.log(obj1); // {"name":"obj1Name","age":18}
// obj2.setAge(19); // obj2.setAge is not a function
console.log(obj2);
obj1.setAge.apply(obj2, [20]);
console.log(obj2); // {"name":"obj2Name","age":20}
~~~
apply()和call()的用法相似，唯一的不同点就是使用apply()方法传参时需要把参数传入数组中
### bind()
`bind(绑定给this的值,arg1,arg2)(arg3)`

`bind(this，agr1，arg2)(arg3)`
和call()很相似，第一个参数是this的指向，从第二个参数开始是接收的参数列表。区别在于bind方法返回值是函数以及bind接收的参数列表的使用。
~~~javascript
function fn(a, b) {
    return this.f+a+b
}
console.log(fn.bind({f:2})(1,1)); // 4
console.log(fn.bind({f:2},2)(1)); // 5
~~~
### call(),apply(),bind()三种方法的区别

bind()返回对应函数, 便于稍后调用； apply(), call()则是立即调用。

#### 需要注意的时箭头函数的this指向，箭头函数内的this的指向指的是箭头函数所在对象中的this