 # Promise用法
  ## 1.Promise是创建promise对象的关键字
 ```javascript
	 const myPromise = new Promise((resolve, reject) =>{	//异步操作代码
		if  (/* 操作成功 */)  {  
			resolve('成功的结果');  // 将 Promise 状态改为 fulfilled  
		}  else  {  
			reject('失败的原因');  // 将 Promise 状态改为 rejected  
		}
	})
```
**resolve** 将传回的promise标记为Fulfilled（已完成），然后执行 **.then( )** 
 **reject** 将传回的promise标记为Rejected （已失败），然后执行 **.catch( )**

## 2.Promise的用法（链式调用）
  1.then( )
  2.catch( )
  3.finally( )
   -   多个 `.then()` 是按顺序执行的，不会并发执行
-   每个 `.then()` 的回调函数会在前一个回调函数执行完成后才开始执行。   
-   如果某个 `.then()` 的回调函数抛出错误，会跳转到最近的 `.catch()` 方法

 ## 3.Promise的静态方法

 ### （1）Promise.all( )
当 **Promise.all** 中的所有 **promise** 完成后，或者有一个失败时，会执行回调函数
```javascript
Promise.all([promise1, promise2, promise3])
  .then((results) => {
    // results 是一个包含所有 Promise 结果的数组
    console.log(results);
  })
  .catch((error) => {
    // 任一 Promise 失败就会进入这里
    console.error(error);
  });
```
 ### （2）Promise.race( )
返回最先完成（无论失败或者成功）的 **Promise** 的结果
```javascript
Promise.race([promise1, promise2, promise3])
	.then((result) => {
		//最先完成的Promise的结果
		console.log(result);
	})
	.catch((error) => {
		//最先完成的Promise是失败的
		console.error(error);
	})
```

 ## 4.Promise 应用实例
  ### （1）使用Promise链处理多个异步操作
  ```javascript
function getUser(userId){
		return fetch(`/users/${userId}`);
	}
	
function getPosts(userId){
	return fetch(`/users/${userId}/posts`);
}

getUser(123)
	.then((user) => {
		console.log('获取用户信息', user);
		return getPosts(user.id);
	})
	.then((posts) => {
		console.log('获取用户帖子', posts);
	})
	.catch((error) => {
		console.error('操作失败', error);
	})
  ```

## 5.注意事项
### （1）then catch finally块是否都可以多次调用
  **then** 和 **finally** 块都可以多次调用，**then**  和 **finally** 链都是向下顺序执行关系。
  **catch** 只会执行第一个
  ### （2）then 块如何中断
  **then** 是向下顺序执行的，**return** 也是不能中断。
  可以使用 **throw** 来跳转至 **catch** 实现中断。
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxOTk4NzQxOV19
-->