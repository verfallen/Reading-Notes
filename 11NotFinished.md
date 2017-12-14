## Promise 和异步编程
### 异步编程背景知识
+ 事件模型
	+  用于点击按钮发生类似`onclick`这样的事件，它向任务队列中添加一个新任务来相应用户的操作，这是JavaScript中最基础的异步编程形式，直到事件触发时才执行事件处理程序，且执行时上下人与定义时相同。
	+ 适用情况：响应用户就会和完成类似的低频功能
	+ 产生问题：必须跟踪每个事件的事件目标（例如按钮），此外必须要保证事件在添加事件处理程序之后才被触发
+ 回调模式
	+ 与事件模型类似，异步代码都会在未来的某个时间点进行，区别是回调模式中被调用的函数是作为参数传入的
### Promise 基础知识
#### 生命周期
+ 进行中（pending）
+ 已处理（settled）
	+ Fulfilled 已成功完成
	+ Rejected 未成功完成
#### `.then()`函数
参数说明：第一个是当Promise的状态变为fulfilled 时要调用的方法，与异步操作相关的附加数据都会传递给这个完成函数，第二个是当Promise的状态变为rejected时要调用的方法，所有与失败状态相关的附加数据都会传递给拒绝函数。两个参数均是可选的
#### `catch()`方法：相当于只传入拒绝处理程序的`then()`方法
```
//以下两方法等价
promise.catch(function(err){
  console.error(err.message);
});

promise.then(null,function(err){
  console.error(err.message)
});
```
#### 创建未完成的Promise
使用Promise构造函数，只接受一个参数：包含初始化Promise代码的执行器函数。执行器接受两个参数，分别是`resolve()`函数和`reject`函数。
```
function readFile(filename){
  return new Promise(function(resolve,reject){
    ...
  })
}
```
Promise工作原理：Promise的执行器会立即执行，然后才执行后续流程中的代码。完成处理程序和拒绝处理程序是在执行器完成后被添加到任务队列的末尾。
```
let promise = new Promise(function(resolve,reject){
  console.log('promise');
  resolve();
})
promise.then(function(){
  console.log('resolve');
});

console.log('hi');
```
执行顺序：
```
promise
hi
resolve
```
#### 创建已处理的Promise
+ 使用`Promise.resolve()`: 创建了一个决议为输入值的Promise
+ 使用`Promise.reject()`：创建一个被拒绝的promise ,决绝理由为‘参数值’
+ 非Promise的Thenable对象：当作为前两个方法的参数时，这些方法会创建一个新的Promise ，并在`then()`函数中调用
	+ 定义：拥有`then()`方法并接受`resolve`和`reject`这两个参数的普通对象
```
let thenable = {
  then:function(resolve,reject){
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value){
  console.log('42');  //42
});  
```
#### 执行器错误：如果执行器内部抛出错误，那么Promise  的拒绝处理程序就会被调用。
```
let promise = new Promise(function(resolve,reject){
  throw new Error('aaaa');
});

promise.catch(function(error){
  console.log(error.message);
});
```
### 全局的Promise拒绝处理
#### Node.js环境的拒绝处理：触发**`process`**对象上的两个事件
+ 作用：识别那些被拒绝又没有被处理的Promise
+ 分类：
	+ unhandleRejection 在一个事件循环中，当Promise 被拒绝，并且没有提供拒绝处理程序时被调用
		+ 参数：拒绝原因及被拒绝的Promise
		+ 示例
	```
    let rejected;
    
    process.on('unhandledRejection', function (reason, promise) {
      console.log(reason.message);
      console.log(rejected === promise);
    });
    
    rejected = Promise.reject(new Error('explosion'));
    ```
	+ rejectionHandled 在一个事件循环后，当Promise 被拒绝，并且没有提供拒绝处理程序时被调用 
		+ 参数：被拒绝的promise
	```
    let rejected;
	
	process.on('rejectionHandled', function (promise) {
	  console.log(rejected === promise);  //true
	});
	
	rejected = Promise.reject(new Error('explosion'));
	
	//等待添加处理程序
	setTimeout(function () {
	  rejected.catch(function (value) {
	    console.log(value.message);    //'explosion'
	  })
	}, 5000);
    ```  
    
#### 浏览器环境的拒绝处理：触发**`window`**对象上的两个事件,事件名称及意义同Node.js环境完全相同
   + 参数：一个有以下属性的事件对象
	   + type 事件名称
	   + promise 被拒绝的promise对象
	   + reason 来自promise 的拒绝值
###  串联Promise
每次调用`then()`方法或`catch()`方法时实际上创建并返回了另一个Promise，只有当第一个Promise完成或者被拒绝后，第二个才会被解决。
```
let p1 = new Promise(function(resolve,reject){
  resolve(42);
});

p1.then(function(value){
  console.log(value);  
}).then(function(){
  console.log('finish');  
});
```
结果：
```
42
finish
```
#### 捕获错误：完成处理程序或拒绝处理程序中可能发生错误，而Promise 链可以用来捕获错误。
使用：务必在Promise 链的末尾留有一个拒绝处理程序以确保能够正确处理所有可能发生的错误。
```
let p1 = new Promise(function(resolve,rejected){
  resolve(42);
});

p1.then(function(value){
  throw new Error('Boom!');
}).catch(function(err){
  console.log(err.message);
});
```
#### Promise链的返回值
+ 传递数据，直接返回值
+ 返回Promise
### 响应多个Promise
#### Promise.all()
+ 接收一个promise数组并返回一个新的promise，这个新的promise等待数组中的所有Promise完成。
	```	 
	let p1 = new Promise(function(resolve,reject){
	  resolve(42);
	})
	
	let p2 = new Promise(function(resolve,reject){
	  reject(43);
	});
	
	let p3 = new Promise(function(resolve,reject){
	  resolve(44);
	});
	
	let p4 = Promise.all([p1,p2,p3]);
	p4.then(function(value){
	 console.log(Array.isArray(value));  //true
	 console.log(value);                        //[42,43,44]
	});
	```        
+ 如果这些promise中有一个被拒绝，则会立即被拒绝，并丢弃来自其他所有promise的全部结果，所以切记为每个promise关联一个错误函数。
	```
	let p1 = new Promise(function(resolve,reject){
	  resolve(42);
	})
	
	let p2 = new Promise(function(resolve,reject){
	  reject(43);
	});
	
	let p3 = new Promise(function(resolve,reject){
	  resolve(44);
	});
	
	let p4 = Promise.all([p1,p2,p3]);
	p4.catch(function(value){
	 console.log(Array.isArray(value)); //false
	 console.log(value);                //43
	});
	```
#### Promise.race()
监听多个Promise的可迭代对象作为唯一参数并返回一个Promise，只要有一个Promise被解决返回的Promise 就被解决，无需等待所有Promise完成。
```
let p1 = Promise.resolve(42);

let p2 = new Promise(function(resolve,reject){
  resolve(43);
});

let p3 = new Promise(function(resolve,reject){
  resolve(44);
});

let p4 = Promise.race([p1,p2,p3]);

p4.then(function(value){
  console.log(value);  //42
});
```
### 自Promise 继承
```
class MyPromise extends Promise{
  success(resolve,reject){
    return this.then(resolve,reject);
  }

  failure(reject){
    return this.catch(reject);
  }
}

let promise = new MyPromise(function(resolve,reject){
  resolve(42);
});

promise.success(function(value){
  console.log(value);
}).failure(function(value){
  console.log(value);
});
```
MyPromise同样从基类派生静态方法，包括`MyPromise.resolve()`、`MyPromise.reject()`、`MyPromise.race()`和`MyPromise.all()`。其中前两个方法无论传入什么值都会返回一个MyPromise实例。
### 基于Promise的异步任务执行
