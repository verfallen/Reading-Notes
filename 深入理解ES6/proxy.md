## 代理（Proxy）和反射（Reflection） API
### 创建一个简单的代理
使用Proxy 构造函数创建代理需要传入两个参数：目标（target）和处理程序（handler）.处理程序是定义一个或多个陷阱的对象。
```
let target = {};
let proxy = new Proxy(target, {});
```
### 使用set陷阱验证属性
+ 参数说明：
	+ trapTarget 用于接收属性（代理的目标）的对象
	+ key 要写入的属性键（String 或 Symbol）
	+ value 写入的属性值
	+ receiver 操作发生的对象（通常是代理）
+ 举个栗子:创建一个属性值必须是数字的对象，每增加一个属性都要加以验证
	```
	let target = {
	  name: 'target'
	};
	
	let proxy = new Proxy(target, {
	  set(trapTarget, key, value, receiver) {
	    //忽略不希望收到影响的已有属性
	    if (!trapTarget.hasOwnProperty(key)) {
	      if (isNaN(value)) {
	        throw new TypeError('属性必须是数字');
	      }
	    }
	
	    //添加属性
	    return Reflect.set(trapTarget, key, value, receiver);
	  }
	});
	
	//添加一个新的属性
	proxy.count = 1;
	console.log(proxy.count);
	console.log(target.count);
	
	//由于目标已有name 属性以内可以给它赋值
	proxy.name = 'proxy';
	console.log(proxy.name);
	console.log(target.name);
	
	//给不存在的属性赋值将会跑出错误
	proxy.anothername = 'proxy';
	```
### 用get陷阱验证对象结构
+ 参数说明：
	+ trapTarget 代理目标
	+ key 读取的属性键
	+ receiver 操作发生的对象（通常是代理）
+ 举个栗子：读取对象属性时，如果属性不存在，则抛出
```
/**
 * 用get陷阱验证对象结构
 */
let proxy = new Proxy({}, {
  get(trapTarget, key, receiver) {
    if (!(key in receiver)) {
      throw new TypeError('属性' + key + '不存在');
    }

    return Reflect.get(trapTarget, key, receiver);
  }
});

proxy.name = 'proxy';
console.log(proxy.name);  //proxy

console.log(proxy.nme);   //抛出错误
``` 
### 使用has陷阱隐藏已有属性
+ 参数
	+ trapTarget 读取属性的对象
	+ key 要检查的属性键
+ 举个栗子
```
/**
 * 使用has隐藏已有属性
 */
let target = {
  name: 'target',
  value: 42
};

let proxy = new Proxy(target, {
  has(trapTarget, key) {
    if (key === 'value') {
      return false;
    } else {
      return Reflect.has(trapTarget, key);
    }
  }
});

console.log("value" in proxy);    //false
console.log("name" in proxy);     //true
console.log("toString" in proxy); //true 
```
### 用deleteProperty 陷阱防止删除属性
+ 参数
	+ trapTarget 代理的目标
	+ key 属性键
+ 举个栗子：设置不可配置的属性不可删除 
```
/**
 * deleteProperty 陷阱防止删除属性
 */
let target = {
  name:'target',
  value:42
};

let proxy = new Proxy(target,{
  deleteProperty(trapTarget,key){
    if(key === 'value'){
      return false;
    }else{
      return Reflect.deleteProperty(trapTarget,key);
    }
  }
});

//尝试删除proxy.value
console.log("value" in proxy);    //true
let result1 = delete proxy.value; 
console.log(result1);             //false

console.log("value" in proxy);     //true
```
### 原型代理陷阱
`setPrototypeOf`接受以下参数

+  trapTargert 代理的目标
+ proto 作为原型使用的对象
#### 原型代理陷阱的运行机制
原型代理陷阱的机制：
+ `getPrototypeOf`必须返回对象或`null`,只要是返回值就会导致运行错误
+ `setPrototypeOf`如果操作失败则返回`false`，只要返回不是`false`的值，则假设操作成功。
+ `Reflect`上的相应方法可以实现两个陷阱的默认行为
```
let target = {};
let proxy = new Proxy(target, {
  getPrototypeOf(trapTarget) {
    return null;
  },
  setPrototypeOf(trapTarget, proto) {
    return false;
  }
});

let targetProto = Object.getPrototypeOf(target);
let proxyProto = Object.getPrototypeOf(proxy);

console.log(targetProto === Object.prototype);  //true
console.log(proxyProto === Object.prototype);   //false
console.log(proxyProto);                        //null

Object.setPrototypeOf(target, {});

Object.setPrototypeOf(proxy, {});                //报错
```
两组方法的区别：
`Object.getPrototypeOf()`和`Object.setPrototypeOf()`是高级操作，创建伊始就是给开发者用的。而`Reflect.getPrototypeOf()`和`Reflect.setPrototypeOf()`是底层操作，其赋予开发者可以访问之前只在内部操作的`[[GetPrototypeOf]]`和`[[setPrototypeOf]]`的权限。
具体：

+ 如果传入的参数不是对象，`Reflect.getPrototypeOf()`方法会跑出错误，而`Object.getPrototypeOf()`则会在操作前将参数强制转换为一个对象。
	```
	let result1 = Object.getPrototypeOf(1);
	console.log(result1 === Number.prototype);  //true
	
	Reflect.getPrototypeOf(1);  //报错	
	```
+ `Reflect.setPrototypeOf()`方法返回一个布尔值来表示操作是否成功，成功时返回`true`，失败则返回`false`，而`Object.getPrototyoeOf()`一旦失败则抛出一个错误，成功则返回第一个参数（如果不是对象，则强制转换为对象）。
	```
	//setPrototypeOf的区别
	let target1 = {};
	let result1 = Object.setPrototypeOf(target1,{});
	console.log(target1 === result1);   //true
	
	let target2 = {};
	let result2 = Reflect.setPrototypeOf(target2,{});
	console.log(target2 === result2);   //false
	console.log(result2);               //ture
	```
### 对象可扩展性陷阱
ES6可以通过代理中的`preventExtensions`和`isExtensible`陷阱拦截这两个方法并调用底层对象，均接受唯一参数trapTarget对象，并且均返回布尔值。`isExtensible`表示对象是否可扩展，`preventExtensions`表示操作是否成功。
区别：传入非对象值时，`Object.isExtensible()`返回`false`，而`Reflect.isExtensible()`则抛出错误。相对于高级方法而言，底层具有更严格的错误检查。
### 属性描述符陷阱
在代理中分别可以使用`defineProperty`陷阱和`getOwnPropertyDescriptor`陷阱拦截`Object.defineProperty()`和`Object.getOwnPropertyDescriptor()`方法的调用。

+ `defineProperty()`
	+ 参数
		+ trapTarget 代理目标
		+ key 属性键
		+ decriptor 属性的描述符对象
	+  返回值：操作成功返会`true`，否则返回`false`
+ `getOwnPropertyDescriptor` 只接受trapTarget和key两个参数，最终返回描述符。
+ 
