<div align=center>
<img src="calculator/src/assets/logo.png"/>
</div>

## VUE3.0 + VITE + ELEMENT + MD editor ##
### 初始化vite项目 ###
    npm init @vitejs/app my-vue-app --template vue
### 引入 Element Plus ###
    npm i element-plus
然后修改项目入口文件，引入 Element Plus 库和相关样式文件  
~~~json
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus'
import 'element-plus/lib/theme-chalk/index.css'
createApp(App).use(ElementPlus).mount('#app')
~~~
### 引入 makedown editor ###
[参考](https://blog.csdn.net/qq_24392199/article/details/116982316) 
#### 安装 ####
    npm i @kangc/v-md-editor@next -S
#### 引入组件 ####
~~~json
import { creatApp } from 'vue';
import VMdEditor from '@kangc/v-md-editor';
import '@kangc/v-md-editor/lib/style/base-editor.css';
import githubTheme from '@kangc/v-md-editor/lib/theme/github.js';
import '@kangc/v-md-editor/lib/theme/style/github.css';

VMdEditor.use(githubTheme);

const app = creatApp(/*...*/);

app.use(VMdEditor);
~~~
#### 基础用法 ####
~~~json
<template>
  <v-md-editor v-model="text" height="400px"></v-md-editor>
</template>

<script>
import { ref } from 'vue';

export default {
	setup () {
		const text = ref('');

		return {
			text
		}
	}
}
</script>
~~~
### 引入vue-router ###
[参考](https://zhuanlan.zhihu.com/p/365264520) 
#### 安装 ####
    npm install vue-router@4
#### 引入组件 ####
~~~json
import {createRouter, createWebHashHistory} from 'vue-router'
~~~
#### 配置路由 ####
~~~json
//定义路由（和组件）
const Home = ()=>import('./components/home.vue');
const About = ()=>import('./components/about.vue');
const Product = ()=>import('./components/product.vue');

// 定义路由
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
  { path: '/product', component: Product },
];


//创建路由
const router = createRouter({
  history: createWebHashHistory(),
  routes,//定义的路由
})


//将路由加入到实例
use(router)
~~~
#### 路由应用 ####
~~~http
<router-view> 组件显示容器
<router-link> 链接
~~~