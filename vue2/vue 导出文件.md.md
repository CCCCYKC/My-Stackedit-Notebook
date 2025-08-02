# VUE 导出文件
## 一、 VUE 导出 EXCEL 文件
>[vue导出excel表格（详细教程）_vue前端导出excel-CSDN博客](https://blog.csdn.net/m0_59023970/article/details/123427008?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522f0fd04bf5b4709599abbc22c6da9d1e2%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=f0fd04bf5b4709599abbc22c6da9d1e2&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-123427008-null-null.142^v102^pc_search_result_base8&utm_term=vue%20%E5%AF%BC%E5%87%BAexcel&spm=1018.2226.3001.4187)

### 1.安装
`npm install vue-json-excel -S
`

### 2.使用
#### （1）在文件 `main.js` 中引用
```js
import JsonExcel from 'vue-json-excel'
Vue.component('downloadExcel', JsonExcel)
```
#### （2）在代码中使用
```html
<template>
 <download-excel
   class="export-excel-wrapper"
   :data="DetailsForm"
   :fields="json_fields"
   :header="title"
   name="需要导出的表格名字.xls"
 >
<!-- 上面可以自定义自己的样式，还可以引用其他组件button -->
  <el-button type="success">导出</el-button>
 </download-excel>
</template>
```
#### （3）数据
-   DetailsForm：需要导出的数据   
-   title：表格标题
-   json_fields：里面的属性是excel表每一列的title，用多个词组组成的属性名(中间有空格的)要加双引号；指定接口的json内某些数据下载,若不指定，默认导出全部数据中心全部字段
```js
<script>
 title: "xx公司表格",
 json_fields: {
        "排查日期":'date',
        "整改隐患内容":'details',
        "整改措施":'measure',
        "整改时限":'timeLimit',
        "应急措施和预案":'plan',
        "整改责任人":'personInCharge',
        "填表人":'preparer',
        "整改资金":'fund',
        "整改完成情况":'complete',
        "备注":'remark',
      },
    DetailsForm: [
        {
          date: "2022-3-10",
          details: "卸油区过路灯损坏",
          measure: "更换灯泡",
          timeLimit: "2022-3-21",
          plan: "先使用充电灯代替,贴好安全提醒告示",
          personInCharge: "王xx",
          preparer: "王xx",
          fund: "20元",
          complete: "已完成整改",
          remark: "重新更换了灯泡",
        },    
      ],
</script>
```
## 二、VUE 导出 PDF
>- https://github.com/FranckFreiburger/vue-pdf
>- [vue-pdf: vue-pdf项目中使用PDF打印 --代码来源gitHub https://github.com/FranckFreiburger/vue-pdf](https://gitee.com/huojiefuren/vue-pdf)
### 1. 安装
``npm install vue-pdf``
### 2.使用
#### （1）在单文件中使用
```js
<template>
  <pdf src="./path/to/static/relativity.pdf"></pdf>
</template>

<script>
import pdf from 'vue-pdf'
export default {
  components: {
    pdf
  }
}
```
#### （3）数据
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDgzNjA3MzE5XX0=
-->