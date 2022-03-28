---
title: 'vue报错解决Duplicate keys detected:[object Object]'
date: 2021-08-16 10:54:37
tags:
  - vue
  - 问题解决
categories: Vue
---

​	最近在做vue项目时遇到了报错

​		*Duplicate keys detected: ‘[object Object]’. This may cause an update error.*

​	由于这个问题是第一次遇见，所以在解决起来废了点时间

​	![1](E:\Blog_Github\Plumliil\source\_posts\vue报错解决Duplicate keys detected[object Object]\1.png)

<!--more-->

解决：在遍历数组时   :key书写错误，

​	我的错误写法

```vue
   		<ul>
          <li v-for="v in caseintrocon" :key="i">
              <p><em>{{v.data_m_d}}</em>{{v.data_y}}</p>
              <p><strong>{{v.title}}</strong></p>
              <p>{{v.text_con}}</p>
          </li>
        </ul>
```

改正后的写法：

​	

```vue
		<ul>
          <li v-for="(v,i) in caseintrocon" :key="i">
              <p><em>{{v.data_m_d}}</em>{{v.data_y}}</p>
              <p><strong>{{v.title}}</strong></p>
              <p>{{v.text_con}}</p>
          </li>
        </ul>
```

总结：平时用修改前的方式写代码也没有出错，这次代码出错给了我个提醒，以后书写代码要更加规范