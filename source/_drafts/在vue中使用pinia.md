## Pinia

- 直观,像定义components一样定义store
- 完整的typescripy支持
- 去除mutation,只有state,getters,actions
- actions支持同步和异步
- Vue Devtools支持pinia,提供更好的开发体验
- 能够构建多个stores,并实现自动代码拆分
- 轻量(1kb)



简单使用
#### 导入

##### 安装
`npm install pinia -g`

##### 使用
在main.js中
~~~javascrit
import { createApp } from 'vue'
import App from './App.vue'
// 导入
import { createPinia } from "pinia";

const app = createApp(App);
const pinia = createPinia();
app.use(pinia);
app.mount('#app');
~~~
在store/index.js中
~~~javascript
import { defineStore } from "pinia";

const useStore = defineStore('main', {
    state: () => {
        return {
            count: 10,
            list: [
                {name:'apple',color:'red'},
                {name:'banana',color:'yellow'}
            ]
        }
    }
})

export default useStore
~~~