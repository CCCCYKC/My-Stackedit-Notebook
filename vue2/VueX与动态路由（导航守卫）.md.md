# VueX与动态路由（导航守卫）
>[VueX与动态路由的关系 - Kimi](https://www.kimi.com/chat/d242skmmu6samfmc2l70)
## 一、VUEX 持久化
- 即仓库。可以将一些传递的数据但是中间隔着多个组件或者传递数据很复杂存储到仓库中。
存储格式如下：
```js
//文件src/store/index.js
import  Vue  from  'vue'
import  Vuex, { Store } from  'vuex'
import  product  from  './modules/product.js'
import  login  from  './modules/login.js'
import  createPersistedstate  from  'vuex-persistedstate'
Vue.use(Vuex)

const  store  =  new  Store({
	state: { // 数据状态
		isCollapse:  false, // 是否折叠侧边栏(默认不折叠)
	},
	mutations: { // 同步修改状态
		// 同步修改isCollapse状态
		changeCollapse(state,bool) {
			state.isCollapse  =  bool; // 修改侧边栏折叠状态
		}
	},
	actions: {}, // 异步操作
	modules: {
		product, // 产品模块
		login, //登录模块
	},
	plugins: [
		createPersistedstate({
			// storage: window.localStorage, // 使用本地存储(默认是localStorage)
			key:  'info', // 存储vuex数据的任意键名key--本地存储里面localStorage.info
			paths: ['product','login'], // 只持久化product模块的数据--存储的模块名称一级全局state数据 不写默认存储所有内容
	})]
})
export  default  store
```
```js
// 存储登录信息
//文件src/store/modules/login.js
export  default {
	namespaced:  true,
	state: {
		userInfo: {
			username:  '',
			token:  ''
		}
	},
	mutations: {
		// 添加用户信息
		setUser(status, payload) {
			status.userInfo  =  payload;
		},
		// 移除用户信息
		removeUser(status) {
			status.userInfo  = {
				username:  '',
				token:  ''
			}
		}
	},
	actions: {}
}
```
- state：存储全局状态，组件通过 this.$store.state 访问。
- mutations：同步修改 state 的唯一方式，通过 this.$store.commit 调用。
- actions：用于处理异步操作，通过 this.$store.dispatch 调用，最终通过 mutations 修改 state。

## 二、导航守卫
>用于一下场景：
>- 未登录时不能进入首页
>- 权限不同，能打开的界面不同

1. 配置路由元信息meta字段，标识哪些页面需要登录才能进入（可以只配给Layout路由，即除登录页的最高级）
设定字段（eg）：`meta: {isLogin: true}//需要登录才能进入,`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzODU5NzEwMV19
-->