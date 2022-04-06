---
title: 近期uniapp使用及总结
tags:
  - 前端篇
  - uniapp
  - 踩坑
date: 2022-03-31 22:33:02
---


​	吃什么饭对于有选择困难的我来说是个大问题,所以想做个根据自己输入的食物随机分配每餐吃的东西,然后就准备用uniapp做这样一个软件,主要是uniapp打包生成移动端app比较方便.然后就去看了一下uniapp,就像把这两天敲代码遇到的问题及心得总结一下.

​	总的来书uniapp的开发和vue的开发差不多,所以用起来也就很快的熟练了起来,但是有些组件以及api的调用还是不同

​	首先从uniapp新建项目开始吧,因为我要做的是uni-app所以就选择了默认的模板.然后来看一下

##### 	uniapp基本目录结构

```shell
│  App.vue
│  index.html
│  main.js
│  manifest.json
│  pages.json
│  uni.scss
├─commons
│  ├─css 
│  ├─imgs  
│  └─js
├─components
│  └─eatWhat
├─pages
│  └─index
├─static
└─util
```

​	**App.vue** 是uni-app的主组件，所有页面都是在 App.vue 下进行切换的，是**页面入口文件**,和vue中的.App.vue类似,也有些不同,uniapp的App.vue页面中没有tempate模板,而是只有script和style这两个标签,uniapp页面中公共的样式可以放在Vue.app这个页面的style中.**index.html**是运行之后生成的,所有的东西都会渲染到这上面,manifest.json进行一些基础的配置,**pages.json**中是配置一些页面信息,导航栏信息等**uni.scss**是一些公共样式的配置.在使用scss是可以方便的引用

​	文件夹中commons和components,util这三个文件夹是我自己创建的**commons**是放置一些公共的东西,css,js之类的文件,**components**主要放置一些组件,有vue开发经验的小伙伴应该对这个很熟悉.而**util**放一些封装的一些工具方法,**pages**中放的是所有的页面,**static**中放置静态资源.

​	说完了基本的目录结构就来说一下最近遇到的问题

##### 储存

​	吃点啥这个块用到最多的就是信息的存储与读取,我一开始想到的就是用storage来进行数据的存储,然后再给当日获取到的食物设置过期时间,超过12小时后清空然后就去查了下官方文档,发现只能简单的设置值,没法直接这设置过期时间,所以只能自己来设置.

下面就把代码贴出来:

~~~javascript
function cache(key, value, time = 3600 * 12) {
	let nowTime = Date.parse(new Date()) / 1000;
	if (key && value) {
		// 设置到期时间
		let expire = nowTime + Number(time);
		uni.setStorageSync(key, JSON.stringify(value) + '|' + expire);
	} else if (key && !value) {
		let val = uni.getStorageSync(key);
		if (val) {
			// 判断缓存是否过期
			let temp = val.split('|');
			if (!temp[1] || temp[1] <= nowTime) {
				uni.removeStorageSync(key);
				console.log(key + '缓存已失效');
				return '';
			} else {
				return JSON.parse(temp[0]);
			}
		}
	}
}
~~~

`uni.getStorageSync(key)`和`uni.setStorageSync(key,value)`是uniapp中同步设置和获取缓存的两个api,我们可以通过在设置值是添加一个设置值的时间,然后在获取值时进行判断,当设置时间-获取时间>过期时间时,缓存无效,通过`uni.removeStorageSync`移除缓存,以此方法来设置过期时间

##### 字体导入与使用

​	另一个问题是字体的导入,因为第一次在uniapp中设置字体,遇到了点困难,所以这个也需要记录下来.

​	字体导入首先是引入字体,到网上下载了心仪字体的ttf文件.放在了static的font文件夹里

然后又去公共的样式文件中设置了`global.css`如下:

~~~scss
// global.css
@font-face {
	font-family:YangRenDongZhuShiTi;
	src: url('~@/static/font/xxxxxx.ttf');
}
~~~

因为我的字体需要全局引用,所以又到App.vue中设置了一下公共页面的css最后完成字体的设置.

其他的都是一些js逻辑之类,相对较简单,以后遇到问题继续补充...