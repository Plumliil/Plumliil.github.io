---
title: 在vue中使用mixin
tags:
  - 前端篇
  - Vue
date: 2022-05-08 09:07:10
---


#### 什么是mixin

mixin的中文意思是混入,是一种编程思想,在vue中可以通过mixin来分发组件中的可复用功能,当某一个组件使用mixin对象时,该mixin对象的内容就会被混入到组件当中,供组件使用.**通过mixin我们可以将公共的方法或者数据抽离出来使用,简化代码**,和mixin类似的有vuex状态管理,但是vuex中的数据是所有使用vuex的组件通用的,在某一个组件中改变数据,其他组件中的相应数据也会发生改变,而mixin则不是这样,mixin对象在每个组件使用时互不干扰.



#### 在vue中使用mixin的两种方法

##### 基本使用

在js文件中导出一个对象,对象中包含需要混入的数据或方法,对象中可以包含在vue中使用的生命周期钩子等

~~~javascript
const mixin = {
    data() {
        return {}
    },
    computed: {},
    created() {},
    mounted() {},
    methods: {},
}
~~~

##### 局部混入:

方法会进行覆盖,但是生命周期函数会合并执行,先执行mixin中的生命周期函数内容再执行组件生命周期内容

一个组件中改变了mixin组件中的数据,另一个组件不受影响

~~~javascript
// mixin/index.js
export const myMixin = {
    data() {
        return {
            msg: '公共数据'
        }
    },
    computed: {},
    created() {
        console.log('公共created生命周期方法');
    },
    methods: {
        clickMe() {
            console.log("公共点击方法");
        },
    },
}
~~~

~~~vue
// A组件
<template>
  <div>
    <h2>A组件</h2>
    <button @click="clickMe">clickMe</button>
    <span>{{ msg }}</span>
  </div>
</template>

<script>
import { myMixin } from "../mixin/index";
export default {
  mixins: [myMixin],
  data() {
    return {
      value: "A组件特有",
    };
  },
  methods: {},
  created() {
    console.log("A组件生命周期");
  },
};
</script>
~~~

~~~vue
// B组件
<template>
    <div>
        <h2>B组件</h2>
        <button @click="clickMe">clickMe</button>
        <span>{{msg}}</span>
    </div>
</template>

<script>
import {myMixin} from '../mixin/index'
export default {
  mixins: [myMixin],
  data() {
    return {
      value: "B组件特有",
    };
  },
  methods: {
    clickMe() {
      console.log("B组件特有方法");
    },
  },
  created() {
    console.log("B组件生命周期");
  },
};
</script>
~~~

~~~vue
// App.vue
<template>
  <div>
    <cpm-a></cpm-a>
    <cpm-b></cpm-b>
  </div>
</template>

<script>
import cpmA from "./components/cpmA.vue";
import cpmB from "./components/cpmB.vue";
export default {
  components: { cpmA, cpmB },
};
</script>

~~~

![1651285652860.png](https://s2.loli.net/2022/04/30/H2RzLoZdfEX1uUj.png)



##### 全局混入:

在main.js中引用

~~~JavaScript
// main.js
import {myMixin} from './mixin'
Vue.mixin(myMixin)
~~~




> 谨慎使用全局混入,因为它会影响每个单独创建的vue实例包括第三方组件.大多数情况下之应当应用于自定义选项,就像上面的事例一样,推荐将其作为插件发布,以免避免重复应用混入.





#### mixin使用选项合并规则:

1.先执行mixin生命周期函数,再执行组件中生命周期函数.

2.如果data里面数据有冲突,优先使用组件中的数据.

3.当mixin中methods方法和组件方法重名时,优先使用组件方法



#### mixin优缺点:

##### 优点

1.提高代码复用性

2.无需传递状态

3.维护方便,只修改一个地方即可

##### 缺点:

1.命名冲突

2.滥用mixin后期不易维护

3.不好溯源,排查问题困难

4.不能轻易的重复代码



**vue官方mixin讲解**:[混入 — Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/mixins.html#基础)