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
>适用于以下场景：
>- 未登录时不能进入首页
>- 权限不同，能打开的界面不同
>- 填完问卷自动跳转

### 1. 全局前置守卫：未登录时不能进入
1. 配置路由元信息meta字段，标识哪些页面需要登录才能进入（可以只配给Layout路由，即除登录页的最高级）
设定字段（eg）：`meta: {isLogin: true}//需要登录才能进入`
2. 使用 `router.beforeEach` 注册一个全局前置守卫：	
```js
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // ...
})
```
每个守卫方法接收三个参数：

-   **`to: Route`**: 即将要进入的目标路由对象
    
-   **`from: Route`**: 当前导航正要离开的路由
    
-   **`next: Function`**: 一定要调用该方法来  **resolve**  这个钩子。执行效果依赖  `next`  方法的调用参数。
	 -   **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是  **confirmed**  (确认的)。
    
	 -  **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到  `from`  路由对应的地址。
    
	-  **`next('/')`  或者  `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向  `next`  传递任意位置对象，且允许设置诸如  `replace: true`、`name: 'home'`  之类的选项以及任何用在 `router-link`  的  `to`  prop 或 `router.push` 中的选项。


**实例：**
```js
// 配置路由全局前置守卫导航
import  store  from  "@/store"
router.beforeEach((to,from,next) => {
	// 判断进入的路由界面是否需要登录 不需要的则直接进入
	if(to.matched.some(item  =>  item.meta.isLogin)) {
		// 需要登录 判断是否已经登录 ==> token值是否存在
		let  token  =  store.state.login.userInfo.token;
		if(token) {
			next(); //跳转至to路由
		} else {
			next('/login'); //跳转至登录页
		}
	} else {
		// 不需要登录
		next();
	}
})
```
### 2. VueX + 路由全局前置守卫：权限不同，能打开的界面不同
> 1. 设置 vuex 的 menu 模块，定义空的菜单导航容器，定义获取导航和清空导航的方法
> 2. actions 定义请求动态路由接口的方法
> 3. 在路由全局前置守卫中请求动态菜单导航---->登录成功后，根据 token 获取动态菜单导航
> 4. 拆分原本写好的静态完整的菜单导航
> 5. 对比后台获取到的导航和前端的拆分出来的导航，匹配哪些是要渲染上去的
> 6. 渲染匹配好的菜单导航
> 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzOTcwMTMyNV19
-->