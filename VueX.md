# VueX
> Vue2---->VueX3
> Vue3---->VueX4

## 1. Vue3
>六个属性：State Getters Mutations Actions Modules Plugins
>前四个都可以通过 `this.$store.属性名` 访问其中的内容

**`store` 文件使用模版：**
```js
import  Vue  from  'vue'
import  Vuex, { Store } from  'vuex'
import myPlugin from '@/....'
const store = new Store({
	state:{},
	getters:{},
	mutations:{},
	actions:{},
	modules:{},
	plugins: [myPlugin]
})
```
- 只要在 `main.js` 文件中将 `store` 实例注入全局，则可以在所有组件以 `this.$store` 的形式调用VueX
```js
const app = new Vue({
	el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
})
```

### （1）state 
存储全局状态（即静态变量），组件通过 `this.$store.state` 访问。只能在 `mutations` 中进行修改
- 当组件是要获取 `store` 模块中的状态时，此时写 `this.$store.state.模块名.变量名` 会非常笼余，可以使用 `mapState`  辅助函数，将 `store` 模块中的状态存储在 `computed` 里面
```js
//在组件中
import { mapState } from  "vuex";
export default {
	computed: {
		...mapState("login", ["userInfo"]), //保存了仓库的用户信息
	}
}
//html中可以直接使用该变量
<h2>{{ userInfo.username }}</h2>
```
### （2）getters 
相当于 `store` 的计算属性。可将一些常用的对 `state` 状态的变体储存在这里。调用时可使用 `this.$store.getters`
- `getters` 可以接受两个变量 `(state, getters)`
- `getters` 的返回值也可以是一个需要传递参数的函数，通过 `this.$store.getters.getTodoById(2)` 来调用
```js
const store = new Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: (state) => {		//基础的
      return state.todos.filter(todo => todo.done)
    },
		doneTodosCount: (state, getters) => {		//同时调用其他getters
	    return getters.doneTodos.length
	  },
		getTodoById: (state) => (id) => {		//返回值为一个需要传递参数的函数
	    return state.todos.find(todo => todo.id === id)
	  }
  }
})
```
- **`mapGetters`  辅助函数**
```js
import { mapGetters } from 'vuex'
export default {
  computed: {
		// 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
	    'doneTodosCount',
			getTodoById: 'getTodo'		//可以将传递的getter属性改名
		])
  }
}
```
### （3）mutations 
更改 store 中的状态的唯一方法是提交 `mutation`。 调用时可使用 `this.$store.commit('mutation名')` 进行调用
- `mutation` 非常类似于函数（非异步），必须是**同步函数**！！
- `mutation` 可以接受两个变量 `(state, payload)`，其中 `state` 是状态，`payload` 为一个对象，是接受参数的集合。 **要事先设定好 `payload` 中有哪些键值对，在传参是key名要相同** 
```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
//调用时
this.$store.commit('increment', { amount: 10 })
```
- **`mapMutations `  辅助函数**
```js
import { mapMutations } from 'vuex'
export default {
	methods: {
		// 若mutations是在模块当中，则`...mapMutations("模块名",['increment'])`
		...mapMutations(['increment']),
		login() {
			this.increment();  //调用
		}
	}
}
```

### （4）actions 
Action 类似于 mutation，调用时可使用 `this.$store.dispatch('menu/getMenuList');` 进行调用，不同在于：
-  Action 提交的是 mutation，而不是直接变更状态。
-  Action 可以包含任意异步操作。
- Action 函数可以接受一个与 store 实例具有相同方法和属性的 context 对象。`context` 对象包括 `{ commit, state, getters, dispatch, rootState }`；还可以接受第二个变量 `payload`。
	- `rootState` 是指在根文件的变量，应该还有 `rootCommit/rootDispatch` 等
```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {		//或者 increment ({ commit })
      context.commit('increment');		//调用mutations
    }
  }
})
```
- **`mapActions` 辅助函数**
```js
import { mapActions } from 'vuex'
export default {
  methods: {
    ...mapActions(['increment','incrementBy']),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```
### （5）modules 
由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割
### （6）plugins 

<!--stackedit_data:
eyJoaXN0b3J5IjpbNzM3Njk3NjUyXX0=
-->