---
title: Vue学习之认识ref
date: 2021-08-04 23:24:11
tags:
  - vue
  - 随笔
categories: Vue
---

ref相当于一个dom节点，值为string

通俗将类似于原生js的document.querySelector('xxx')；但是不同的是vue是操纵虚拟dom，在渲染初期并没有这个属性，而是在创建vue实例后才加到虚拟dom中。

其中ref有以下几种常见用法：

本页面获取dom元素

```xml
 <div ref="divmsg">divmsg</div>
 <button @click="getmsg">div内元素</button>
```

```javascript
getmsg(){
	console.log(this.$ref.divmsg)
}
// 输出11111
```

<!--more-->

获取子组件中的data

子组件

```xml-dtd
<div>{{msg}}</div>
------------------
data(){
	return{
		msg:'hello world！'
	}
}
```

父组件

```xml-dtd
 <div id="app">
	<Child ref="child"></Child>
    <button @click="getChild">获取子组件中值</button>
  </div>
---------------------------------
import xxx from xxx
methods:{
	getchild(){
		console.log(this.$ref.child.msg);
	}
}
// 输出：hello world!
```

调用子组件中方法

子组件

```xml-dtd
<div></div>
----------------------
methods:{
	childMethod(){
		console.log('childMethod');
	}
}
```

父组件

```xml-dtd
 <div id="app">
	<Child ref="child"></Child>
    <button @click="getChild">调用子组件方法</button>
  </div>
---------------------------------
import xxx from xxx
methods:{
	getchild(){
		console.log(this.$ref.child.childMethod());
	}
}
// 输出：hello world!
```

其中$ref是一个对象，存放已注册过ref的所有子组件

通过`vue实例.$refs.属性名`即可获取标记的元素

如果ref重名后面的会把前面的覆盖

其中若元素使用v-for则返回dom节点数组