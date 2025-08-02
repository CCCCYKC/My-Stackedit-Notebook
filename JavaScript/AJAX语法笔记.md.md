# AJAX 语法
>[AJAX 教程 | 菜鸟教程](https://www.runoob.com/ajax/ajax-tutorial.html)

## 1. AJAX 是用来干什么的？
AJAX = 异步 JavaScript + XML

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。

## 2. AJAX的使用方法

### （1）XMLHttpRequest对象
XMLHttpRequest用于在后台与服务器进行数据交换。即可以在不重新加载整个网页的情况下，通过获取后端的数据然后对网页的某部分进行更新。
- 浏览器几乎都使用XMLHttpRequest对象，只有IE5/IE6使用ActiveXObject对象（要对其进行**适配化处理**）
```javascript
let xmlhttp;
if(window.XMLHttpRequest) {
	//若浏览器支持XMLHttpRequest对象，则执行
	xmlhttp = new XMLHttpRequest();
} else {
	//若浏览器不支持XMLHttpRequest对象，则使用ActiveXObject来建立一个兼容的XMLHTTP对象
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
};
```
### （2）XMLHttpRequest的方法
#### 1）open( )
open 方法用于初始化一个**新的请求**
``xmlhttp.open(method, url, async, user, password);``
- method：请求的类型；GET/POST
- url：文件在服务器上的位置
- async：true（异步）或 false（同步）
- user & password：可省参数（一般都省略），用于HTTP认证
```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', '/try/ajax/getcustomer.php?q=1', true);
```
#### 2）send( )
send 方法用于**发送请求**
``xmlhttp.send(data);``
- data：要发送的数据。对于**GET请求**，通常传入null；对于**POST请求**，通常传入一个字符串或FromData对象。
#### 示例
- GET请求：
```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', '/try/ajax/getcustomer.php?q=1', true);
xhr.send(null);
```
- POST请求：
```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', /try/ajax/getcustomer.php', true);
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send('q=1');
```
>- setRequestHeader 是 XMLHttpRequest 对象的一个方法，用于设置HTTP请求的头部信息。通过这个，你可以设置在发送请求之前向服务器发送额外的元数据，例如认证信息、内容类型等。
#### 3）readyStatus 和 status
>详细教程：[AJAX – onreadystatechange 事件 | 菜鸟教程](https://www.runoob.com/ajax/ajax-xmlhttprequest-onreadystatechange.html)
- readyState 表示 XMLHttpRequest 请求的状态
- status 表示 HTTP 状态码
- status 以2开头的都是成功的状态

#### 4）onreadystatuechange 
>- 常用于异步请求
>- onreadystatuechange 是传统的处理方法，在如今更常使用 `Promise` 和 `async/await`，例如使用 `fetch` API。`fetch` 提供了更简洁、强大的功能，代码可读性也更好。

onreadystatechange 是 `XMLHttpRequest` 对象的一个事件处理器属性，用于监听 `readyState` 属性的变化。每当 `readyState` 的值发生变化时，onreadystatechange 事件处理器会被触发。
通过监听 `readyState` 的变化，你可以处理请求的各个阶段，例如请求发送成功、服务器响应返回等。
```javascript
let xhr = new XMLHttpRequest();

//监听readyState的变化
xhr.onreadystatechange = function() {
	if(xhr.readyState == 4) {//请求已完成
		if(xhr.status == 200) {//请求成功
			console.log('Response:', xhr.responseText);
		} else {
			concole.error('Error:', xhr.statusText);
		};
	};
};

//初始化请求
xhr.open('GET', '/try/ajax/getcustomer.php?q=1', true);

//发送请求
xhr.send();
```

#### 5）onload
onload 是一个事件处理器，用于处理请求成功完成时的事件。当 `readyState` 变为 `4`（请求完成）且 `status` 为 `200`（请求成功）时，会触发 onload 事件。
```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', '/try/ajax/getcustomer.php?q=1', true);
xhr.onload = function() {
	if(xhr.status >= 200 && xhr.status < 300){
	console.log(xhr.responseText);
	} else {
		console.error('Error:', xhr.statusText);
	};
};
xhr.send();
```
#### 6）onerror
onerror 是一个事件处理器，用于处理请求失败时的事件。当请求发生网络错误或服务器错误时，会触发 onerror 事件。
```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', '/try/ajax/getcustomer.php?q=1', true);
xhr.onload = function() {
	if(xhr.status >= 200 && xhr.status < 300){
	console.log(xhr.responseText);
	} else {
		console.error('Error:', xhr.statusText);
	};
};
xhr.onerror = function() {
	console.error('Network error');
}
xhr.send();
```

## 3. AJAX 与 fetch( ) 的关系
JavaScript中的fetch函数可以代替AJAX。
- fetch( ) 是JavaScript提供的更简洁、更强大的功能
- fetch 常与  async/await 连用
>fetch 和 AJAX 都可以与 async/await 连用。但是 AJAX 需要封装成一个 Promise 的函数，然后再用 async/await 来调用函数（不常用）。fetch 则已经提供了基于 Promise 的接口，直接使用 fetch 与 async/await 会更简单、方便。
#### 示例：
#### 1）使用 XMLHttpRequest 和 async/await
```javascript
function makeRequest(url) {
	return new Promise((resolve, reject) => {
		let xhr = new XMLHttpRequest();
		xhr.open('GET', url);
		xhr.onload = () => {
			if (xhr.status >= 200 && xhr.status < 300) {
				resolve(xhr.responseText);
			} else {
				reject(Error('Something went wrong'));
			};
		};
		xhr.onerror = () => {
			reject(Error('Network error'));
		};
		xhr.send();
	});
}

async function fetchData() {
	try{
		const data = await makeRequest('/try/ajax/getcustomer.php?q=1');
		console.log(data);
	} catch {
		console.error('Error:', error);
	}
}

fetchData();
```
#### 2）使用 fetch 和 async/await
```javascript
async function fetchData() {
	try {
		const response = await fetch('/try/ajax/getcumstomer.php?q=1');
		if (!response.ok) {
			throw new Error('Network response was not ok');
		};
		const data = await response.text();
		console.log(data);
	} catch (error) {
		console.error('Error:', error);
	}
};

fetchData();
```

## 4. 与后端语言 PHP 连用
#### 实例 （当文本框中的值 str 发生改变时，变量 txtHint 的匹配值）：
```javascript
function showHint(str) {
	let xmlhttp;
	if(str.length == 0){
		//当文本框为空时，textHint设置为空
		document.getElementById('textHint').innerHTML = "";
		return;
	};
	if(window.XMLHttpRequest) {	//若浏览器支持XMLHttpRequest对象，则执行
		xmlhttp = new XMLHttpRequest();
	} else {
		xmlhttp=new  ActiveXObject("Microsoft.XMLHTTP");
	};
	xmlhttp.onreadystatuechange = function() {
		if(xmlhttp.readyState == 4 && xmlhttp.statue) {
			document.getElementById('txtHint').innerHTML = xmlhttp.responseText;
		};
	};
	xmlhttp.open('GET', '/try/ajax/gethint.php?q=1', true);
	xhrhttp.send();
}
```
#### 源代码解析：

如果输入框为空  str.length==0，则该函数清空  **txtHint**  占位符的内容，并退出函数。

如果输入框不为空，showHint()  函数执行以下任务：

-   创建  **XMLHttpRequest**  对象
-   当服务器响应就绪时执行函数
-   把请求发送到服务器上的文件
-   请注意我们向 URL 添加了一个参数 q （带有输入框的内容）
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyOTk1NjIzXX0=
-->