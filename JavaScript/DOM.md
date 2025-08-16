# DOM
## 一、获取元素
### （1）`document.querySelector(selector)`
根据CSS选择器获取第一个匹配的元素。`selector` 是选择器
示例：`document.querySelector(".box")`
### （2）`docement.getElementById`
根据 Id 获取唯一匹配的元素
示例：`document.getElementById("main-content")`
### （3）`docement.getElementsByTagName`
根据标签名获取所有匹配的元素，返回一个`HTMLCollection`。
示例：`document.getElementsByTagName("div")`
**`HTMLCollection` 对象是一个类数组对象，它具有索引和 `length` 属性，可以通过索引访问其中的元素**
```js
var divs = document.getElementsByTagName("div");
console.log(divs[0]); // 访问第一个 <div> 元素
console.log(divs.length); // 获取 <div> 元素的数量
```
### （4）`docement.getElementsByClassName`
根据类名获取所有匹配的元素，返回一个`HTMLCollection`。
 示例：`document.getElementsByClassName("box")`。
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDY5MjE3MDgzXX0=
-->