# this
>在JavaScript中，`this`是一个非常重要的关键字，它用于指向函数运行时的上下文环境。

**`this`的指向在函数被调用时确定，而不是在函数被定义时确定**

## 1. `this`的常见用法
### （1）全局上下文中的`this`

-   在全局作用域中，`this`指向全局对象（在浏览器中是`window`）
### （2）对象方法中的`this`

-   当函数作为对象的方法被调用时，`this`指向该对象
```js
const obj = {
  name: "Kimi",
  sayName: function() {
    console.log(this.name); // 输出 "Kimi"
  }
};
obj.sayName();
```
### （3）构造函数中的`this`

-   当函数作为构造函数被调用时（使用`new`关键字），`this`指向新创建的对象
```js
function Person(name) {
  this.name = name;  //this 相当于 Person
}
const person = new Person("Kimi");
console.log(person.name); // 输出 "Kimi"
```

### （4）箭头函数中的`this`

-   箭头函数不绑定自己的`this`，它会捕获其所在上下文的`this`值（捕获最近的）
```js
const obj = {
  name: "Kimi",
  sayName: function() {
    const arrowFunc = () => {
      console.log(this.name); // 输出 "Kimi"
    };
    arrowFunc();
  }
};
obj.sayName();
```

## 2. 改变 `this` 指向的方法
>`bind`、`apply`和`call`都是用来改变函数的`this`指向的方法，但它们的使用场景和行为有所不同

### （1）`call`方法

-   `call` 方法可以调用一个函数，并且可以指定函数内部的 `this` 指向。并且立即执行该函数
    
-   语法：`function.call(thisArg, arg1, arg2, ...)`
```js
function greet() {
  console.log(`Hello, ${this.name}!`);
}
const person = { name: "Kimi" };
greet.call(person); // 输出 "Hello, Kimi!"
```

### （2）`apply`方法

-   `apply`方法与`call`类似，但它接受一个参数数组。立即调用函数
    
-   语法：`function.apply(thisArg, [argsArray])`
```js
function greet() {
  console.log(`Hello, ${this.name}!`);
}
const person = { name: "Kimi" };
greet.apply(person); // 输出 "Hello, Kimi!"
```

### （3）`bind`方法

-   `bind`方法用于创建一个新函数，并且可以指定新函数内部的`this`指向。
    
-   语法：`const newFunc = function.bind(thisArg, arg1, arg2, ...)`
```js
function greet() {
  console.log(`Hello, ${this.name}!`);
}
const person = { name: "Kimi" };
const boundGreet = greet.bind(person);
boundGreet(); // 输出 "Hello, Kimi!"
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MTI3MDU1MTRdfQ==
-->