# 用vite搭建一个vue3项目
>[开始 | Vite 官方中文文档](https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project)

## 一、用命令行搭建
1. 搭建项目框架：`npm create vite@latest`
	- 起项目名
	- 选择VUE
	- 选择JavaScript
2. 安装依赖：`npm i`
3. 在文件 `package.json` 中：可看出运行项目的命令是 `npm run dev`
	- 可自定义运行单个文件的命令：如运行根目录下的 `index.js` 文件，可运行 `npm run sh`，则文件内容会输出到终端
```json
"scripts": {
	"dev": "vite",
	"build": "vite build",
	"preview": "vite preview"
	//可自由定义运行命令
	"sh": "node index.js"
},
```
4. 若启动失败，可能是因为 `node` 版本过低，使用 `nvm use 版本号` 选择版本较高的 `node` 即可

## 二、从头搭建项目
1. `npm init -y` 构建一个简单的包信息，即 `package.json` 文件.
	- 填写 `"description"`
	- `"scripts"` 为命令行
	- 设置 `"license": "MIT"`，并右键生成证书，在证书文件 `LICENSE` 中修改 `name` 
2. `npm i vite -D` 生成开发环境下的依赖
3. `npm i vue` 安装 vue（版本号默认为 vue3）
4. 在根目录下面新建文件 `index.html`，这是整个项目的出口文件
	- 在文件里面输入 `html:5` ，回车
	- 并在 `<body></body>` 里面输入 `<script  type="module"  src="./src/main.js"></script>`
	- 同时在根目录下面新建 `src/main.js`，先随便 `console.log` 一些东西
5. 在 `package.json` 里面的  `"scripts"` 里面，设置
```json
"scripts": {
	"dev": "vite"
},
```
6. 最后 `npm run dev`

**项目能够跑起来后，可以开始搭建项目结构**


 1. 在 `src` 下面新建文件 `App.vue`
 2. 修改 `main.js` 文件：
```js
import { createApp } from  "vue";
import  App  from  "./App.vue";
const  app  =  createApp(App);  
app.mount('#app');
```
3. 在根目录下面的 `index.js` 里面添加挂载点
```html
<body>
	//新添加的
	<div  id="app"></div>
	<script  type="module"  src="./src/main.js"></script>
</body>
```
4. **然后就会报错**，此时就要安装新插件 `npm i @vitejs/plugin-vue -D`
5. 在跟目录下面新建文件 `vite.config.js` **（因为浏览器其实是不支持 vue 文件解析的，所以要引用插件）**
```js
import { defineConfig } from  "vite"; 
import  Vue  from  "@vitejs/plugin-vue";
export  default  defineConfig({
	plugins:[Vue()]
})
```
**此时再 `npm run dev` 运行，就不会报错了**

- 补充：
在 `package.json` 文件里面的 `"scripts"` 添加 `"bulid": "vite bulid"` ,在终端输入命令 `npm run build`，就会生成 `dist` 文件，里面是打包好的项目

## 三、安装 Vue-Router
1. `npm i vue-router -D` 安装
2. 新建文件 `src/modules/router.js`
```js
import { createRouter,createWebHistory } from  "vue-router";  
const routes = [
	{
		name:'home',
		component: () => import('../views/home/home.vue')
	},
	{
		name:'Produce',
		path:'/produce',
		compoment: () => import('../views/produce/produce.vue')
	}
]
const  router  =  createRouter({
	routes,
	history:createWebHistory()
});  
export  default  router;
``` 
3. 在文件 `main.js` 中引入 `router` 并挂载
```js
import  Router  from  "./modules/router.js"
app.use(Router);
```
4. 新建页面文件 `src/views/home/home.vue`
5. 修改文件 `App.vue`，在里面放置路由出口
 ```html
<template>
	<router-view></router-view>
</template>
```

## 四、安装 Pinia
1. `npm i pinia -D` 安装
2. 新建文件 `src/modules/pinia.js` 
```js
import { createPinia } from  "pinia";
const  pinia  =  createPinia();
export  default  pinia;
```
3. 更新 `main.js` 文件
```js
import  pinia  from  "./modules/pinia.js"
app.use(pinia);
```
4. 新建存放全局状态的文件夹 `src/store`，在里面可以新建状态 `homeCount.js`
	- `defineStore()` 第一个参数是字符串形式的ID，第二个参数是带有 `state`、`actions` 与 `getters` 属性的对象
	- Pinia 与 VueX 不同，Pinia没有 `mutations` 属性
```js
import { defineStore } from  "pinia"
export const useCounterStore = defineStore('counter', {
  state() {
		return {
			count:  1,
		}
	},
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```
5. 引用时，例如在 `home.vue` 文件中
	- Pinia 比 VueX 使用更加简洁
	- 在调用状态、方法时，不再使用 `this.$store.state.count`，而是直接定义该仓库为一个变量，直接调用变量的状态或方法。例如 `counter.count`、`counter.increment()`
```html
<script  setup>
import { useCounterStore } from  "../../store/homeCount.js";
const  counter  =  useCounterStore();
</script>

<template>
	<div>
		<h2>首页界面</h2>
		<h3 @click="counter.increment()">点击这里{{  counter.count  }}</h3>
	</div>
</template>

<style></style>
```

## 五、安装 `unplugin-vue-components` 插件
>当组件过多时，可以根据页面使用的组件自己按需引用
>自定义组件、外部组件库、

1. `npm i unplugin-vue-components -D` 安装
2. 修改文件 `vite.config.js` 为
```js
import { defineConfig } from  "vite";
import  Vue  from  "@vitejs/plugin-vue"
import  Components  from  'unplugin-vue-components/vite'

export  default  defineConfig({
	plugins:[Vue(),Components()]
})
```
3. 使用该插件后，使用自定义组件时可以不用 import 导入，直接使用即可
---
4. 对于外部组件库，例如 `Element UI`、`Element Plus` 这些，也可以做到按需引入
	- 改写 `vite.config.js` 文件为
	-  安装 element plus 组件库 `npm i element-plus -D`
	- 然后就可以直接在文件里面使用组件了
```js
import { defineConfig } from  "vite";
import  Vue  from  "@vitejs/plugin-vue"
import  Components  from  'unplugin-vue-components/vite'
import {
	ElementPlusResolver,
	VantResolver,
} from  'unplugin-vue-components/resolvers'

export default defineConfig({
	plugins:[Vue(),Components({
		resolvers:[ElementPlusResolver(),VantResolver()]
	})]
})
```
## 六、安装 `unplugin-auto-import` 插件
> 可以省去 Vue 的一些方法、生命周期的引入。例如：
> - import { computed, ref } from 'vue'
> - 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNjA0MzMyMTgsLTcyODMzOTQ1MywxNj
I4MTk1MTEyLC0xNzM5Njk4MTg2LC0yMDI5NzMxNTg5LC01MjIw
NTEwODEsNDA0Mzg1MzUwXX0=
-->