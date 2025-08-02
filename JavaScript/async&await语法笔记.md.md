# async / await 用法
>async/await 是基于Promise上，在ES2017中引入的新语法结构
## 1. async 函数
在函数声明前添加 **async** 关键字，表示该函数时异步的
```javascript
async function getUser(userId){
	//函数体
}
```
async 函数会返回一个 **Promise**：
- 如果返回值不是Promise，则会自动变成``Fulfilled Promise`` 
- 如果有异常/报错，则会返回``rejected Promise``


## 2. await 表达式
await 表达式只能在 async 函数中使用：
```javascript
async function getUser(userId){
	const result = await somePromises;
	console.log(result);
}
```
即 **await** 关键字是添加在需要异步操作的代码前面。
当代码运行到 **await** 时，await会暂停 **async** 函数的执行，等待Promise的完成。
 - 如果Promise被resolve，则返回resolve的值
 - 如果Promise被rejected，则抛出错误（可用 **try/catch** 捕获）
```javascript
async function fetchUserData() {
	try{
		const response = await fetch('https://api.example.com/user');
		if(!response.ok) {
			throw new Error('网络响应不正常');
		};
		const data = await response.json();
		console.log(data);
	} catch(error) {
		console.error('获取数据失败', error)
	};
}
```
 - 如果需要并行执行多个异步操作，可以使用Promise.all
 ```javascript
 async function fetchMultipleData() {
	const [userData, productData] = await Promise.all([
		fetch('/api/user'),
		fetch('/api/products')
	]);

	const user = await userData.json();
	const products = await productData.json();

	return {user, products};
}
 ```

## 3. async/await 和 Promise 的异同

>async/await并不是完全替代 Promise ，只是在大多数情况下提供更易读、优雅的解决方法

|特性|async/await|Promise then/catch|
|-|-|-|
|可读性|高，类似于同步代码|中，链式调用|
|错误处理|使用 **try/catch**|使用 **.catch()**|
|调试|更容易，有明确的调用栈|较困难，调用栈可能不清晰|
|代码结构|更扁平|嵌套或链式|
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NjY5OTY0MTBdfQ==
-->