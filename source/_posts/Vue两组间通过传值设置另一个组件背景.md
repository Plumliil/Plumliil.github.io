---
title: Vue两组间通过传值设置另一个组件背景
date: 2021-07-31 09:58:16
tags:
  - vue
  - 问题解决
categories: Vue
---

#### vue项目中，从一个组件获取图片url，并为另一个组件设置该图片为背景的盒子

##### 子组件

```vue
<template>
  <div 
       class="intd_content_banner" 
       :style="{background:'url('+banner.url+')'}">
  </div>
</template>
<script>
    export default {
      props:['banner'],
      name: "Banner",
    }
</script>
```

<!--more-->

##### 父组件

```vue
<template>
      <div id="intro">
        <Banner :banner="banner"></Banner>
      </div>
</template>

<script>
    import Banner from "./common/Banner";
    export default {
        name: "Intro",
        data(){
          return{
            banner:{
              title:'博音简介',
              url:'https://liangcheng-law.oss-cn-beijing.aliyuncs.com/static/images/lawyer_banner.jpg',
            }
          }
        },
        components: {Banner}
    }
</script>
```

最近在做vue项目时遇到的小问题，特此记录，下次遇到可以更好解决。（一）