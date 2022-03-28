---
title: 手写call&apply&bind
date: 2022-01-06 22:54:44
tags:
 - JavaScript	
 - call()&apply()&bind()
 - 函数封装
categories: 函数封装
---
主要思想：给对象一个临时函数来调用,调用完毕后删除该临时函数对应的属性
call函数封装
~~~javascript
function pliCall(fn, obj, ...args) {
    if (obj === undefined || obj === null) {
        obj = globalThis
    }
    obj.temp = fn
    let result = obj.temp(...args)
    delete obj.temp
    return result
}
~~~
apply函数
~~~javascript
function pliApply(fn, obj, args) {
    if (obj === undefined || obj === null) {
        obj = globalThis
    }
    obj.temp = fn;
    let result = obj.temp(...args)
    delete obj.temp
    return result
}
~~~
bind函数
~~~javascript
function pliBind(fn, obj, ...args) {
    return function (...args2) {
        if (obj === undefined || obj === null) {
            obj = globalThis
        }
        obj.temp = fn;
        let result = obj.temp(...args, ...args2)
        delete obj.temp;
        return result
    }
}
~~~