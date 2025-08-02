# 信息传递的方式
## 1. props（父组件 --> 子组件）
>从父组件通过 **props** 将动态数据传递给子组件。
### （1）子组件中要声明要接受哪些props
```html
// 在子组件中
<script>
export default {
  props: {
    title: String, // 接收一个字符串类型的 title 属性
    count: {
      type: Number,
      default: 0 // 默认值为 0
    }
  }
}
</script>
```
### （2）父组件上绑定传递数据
通常使用 `v-bind` 来绑定传递给子组件的数据
```html
//在父组件中
<template>
  <ChildComponent :title="articleTitle" :count="articleCount" />
</template>

<script>
import ChildComponent from './ChildComponent.vue'
export default {
  components: { ChildComponent },
  data() {
    return {
      articleTitle: 'Vue.js 教程',
      articleCount: 42
    }
  }
}
</script>
```
### （3）特点
- 单项数据流：只能从父组件传递给子组件
- 命名统一性：在声明和绑定数据时，变量的名称必须一致
- 类型检查：可以在 `props` 设定数据类型
- 默认值：可以在 `props` 设定默认值
## 2. emits（子组件 --> 父组件）
>从子组件通过 **emits** 触发事件，从而通知父组件某些操作已发生，父组件可以监听这些事件从而来进行响应。
### （1）子组件中声明并且触发事件
子组件中在 `emits` 中声明时间，然后用 `$emits` 方法触发事件
`this.$emit()` 的第一个参数是事件（函数）的名称。其他所有参数都是事件（函数）的参数
```html
//在子组件中
<template>
  <button @click="handleClick">点击我</button>
</template>

<script>
export default {
  emits: ['update'], // 声明可以触发的事件
  methods: {
    handleClick() {
      this.$emit('update', 新的数据) // 触发事件并传递数据
    }
  }
}
</script>
```
### （2）父组件用 `v-on` 监听子组件的事件
```html
//在父组件中
<template>
  <ChildComponent @update="handleUpdate" />
</template>
<script>
import ChildComponent from './ChildComponent.vue'
export default {
  components: { ChildComponent },
  methods: {
    handleUpdate(data) {
      console.log('接收到子组件传递的数据：', data)
    }
  }
}
</script>
```
### （3）特点
- 反向通信：数据从子组件传递给父组件
- 自定义事件名称：子组件的事件名=父组件的事件名（类似于 `@click` ），父组件的函数名可自定义
- 数据类型自由：可传递任意类型的数据
## 3. 插槽 `slots`（父组件 --> 子组件）
>插槽是 Vue.js 中用于内容分发的机制。即允许父组件更改子组件在 **`<slot></slot>`** 位置内的内容。
### （1）在子组件内定义插槽
```html
<template>
  <div>
    <h1>{{ title }}</h1>
    <slot name="slot1">默认内容</slot> <!-- 如果父组件没有提供内容，则显示默认内容 -->
  </div>
</template>

<script>
export default {
  props: {
    title: String
  }
}
</script>
```
### （2）在父组件内填充插槽
在父组件中，通过在子组件标签内添加内容来填充插槽。
**填充内容可以是大段的html**
```html
<template>
  <ChildComponent title="Vue.js 教程">
	  <template slot="slot1">
	    <p>这是父组件提供的内容</p>
    <template/>
  </ChildComponent>
</template>

<script>
import ChildComponent from './ChildComponent.vue'
export default {
  components: { ChildComponent }
}
</script>
```
填充的插槽是 **标记语言**
### （3）特点
-   内容自定义：父组件可以自定义子组件的显示内容。   
-   默认内容：可以为插槽提供默认内容，当父组件没有提供内容时显示。
-   作用域插槽：可以将子组件的数据传递给父组件的插槽内容，实现更灵活的内容定制。

### （4）补充 element-ui 的 `＜template  [slot-scope]=“scope“＞`
>[slot插槽及Element-ui 中＜template slot-scope=“scope“＞_<template slot-scope="scope">-CSDN博客](https://blog.csdn.net/m0_63005501/article/details/128148252?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522b48bb2717fc5182d6898072da43ec519%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=b48bb2717fc5182d6898072da43ec519&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-5-128148252-null-null.142^v102^pc_search_result_base8&utm_term=template%20slot-scope&spm=1018.2226.3001.4187)

常用于表格 **el-table** 中
```js
// props可以绑定data(gdglList)中的各个属性
 <el-table v-loading="loading" :data="gdglList" @selection-change="handleSelectionChange">
	 <el-table-column label="开始时间" align="center" prop="startDatetime" width="180">
    <template slot-scope="scope">
     <span>{{ moment(scope.row.startDatetime).format(YYYY-MM-DD HH:mm:ss) }}</span>
    </template>
   </el-table-column>
   <el-table-column label="预计工时" align="center" prop="estimatedWorkingTime" />
   <el-table-column label="状态" align="center" prop="state">
     <template scope="scope">
			<div :style="{'color':scope.row.state==0? 'red':scope.row.state==1? 'orange':scope.row.state==2? 'green':'#333'}">
				{{scope.row.state|stateTrans}}
			</div>
		</template>
   </el-table-column>
   <el-table-column  label="商品描述"  align="center"  show-overflow-tooltip>
		<template  slot-scope="scope">{{ removeHTMLTag(scope.row.descs) }}</template>
   </el-table-column>
   <el-table-column label="结束时间" align="center" prop="endDatetime" width="180">
    <template slot-scope="scope">
      <span>{{ parseTime(scope.row.endDatetime, '{y}-{m}-{d}') }}</span>
     </template>
  </el-table-column>
</el-table>
```
- ``slot-scope``可以获取到 row（行）, column（列）, $index（拿到当前行的index） 和 store（table 内部的状态管理）的数据
- 我们可以理解为：tableData是给到table的记录集，scope是table内部基于tableData数据生成出来的数据集合（相似且有区别于整个数据）
- scope.row：我们就可以读取到每一行的集合。



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMTYxMzg5NzVdfQ==
-->