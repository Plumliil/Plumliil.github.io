---
title: Js中几种循环的使用
tags:
  - 前端篇
  - JavaScript
date: 2022-04-23 21:15:17
---




在JavaScript中有五种常用的循环，现在来分别介绍一下五种循环的用法。

### 1.while

当满足条件时进入循环，进入循环后，当条件不满足时，跳出循环。while语句的一般表达式为：while（表达式）{循环体}。

```javascript
let num = 0;
while (num < 5) {
    num++;
    console.log(num);
}
// 1 
// 2 
// 3 
// 4
// 5
while (num > 5) {
    num++;
    console.log(num);
}
// 5
// 4
// 3
// 2
// 1
```

### 2.do...while

do {循环体} while（表达式）

用法和while相差不大，但有特殊的一点是当while后条件表达式为false时，do内的循环体仍然会执行一次

```javascript
let num = 0;
do {
    num++;
    console.log(num);
} while (num < 5)
// 1
// 2
// 3
// 4
// 5
do {
    num++;
    console.log(num);
} while (num > 5)

// 1
```

### 3.for

for循环是平时用法最多的循环，for循环的循环语句由循环体及循环的判定条件两部分组成，其表达式为：for（单次表达式;条件表达式;末尾循环体）{中间循环体；}。因为for循环的单次表达式，条件表达式，末尾循环体可以使用不同的形式，所以for循环很灵活,有多种变式。

```javascript
shilet num = 0;
// 最基本的用法
for (let i = 5; i > num; i--) {
    console.log(i);
}
// 当单次表达式写在判断条件外部时
let i=5;
for(;i>num;i--){
    console.log(i)
}
// 当单次表达式和条件表达式写在判断条件外部时
for(;;i--){
    if(i>0){
        console.log(i);
    }else{
        break
    }
}
for(;true;){
    if(i>0){
        console.log(i);
    }else{
        break
    }
    i--;
}
// 当作while循环使用
// 死循环 一直打印 for
for(;true;){
    onsole.log('for');
}
```

### for...of...

for...of...通常用来遍历传入对象的属性值，前提传入的对象是一个可迭代对象，不可迭代对象需要转为可迭代对象再遍历.

```javascript
let arr = [1, 'a', '2', 3, 'b'];
let obj = {
    name: 'aa',
    age: 11
}
for (const value of arr) {
    console.log(value);
}
// 1
// a
// '2'
// 3
// b
for (const value of obj) {
    console.log(value);
}
// TypeError: obj is not iterable 提示obj不是可迭代对象
```

使用for...of...可以消耗迭代器

```javascript
// o为生成斐波那契数列的一个迭代器对象
let o = {
    n1: 1,
    n2: 1,
    [Symbol.iterator]() {
        return this
    },
    next() {
        let current = this.n2;
        this.n2 = this.n1;
        this.n1 = this.n1 + current;
        if(current>10) return {value: current,done: true}
        return {value: current,done: false}
    }
}

for (const v of o) {
    console.log(v);
}
// 1
// 1
// 2
// 3
// 5
// 8
```

### for...in...

for...in...通常来遍历传入对象的属性名，该循环没有对对象是否可迭代的要求

```javascript
let arr = [1, 'a', '2', 3, 'b'];
let obj = {
    name: 'aa',
    age: 11
}

for (const key in arr) {
    console.log(key);
}
// 0
// 1
// 2
// 3
// 4
for (const key in obj) {
    console.log(key);
}
// name
// age
```