## 函数(Function)
[TOC]
### 创建方式
1. 函数声明
``` javascript
function sum(num1, num2) {
	return num1+ num2;
}
```
2. 函数表达式
``` javascript
var sum = function(num1, num2){
	return num1 + num2;
}
```
3. 使用`Function`构造函数（不推荐）
``` javascript
var sum = new Function("num1", "num2", "return num1 + num2")
```
###函数的使用
#### 函数作为值
由于在`JavaScript`中，函数名本身就是对象，所以函数也可以作为值使用。作为值，函数可以
+ 作为参数
``` javascript
function callFunction(someFunction, args) {
  someFunction(args);
}

```
+ 作为返回值
``` javascript
function crateSortFunction(name) {
  return function(obj1, obj2) {
    return obj1[name] - obj2[name];
  };
}
```
### 函数内部属性
#### `arguments`
`arguments` 类数组对象，包含传入函数的所有参数
+ `arguments`中存在一个`callee`属性，该对象是一个指针，指向拥有这个`arguments`对象的函数
####  `this` 
`this` 指向函数执行的环境对象，有以下情况：

+ 作为全局函数或普通函数调用，`this`指向全局对象，在浏览器环境中，`this`指向`window`，在`node`环境中，`this`指向`global`
	``` javascript
	function foo() {
	  console.log(this);  //window 或 global
	}
	```
	
+ 作为对象的方法调用时，`this`指向该对象
	``` javascript
	var obj = {
	  name: "object",
	  getName() {
	    console.log(this);  // 指向obj
	  }
	};
	```
	
+ 作为构造函数调用时，`this`指向新构造的函数
	``` javascript
	function Person(name){
	  this.name = name;
	  console.log(this);  //Person { name: 'lily' }
	}
	new Person('lily');  
	```
	
+ 通过`call`,`apply`调用时，`this`指向调用时传入的参数`this`
	``` javascript
	var obj = {
	  name: "obj",
	  getName() {
	    console.log(this.name);
	  }
	};
	var temp = {
	  name: "temp"
	};
	obj.getName.call(temp);  // 'temp'
	```

+ 闭包使用时，`this`指向全局对象
	``` javascript
	var obj = {
	  name: "obj",
	  getName() {
	   return function(){
	      console.log(this);
	    }
	  }
	};
	var temp = {
	  name: "temp"
	};
	obj.getName()();  // "temp"
	```

####`caller`调用当前函数的函数引用
### 函数属性和方法
#### 属性
+ `length` 接收的命名参数个数
+ `prototype` 
#### 方法
+ `call()` 在特定的作用域中调用函数，接收两个参数，第一个是在其中运行函数的作用域，另一个是参数数组
+ `apply()`与`call()`作用相同，不同的是必须明确传递每一个参数
+ `bind()` 创建一个函数的实例，其`this`值会被绑定到传给`bind()`函数的值
### 递归
**递归函数**：一个函数通过名字调用自身的情况下构成递归。
#### 实现递归的方式
例如，一个经典的阶乘函数，可以下列三种方式实现：
1. 函数通过名字调用自身
``` javascript
function factorial(num) {
	if(num <= 1 ){
		return 1;
	}else{
		return num * factorial(num - 1);
	}
}
```
2. 函数通过`arguments.callee`实现递归
``` javascript
function factorial(num) {
	if(num <= 1 ){
		return 1;
	}else{
		return num * arguments.callee(num - 1);
	}
}
```
3. 通过函数表达式实现递归
``` javascript
var factorial = function f(num) {
	if(num <= 1 ){
		return 1;
	}else{
		return num * f(num - 1);
	}
}
```
### 闭包
**闭包**：是指有权访问另一个函数作用域中的变量函数。
#### 通过一个栗子理解闭包
``` javascript
function createCompareFunction(property) {
  return function(obj1, obj2) {
    return obj1[property] - obj2[property];
  };
}
```
上述例子中的匿名函数会将`createCompareFunction`的活动对象添加到自身的作用域链中，从而能访问到外部函数的变量。
#### 闭包的坑
值得注意的是，**闭包只能取得包含函数中任何变量的最后一个值**。
``` javascript
function createFunctions(){
  var result = [];

  for(var i = 0;i<10;i++){
    result[i] = function(){
      return i;
    }
  }

  return result;
}

var result = createArray();
result[0](); // 10
```
上述示例中，`result[0]()`实际结果返回`10`。原因就在于，内部函数访问`i`的时候，`i`值为`10`。
#### 闭包的使用
在上述示例中，我们想要在每个函数中得到自己的索引值，可以通过`创建另一个匿名函数`来实现。
```javascript
function createFunctions() {
  var result = [];

  for (var i = 0; i < 10; i++) {
    result[i] = function(num) {
      return (function() {
        return num;
      })(i);
    };
  }

  return result;
}

result[0](); // 0
```
#### 内存泄露
#### 模拟块级作用域
``` javascript
(function() {})(); 
```
#### 私有变量
在`JavaScript`中，没有私有成员的概念，但是在函数内部定义的变量，都可以认为是**私有变量**。在函数外部不能访问到这些变量，需要通过对象的**特权方法**来访问。
创建特权对象的两种方法：
1. 在构造函数中定义特权方法
```javascript
function Person() {
  var name = "myName";

  this.getName = function() {
    return this.name;
  };
}
```
2. 在私有作用域中定义私有变量或函数，创建特权方法
``` javascript
(function() {
  var name = "obj";

  //构造函数
  MyObject = function() {};

  MyObject.prototype.getName = function() {
    return name;
  };
})();

```