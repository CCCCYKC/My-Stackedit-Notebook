# 路由的配置和用法
>在 Vue 2 中，Vue Router 是用于构建单页面应用中路由的核心工具。
## 1. 一级、二级和多级路由
### （1）一级路由
一级路由是最基础的路由，它直接对应到一个组件
```javascript
const router = [
	{
	  path: '/home',
	  component: Home
	}
]
```
访问 `/home` 路径时，会渲染 `Home` 组件。
### （2）二级路由
是嵌套在一级路由下的子路由
```javascript
const  routes  = [
	{
		path:  "/",
		component:  Layout,
		children: [
			{
				path:  "/", // 首页
				name:  "home",
				component:  Home
			},
			{
				path:  "/produce", // 产品管理
				name:  "produce",
				component:  Produce,
			}
	}
]
```
访问 `/home` 时，会先渲染 `Layout` 组件，然后在 `Layout` 组件内部的 `<router-view>` 中渲染 `Home` 和 `Produce` 组件。
### （3）多级路由
>- 如何判断path是否要加 `/` ，当上一级不为 `/` 时，路径就不用加 `/` 。
>- 因为加上 `/` 的是从根目录出发的绝对路径
>- 在router里面不用加 `/` ，但是在菜单等需要跳转的地方就必须将完整路径写出来
```javascript
{
  path: '/admin',
  component: Admin,
  children: [
    {
      path: 'dashboard',
      component: Dashboard,
      children: [
        {
          path: 'stats',
          component: Stats
        }
      ]
    }
  ]
}
```

## 2. 路由的出口标签
- `<router-view>` 是一个特殊组件，用于渲染匹配到的路由对应的组件。在 Vue Router 中，每当路由匹配时，对应的组件会在 `<router-view>` 中渲染。
- 在父级路由对应的组件中使用 `<router-view>`，用于渲染其子路由的组件。
```html
<router-view></router-view>
```

## 3. (v-bind):default-active="$route.path"
这个语法通常用于导航栏或菜单组件中，结合 `v-bind`（简写为 `:`）来动态设置当前活动的路由。例如，在使用 `element-ui` 的 `<el-menu>` 组件时：
```javascript
<el-menu :default-active="$route.path">
  <el-menu-item index="/home">首页</el-menu-item>
  <el-menu-item index="/about">关于</el-menu-item>
</el-menu>
```
作用：`$route.path` 会获取当前路由的路径，将当前活动的菜单项高亮显示，在刷新时也保持当前路径

## 4. Vue.use(VueRouter)
这是 Vue.js 的插件机制。`Vue.use(VueRouter)` 用于全局安装 Vue Router 插件。安装后，Vue 实例会将路由实例混入自身，使得所有组件都可以通过 `this.$router` 访问路由实例，通过 `this.$route` 访问当前路由信息。

## 5. router 中组件的同步加载和异步加载

router 中组件有两种加载方式：
- **同步加载：** 直接在路由配置中导入组件并指定。这种方式简单直观，但当应用很大时，所有路由组件都会被打包在一起，导致初始加载时间变长。
```javascript
import Home from '@/views/Home.vue'
const routes = [
  {
    path: '/home',
    component: Home
  }
]
```
- **异步加载：** 使用 `() => import()` 的语法进行代码分割，按需加载组件。这种方式会将组件的代码分割到单独的文件中，只有当访问到对应的路由时，才会加载相应的组件，从而优化了应用的加载性能。
```javascript
const Home = () => import('@/views/Home.vue')
const routes = [
  {
    path: '/home',
    component: Home
  }
]
```
## 6. 动态路由匹配
动态路由允许你在路由的路径中包含动态段，使用冒号（`:`）来定义动态路由参数。
>动态路由和VueX的关系
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTM4MjUxNzExXX0=
-->