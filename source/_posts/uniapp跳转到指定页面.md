---
title: uniapp跳转到指定页面
tags:
  - 前端篇
  - uniapp
date: 2022-07-20 09:44:10
---






#### 在uniapp中访问外部链接由两种方法:

**第一**是通过webview,在需要跳转的页面通过`uni.navigateTo()`跳转,跳转时需要加上url地址,在webview界面需要接受url地址并且动态绑定给webview标签,这种方法是在app内部显示外部链接页面,具体代码如下所示:

~~~html
// 跳转前页面
<template>
	<view>
		<button @tap="go">跳转到外部</button>
	</view>
</template>

<script>
	export default {
		methods: {
			go(){
				let url= encodeURIComponent('https://www.baidu.com');
				uni.navigateTo({
					url: '../webView/webView?url=' + url,
				})
			}
		}
	}
</script>

~~~

~~~html
//webview页面
<template>
	<view>
		<web-view :src="url" :progress="true"></web-view>
	</view>
</template>
 
<script>
    export default {
        data() {
            return {
                url:'',
            }
        },
        onLoad(option) {
            this.url = decodeURIComponent(option.url);
        },
    }
</script>

~~~

> `web-view` 是一个 web 浏览器组件，可以用来承载网页的容器，会自动铺满整个页面（nvue 使用需要手动指定宽高）。

**第二种**方法是同步外部浏览器来进行链接的访问`plus.runtime.openURL(url);`具体代码如下:

~~~html
<template>
	<view>
		<button @tap="go">跳转到外部</button>
	</view>
</template>

<script>
	export default {
		methods: {
			go(){
				let url= 'https://www.baidu.com';
                  plus.runtime.openURL(url);
			}
		}
	}
</script>
~~~

如果是对页面进行简单的浏览推荐第一种方法,就用户体验来说比较友好