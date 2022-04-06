---
title: JsQuestions-1
date: 2021-07-23 20:44:59
tags:
- 前端篇
- JavaScript
---

### 

```javascript
// 输出什么？
const myFunc = ({ x, y, z }) => {
 console.log(x, y, z);
};

myFunc(1, 2, 3);
```

- A: `1` `2` `3`

- B: `{1: 1}` `{2: 2}` `{3: 3}`

- C: `{ 1: undefined }` `undefined` `undefined`

- D: `undefined` `undefined` `undefined`

   <!--more-->

#### 解析：

##### 答案: D

`myFunc` 期望接收一个包含 `x`, `y` 和 `z` 属性的对象作为它的参数。因为我们仅仅传递三个单独的数字值 (1, 2, 3) 而不是一个含有 `x`, `y` 和 `z` 属性的对象 ({x: 1, y: 2, z: 3})， `x`, `y` 和 `z` 有着各自的默认值 `undefined`.



-来源：Github - Javascript Questions