---
title: 认识Promise
tags:
  - 前端篇
  - JavaScript
date: 2022-04-17 17:01:44
---


### 什么是Promise

Promise是异步编程的一个解决方案：从语法上讲它是一个对象，可以获取到异步操作的消息，从本意上讲，它是一个承诺，承诺过一段时间后它会给你一个结果。Promise有三种状态：pending(等待)，fulilled(成功)，rejected(失败)，状态一旦改变就不会再变，创建Promise后会立即执行。

### 为什么要使用Promise

避免回调地狱

```javascript
// 请求 代表 一个异步网络调用。
// 请求结果 代表网络请求的响应。

请求1(function(请求结果1){
    请求2(function(请求结果2){
        请求3(function(请求结果3){
            请求4(function(请求结果4){
                请求5(function(请求结果5){
                    请求6(function(请求结果3){
                        ...
                    })
                })
            })
        })
    })
})

```

解决办法

```javascript
new Promise(请求1)
    .then(请求2(请求结果1))
    .then(请求3(请求结果2))
    .then(请求4(请求结果3))
    .then(请求5(请求结果4))
    .catch(处理异常(异常信息))
```

### Promise 的常用 API 

- #### Promise.resolve(value)

类方法，该方法返回一个以 value 值解析后的 Promise 对象

1、如果这个值是个 thenable（即带有 then 方法），返回的 Promise 对象会“跟随”这个 thenable 的对象，采用它的最终状态（指 resolved/rejected/pending）

2、如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。

3、其他情况以该值为成功状态返回一个 Promise 对象。

```JavaScript
let p0=Promise.resolve('success');
p0.then(
    res=>{
        console.log(res);
    },
    err=>{
        console.log(err);
    }
)
// success
// 和p1效果相同
let p1=new Promise((resolve,reject)=>{
    resolve('success')
})
p1.then(
    res=>{
        console.log(res);
    },
    err=>{
        console.log(err);
    }
)
// success

//如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。  
function fn(resolve){
    setTimeout(function(){
        resolve(123);
    },3000);
}
let p2 = new Promise(fn);
let p3 = Promise.resolve(p2);
// 返回为true，返回的 Promise 即是 入参的 Promise 对象。
console.log(p2 === p3);
// true
```

- #### Promise.reject

类方法，且与 resolve 唯一的不同是，返回的 promise 对象的状态为 rejected。

- #### Promise.prototype.then

实例方法，为 Promise 注册回调函数，函数形式：fn(vlaue){}，value 是上一个任务的返回结果，then 中的函数一定要 return 一个结果或者一个新的 Promise 对象，才可以让之后的then 回调接收。

- #### Promise.prototype.catch

实例方法，捕获异常，函数形式：fn(err){}, err 是 catch 注册 之前的回调抛出的异常信息。

~~~javascript
let p=Promise.resolve('0 success');
p.then(
    res=>{
        console.log(res);
        return '1 success'
    }
)
.then(res=>{
    console.log(res);
    throw new Error('2 error')
})

.catch(err=>{
    console.log('catch',err);
})
// 0 success
// 1 success
// catch Error: 2 error
~~~

catch方法可以统一捕获then链式调用中产生的错误

- #### Promise.finally

实例方法，用于指定不管Promise对象最后状态如何，都会执行的操作，不管Promise最后的状态，在执行完then或者catch指定的回调以后都会执行finally方法。可以进行任何必要的清理工作

~~~js
let p=Promise.resolve('0 success');
p.then(
    res=>{
        console.log(res);
        return '1 success'
    }
)
.then(res=>{
    console.log(res);
    throw new Error('2 error')
})

.catch(err=>{
    console.log('catch',err);
})
.finally(()=>{
    console.log('p finally');
})
// 0 success
// 1 success
// catch Error: 2 error
// p finally
~~~

- #### Promise.all([...])

类方法，多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败。 。

~~~javascript
let p1 = Promise.resolve('p1 success');
let p2 = Promise.resolve('p2 success');
let p3 = Promise.reject('p3 error')
let arr = [p1, p2, p3]
Promise.all(arr)
    .then(
        res => {
            console.log(res);
        },
        err => {
            console.log(err);
        }
    )
    // p3 error

~~~

- #### Promise.race([...])

类方法，多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败。

~~~javascript
let p1 = Promise.resolve('p1 success');
let p2 = Promise.resolve('p2 success');
let p3 = Promise.reject('p3 error')
let arr = [p1, p2, p3]
Promise.race(arr)
    .then(
        res => {
            console.log(res);
        },
        err => {
            console.log(err);
        }
    )
    // p1 success
~~~

- #### Promise.any([...])

与all相似，但是会忽略拒绝，所以只需要完成一个而不是全部

```JavaScript
let p1 = Promise.reject('p1 error');
let p2 = Promise.resolve('p2 success');
let p3 = Promise.reject('p3 error')
let arr = [p1, p2, p3]
Promise.any(arr)
    .then(
        res => {
            console.log(res);
        },
        err => {
            console.log(err);
        }
    )
// p2 success
```

#### 变式

#### Promise.none([...])

类似于all不过完成和拒绝的情况互换，只有所有的函数都被拒绝，该函数转化为完成值，反之亦然

#### Promise.first([...])

类似于any的竞争，只要第一个Promise完成，会忽略后边任何的拒绝和完成

#### Promise.last([...])

类似于first，但是只有最后一个完成胜出