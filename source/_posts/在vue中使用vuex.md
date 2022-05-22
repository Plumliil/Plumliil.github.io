---
title: 在vue中使用vuex
tags:
  - 前端篇
  - Vue
date: 2022-05-14 10:48:42
---


### vuex



#### 什么是状态管理



+ 在开发中,我们会的应用程序需要处理各种各样的数据,这些数据需要保存在我们应用程序中的某一位置,对于这些数据的管理我们称之为是状态管理

在前面是如何管理自己的状态

+ 在前面如何管理自己状态

 - 在Vue开发中,我们使用组件化开发模式

 - 而在组件中我们定义data或者setup中返回使用的数据,这些数据我们称之为state

 - 在模块template中我们可以使用这些数据,模块最终会被渲染成DOM,我们称之为view

 - 在模块中我们会产生一些行为时间,处理这些行为事件时,有可能会修改state,这些行为事件我们称之为actions


#### vuex是什么



vuex是一个专为vuejs应用程序开发的状态管理模式,它采用集中式管理应用的所有组件状态,并以相应的规则保证状态以一种可预测方式发生变化.vuex也集成到vue官方调试工具devtools extension,提供了诸如零配置time-travel调试,状态快照导入导出等高级调试功能

调试工具:devtools

> vuex就像是眼睛:您自会知道什么时候使用它



#### vuex的状态管理



+ 管理不断变化的state本身是非常困难的:

 - 状态之间相互会存在依赖, -个状态的变化会引起另一个状态的变化, View页面也有可能会引起状态的变化;

 - 当应用程序复杂时, state在什么时候,因为什么原因而发生了变化,发生了怎么样的变化,会变得非常难以控制和追踪;

+ 因此,我们是否可以考虑将组件的内部状态抽离出来,以一一个全局单例的方式来管理呢?

 - 在这种模式下,我们的组件树构成了一个巨大的“视图View” ;

 - 不管在树的哪个位置,任何组件都能获取状态或者触发行为;

 - 通过定义和隔离状态管理中的各个概念,并通过强制性的规则来维护试图和状态间的独立性,我们的代码边会变得更加结构化和易于维护、跟踪;



#### 单一状态树



+ vuex使用单一状态树:

 - 用一个对象就包含了全部的应用层级别状态

 - 这也意味着,每个应用将仅仅包含一个store实例

 - 单一状态树和模块化并不冲突

+ 单一状态树优势

 - 如果你的状态是保存到多个Store对象中,那么之后的管理和维护等等都会变得特别困难

 - 所以vuex也使用单一状态树来管理应用层级全部状态

 - 单一状态树能够让我们最直接的方式找到某个状态的片段,而且在之后的维护调试中,也可以非常方便的管理和维护



#### state 



在store中定义数据,在组件中直接使用

目录:store/index.js

~~~javascript

// store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
 state: {
  num: 0
 },
 mutations: {},
 actions: {},
 modules: {}
})

~~~

目录:Home.vue

~~~html

<template>
  <div class="home">
  <h2>Home页面的数字:{{$store.state.num}}</h2>
 </div>
</template>

<script>
export default {
}
</script>

~~~

或者写为

~~~html

<template>
  <div class="about">
  <h2>About页面的数字:{{num}}</h2>
 </div>
</template>

<script>
export default {
 computed:{
  num(){
   return this.$store.state.num;
  }
 }
}

</script>

~~~

相当于组件中的数据data,专门用来存放全局数据

更改state中数据唯一办法是提交mutations



#### getters

+ 某些属性我们可能需要经过变化后来使用,这个时候可以使用getters

+ getter第一个参数是state状态,第二个是getters,都可以帮助getters拿到想要的数据

getters相当于组件中的computed.区别是getters是全局的,computed是组件内部使用的在外部使用需要`$store.getters.getNums`

将组件中统一使用的computed,放到getters中使用

目录:store/index.js

~~~javascript

export default new Vuex.Store({
 state: {
  num: 0
 },
 getters:{
  getNum(state){
   return state.num;
  }
 },
 mutations: {},
 actions: {},
 modules: {}
})

~~~

目录:Home.vue

~~~html

<template>
  <div class="home">
  <h2>Home页面的数字:{{ $store.getters.getNum }}</h2>
 </div>
</template>

<script>
export default {}
</script>

~~~



#### mutations

更改store中state数据状态的唯一方法是提交mutation

mutations相当于组件中的methods,但是不能使用异步方法(定时器,axios...)

~~~javascript

export default new Vuex.Store({
 state: {
  num: 0
 },
 getters:{
  getNum(state){
   return state.num;
  }
 },

 mutations: {
  // state store中的state payload是一个形参,如果组件在commit时有传参,
  // 就存在,如果没有传参就为undefined
  increase(state,payload=1){
   state.num+=payload;
  }
 },
 actions: {},
 modules: {}
})

~~~

目录:Btn.vue

~~~html

<template>
  <div>
  <button @click="add">点击+1</button>
 </div>
</template>

<script>
export default {
 methods: {
   add(){
    // 提交mutation
    this.$store.commit('increase',3)
   }
 },
};

</script>

~~~

#### actions

actions是store中专门处理异步的,实际修改状态值的还是mutations

actions类似于mutation,不同在于:

 - actions提交的是mutation而不是直接改变状态

 - actions可以包含任意异步操作

这里有一个非常重要的参数context:

 - context是一个和store实例均有相同方法和属性的context对象

 - 我们可以从其中获取commit方法来提交一个mutation,或者通过context.state和context.getters来获取state和getters

 - 但是它为什么不是store对象,Modules

 - 可以对context进行解构

actions分发

- 使用store中的dispatch进行分发

目录:store/index.js

~~~javascript

export default new Vuex.Store({
 state: {
  num: 0
 },
 getters:{
  getNum(state){
   return state.num;
  }
 },
 mutations: {
  // state store中的state payload是一个形参,如果组件在commit时有传参,
  // 就存在,如果没有传参就为undefined
  increase(state,payload=1){
   state.num+=payload;
  },
  decrease(state){
   state.num--;
  }
 },
 // 专门处理异步,支持修改状态值的依然是mutations
 actions: {
  // 点击 -1 按钮 1s 后执行
  decreaseAsync(context){
   context.commit('decrease')
  }
 },
 modules: {}
})

~~~

目录:Btn.vue

~~~html

<template>
  <div>
  <button @click="$store.commit('increase', 3)">点击+1</button>
  <button @click="decrease">点击-1</button>
 </div>
</template>

<script>
export default {
 methods: {
  decrease() {
   setTimeout(() => {
    this.$store.dispatch("decreaseAsync");
   }, 1000);
  },
 },
};
</script>

~~~

#### map 辅助函数

mapState和mapGetters在组件中都是写在computed里

~~~html

<template>
  <h2>Home页面的数字:{{ num }}</h2>
  <h2>About页面的数字:{{getNum}}</h2>
</template>
<script>
import { mapState,mapGetters } from "vuex";
export default {
 computed:{
  ...mapState(['num']),
  ...mapGetters(['getNum'])
 }
};
</script>

~~~

mapMutations和mapActions在组件中都是写在methods里

~~~html

<template>
  <div>
  <button @click="increase(3)">点击+1</button>
  <button @click="decrease">点击-1</button>
 </div>
</template>

<script>
import { mapMutations,mapActions } from "vuex";
export default {
 methods: {
  decrease() {
   setTimeout(() => {
    this.decreaseAsync()
   }, 1000)
  },
  ...mapMutations(['increase']),
  ...mapActions(['decreaseAsync']),
 },

};

</script>

~~~



#### 拆分写法

store中的每个属性都可以差分成单独的文件

![](https://s2.loli.net/2022/05/05/cdWlF12SPTmQx6G.png)

~~~JavaScript

import Vue from 'vue'
import Vuex from 'vuex'

import state from './state';
import getters from './getters';
import mutations from './mutations';
import actions from './actions';
import modules from './modules';

Vue.use(Vuex)

export default new Vuex.Store({
 state,
 getters,
 mutations,
 actions,
 modules
})

~~~



#### modules



![](https://s2.loli.net/2022/05/05/spNQBKeX3F695zR.png)

+ 什么是Module ?

 - 由于使用单一状态树,应用的所有状态会集中到一个比较大的对象,当应用变得非常复杂时, store对象就有可

能变得相当臃肿;

 - 为了解决以上问题, Vuex允许我们将store分割成模块( module) ;

 - 每个模块拥有自己的state、mutation. action. getter、 甚至是嵌套子模块;

我们的store可以认为是一个主模块,它下边可以分解为很多子模块,子模块都可以单独分离出来写,写完再导入到主模块中.下面以`users`子模块:

users模块也可以分为state,getters,mutations和actions,所有的方法和属性该怎么写就怎么写.

但是users的index.js文件夹中,需要写入`namespace:true`命名空间.然后再store的index.js中引入到modules中.

在组件中获取值的方法:

~~~javascript

$store.state.users.nickName

~~~

在组件中触发mutations的方法:

~~~javascript

methods:{
  ...mapMutations({
    'changeNickName':'users/changeNickName'
  })
}
~~~



#### MUTATIONS_TYPE

用来将mutations所有的方法归纳起来.

目录:mutations_type.js

~~~javascript

export const MUTATIONS_TYPE = {
  INCREASE: 'increase',
  DECREASE: 'decrease'
}

export default {
  [MUTATIONS_TYPE.INCREASE](state, payload = 1) {
    state.num += payload;
  },
  [MUTATIONS_TYPE.DECREASE](state) {
    state.num--;
  }
}

~~~

目录:store/index.js

~~~javascript

...
import mutations from './mutations_type';
export default new Vuex.Store({
  ...
  mutations,
  ...
})

~~~

组件中

~~~html

<template>
  <div class="about">
  <h2>About页面的数字:{{$store.getters.getNum}}</h2>
  <button @click="increase()">About按钮.点击+1</button>
 </div>
</template>

<script>
import { mapGetters} from "vuex";
// import { mapGetters,mapMutations } from "vuex";
import {MUTATIONS_TYPE} from '../store/mutations_type'
export default {
 computed:{
  ...mapGetters(['getNum'])
 },
 methods: {
  // 方法一:
  // ...mapMutations([MUTATIONS_TYPE.INCREASE]),
  // 方法二:
  increase(){
   this.$store.commit(MUTATIONS_TYPE.INCREASE)
  },
 },
}
</script>
~~~



#### 在vue3中使用简单使用vuex



~~~html

<template>
  <div>
    <h1>{{ store.state.num }}</h1>
    <button @click="add">+1</button>
    <button>-1</button>
 </div>
</template>
<script setup>
import { useStore} from "vuex";
const store = new useStore();
const add = () => {
 store.commit("add");
 console.log(store.state.num);
};
</script>

~~~