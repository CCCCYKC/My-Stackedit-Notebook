# v-____等语法（数据事件绑定+渲染）
## 1. `v-bind` 属性绑定
>在vue中，用`{{ }}`可以绑定文本插值。使用 `v-bind` 可以绑定动态属性。即将已有的值赋予要绑定的对象

- **语法：**``<div v-bind:id="dynamicId"></div>``
冒号后面的部分 (`:id`) 是指令的“参数”。此处，元素的 `id` attribute 将与组件状态里的 `dynamicId` 属性保持同步。
- **简写：**``<div :id="dynamicId"></div>``
- **示例：**
```html
<script>
export default {
  data() {
    return {
      titleClass: 'title'
    }
  }
}
</script>

<template>
  <h1 :class="titleClass">Make me red</h1>
</template>

<style>
.title {
  color: red;
}
</style>
```

## 2. `v-on` 事件监听
>可以使用 `v-on` 来监听DOM事件。即让绑定的对象影响已有的变量

**语法：**`<button v-on:click="increment">{{ count }}</button>`
**简写：**`<button @click="increment">{{ count }}</button>`
**示例：**
```html
<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```
>事件修饰符：[事件处理 | Vue.js](https://cn.vuejs.org/guide/essentials/event-handling#event-modifiers)
>`:click.stop` 等，进一步指出事件是如何进行的


## 3. `v-model` 表单绑定
>- 结合 `v-bind` 和 `v-on` 来在表单的输入元素上创建双向绑定。
>- 即可以不用绑定两次，只用一个 `v-__` 语法就可以完成一个变量 `value` 对标签的赋值以及被改变

**语法：**`<input v-model="text">`
`input` 中输入的值改变了变量 `text` ，变量 `text` 的值也显示在 `input` 中。
**示例：**
```html
<script>
export default {
  data() {
    return {
      text: ''
    }
  }
}
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

## 4. `v-if` `v-else` 条件渲染
>可以使用 `v-if` 和 `v-else` 指令来有条件地渲染元素

**示例：**
```html
<script>
export default {
  data() {
    return {
      awesome: true
    }
  },
  methods: {
    toggle() {
      this.awesome = !this.awesome
    }
  }
}
</script>

<template>
  <button @click="toggle">Toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
```

## 5. `v-for` 列表渲染
>- 使用 `v-for` 指令来渲染一个数组列表
>- 数组中必须每个列表都要有唯一的ID来绑定 `:key`

**示例：**
```html
<script>
// 给每个 todo 对象一个唯一的 id
let id = 0
export default {
  data() {
    return {
      newTodo: '',
      todos: [
        { id: id++, text: 'Learn HTML' },
        { id: id++, text: 'Learn JavaScript' },
        { id: id++, text: 'Learn Vue' }
      ]
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
```

**补充：更新数据列表的两种方法**
- **在源数组上调用变更方法：**`this.todos.push(newTodo)`
还有`shitf` `unshitf` 等等
- **使用新的数组替代原数组：**`this.todos = this.todos.filter(/* 条件 */)`
满足条件的留下形成新数组


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTYzNjA1ODAwXX0=
-->