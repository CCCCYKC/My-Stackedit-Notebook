# VUE 国际化
>国际化不是硬性翻译，而是已经准备好两份静态文字语言，对其进行切换
>1. vue 国际化 `vue-i18n`
>2. element-ui 国际化

## 一、vue 国际化 `vue-i18n`
### 1.安装
``npm install vue-i18n@8.x -S``

### 2. 使用
```js
//1.先对其进行声明(单文件、全局都这样引用)
import Vue from 'vue'
import VueI18n from 'vue-i18n'
Vue.use(VueI18n)

// 2.准备翻译的语言环境信息
const messages = {
  en: {
    message: {
      hello: 'hello world'
    }
  },
  ja: {
    message: {
      hello: 'こんにちは、世界'
    }
  }
}

// 3.通过选项创建 VueI18n 实例
const i18n = new VueI18n({
  locale: 'ja', // 设置地区
  messages, // 设置地区信息
})
// 4.通过 `i18n` 选项创建 Vue 实例
new Vue({ i18n }).$mount('#app')  //在main.js里面这样写
//如果单独拎一个文件写，则直接export 然后在全局import
export  default  i18n

```
### 3.在视图上使用
```html
<div id="app">
  <p>{{ $t("message.hello") }}</p>
</div>
```

## 二、element-ui 国际化（官网上有）
```js
// 导入element UI的国际化
// import Element from 'element-ui'
// 兼容写法(推荐)
import  ElementLocale  from  'element-ui/lib/locale'
import  enLocale  from  'element-ui/lib/locale/lang/en'
import  zhLocale  from  'element-ui/lib/locale/lang/zh-CN'
// 导入语言文件
import  Myzh  from  "./zh.js"
import  Myen  from  "./en.js"
// 2.准备翻译的语言环境信息
const  messages  = {
	zh: {
		...Myzh,
		...zhLocale  //导入element UI的中文
	},
	en: {
		...Myen,
		...enLocale
	},
}
// Vue.use(Element, {
// i18n: (key, value) => i18n.t(key, value)
// })
// 兼容写法(推荐)
ElementLocale.i18n((key, value) =>  i18n.t(key, value))
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzMyMTQ3MDNdfQ==
-->