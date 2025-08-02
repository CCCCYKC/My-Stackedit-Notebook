# 创建VUE2项目的步骤

### 1. 创建vue项目
#### （1）``vue create vue-project``
第一个问题是问是否用镜像网来加速安装，选择`Y`
然后选择时vue2还是vue3
PS：
> -  通过方向键选择 Manually select features（手动选择特性）
 >- 用空格键勾选：
      [*] Router       # 生成 router/index.js
      [*] Vuex         # 生成 store/index.js
 >-  选择 Vue 2 版本
> -  完成创建后，src 目录下将出现 router 和 store


### 2. 引入element组件库
#### （2）``vue add element``
- **全部引用**：选择第二个
- **按需引入**：选择最后一个`Import in demand`
PS：按需引入需要在文件`src\plugins\element.js`里面根据所需要的组件引入想要的组件
>根据 [组件 | Element 网址](https://element.eleme.cn/#/zh-CN/component/quickstart) 网址的“完整组件列表和引入方式（完整组件列表以 components.json为准）”的以下内容来引入

#### （3）``c``+``npm install babel-plugin-component -D``
>加上`c`代表是从镜像网下载安装的，速度比较快，但是要安装`cnpm`
>未加`c`就是直接用`npm`下载，但是速度比较慢

安装完后在`babel.config.js`文件里面加上下面的配置
```javascript
"plugins": [
	[
		"component",
		{
			"libraryName":  "element-ui",
			"styleLibraryName":  "theme-chalk"
		}
	]
]
```
### 3. 安装需要的库
#### （4）`npm i axios -S`
>是一个基于 Promise 的 HTTP 客户端库，用于浏览器和 Node.js 环境。它主要用于发送 HTTP 请求（如 GET、POST、PUT、DELETE 等）
-  **替代原生 fetch**：提供更简洁的 API 和更强大的功能
    
-  **统一请求管理**：方便添加全局配置（如 baseURL, timeout）

#### （5）`npm i querystring -S`
>用途：处理 URL 查询字符串的解析和序列化

#### （6）`npm install normalize.css --save`
>用途：数据标准化/规范化（不是单一库，而是一类处理）

#### （7）`npm install echarts --save`
>图表库   [Apache ECharts](https://echarts.apache.org/zh/index.html)

用的时候再去装，可以根据需求生成需求包再下载



### 4. 运行
#### （8）`npm run serve`

### 5. 项目初始化
（1）删除一些无用的组件
```
src\components\HelloWorld.vue
src\views\AboutView.vue
src\views\HomeView.vue
App.vue中的<style>删除，<template>中只保留一个div即可，<script>中删除引入的被删除的组件
```
如果`src`文件夹中没有`router`文件夹，则在`src`中新建`router\index.js`，内容为：
```javascript
import  Vue  from  "vue";
import  VueRouter  from  "vue-router";

Vue.use(VueRouter);

const  routes  = [

]

const  router  =  new  VueRouter({
	mode:  "history",
	base:  process.env.BASE_URL,
	routes
});

export  default  router;
```
如果`src`文件夹中没有`store`文件夹，则在`src`中新建`store\index.js`，内容为：**自己去问AI**
```javascript
import  Vue  from  'vue'
import  Vuex, { Store } from  'vuex'

Vue.use(Vuex)

const  store  =  new  Vuex.Store({
	state: {}, // 数据状态
	mutations: {}, // 同步修改状态
	actions: {}, // 异步操作
	modules: {} // 模块化
})
  
export  default  store
```

（2）css初始化

- 在`main.js`文件里面添加`import 'normalize.css/normalize.css'`
- 在`assets`文件夹里面新建文件`css\base.css`，设置默认的css
- 在`main.js`文件里面添加`import '@\assets\css\base.css'`
- 引入icon图表库，在`base.css`中设置`@import url('网址')`
>[iconfont-阿里巴巴矢量图标库](https://www.iconfont.cn/)

**示例：**

```css
@import url('https://www.iconfont.cn/collections/detail?cid=9402')
h1,h2,h3,h4,p,ul,li{
	margin: 0;
	padding: 0;
}
ul{
	list-style: none;
}
a{
	text-decoration: none;
}
img{
	vertical-align: middle;
}
body{
	font-size: 14px;
	font-family: '微软雅黑', 'Arial Narrow', Arial, sans-serif;
}
```
引用图标
```html
<div>
	<span class="iconfont check-circle"></span>
</div>
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NTgzNDEwOTZdfQ==
-->