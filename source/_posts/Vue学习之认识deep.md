---
title: Vue学习之认识/deep/
date: 2021-08-05 23:38:41
tags:
  - vue
  - 随笔
categories: Vue
---

elementui使用中deep的用法/deep/

在做第一个vue项目时遇到了一个问题，在使用elementui中的Drawer（抽屉组件时），想要修改Drawer的标题，尝试着修改title的样式，但没有效果，尝试了很多方法，最后用/deep/解决了问题
通过深度选择 .el-drawer 中的title即可修改标题样式成功

```css
 /deep/ .el-drawer__header{
    color: white;
 }
```

在此之后可以用/deep/来修改elementui中一些其他组件的样式