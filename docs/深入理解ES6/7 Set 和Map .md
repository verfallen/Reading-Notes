# Set 集合和 Map 集合

`Set`：无重复元素的列表，通常的做法是检测给定的值在某个集合中是否存在
`Map`：包含多组键值对，经常被用于缓存频繁取用的数据

## ES5 中模拟实现的情况

```
var set = Object.create(null);
set.foo = true;

var map = Object.create(null);
map.foo = 'bar';
```

模拟这两种集合对象的唯一区别是存储的值不同。
模拟实现出现的问题：

- 由于键名都是字符串类型且在对象中是唯一的，所以`map[5]`和`map["5"]`引用的其实是同一个属性。
- 对于`Map`集合来说，如果属性值是假值，则在要求使用布尔值的情况下会被自动转换为`false`。

## ES6 中的 Set 集合

将对象存储在 Set 的实例与存储在变量中完全一样，只要`Set`实例中的引用存在，垃圾回收机制就不能释放该对象的内存空间。可以将`Set`类型看作一个强引用的`Set`集合。
**创建**：`new Set()`  
**使用数组初始化**：`new Set([1,2,3])`  
**添加**：`Set.prototype.add()`  
**检测元素是否存在**：`has()`  
**移除**：  
 `delete()`删除一个  
 `clear()`删除所有  
**长度属性**：`size`  
`forEach()`的回调函数接收以下 3 个参数：

- Set 集合中下一次索引的位置
- 与第一个参数一样的值
- 被遍历的`Set`集合本身

**访问**：不能像访问数组元素那样直接通过索引访问，最好将`Set`集合转换成一个数组
**方法**：`array = [...set];`

## Weak Set 集合：弱引用 Set 集合

**创建**：`new WeakSet();`
**方法**：`add()`，`delete()`和`has()`
**与 set 的区别**：

- 方法参数只能是对象
- 不可迭代
- 不暴露任何迭代器
- 不支持`forEach()`
- 不支持`size`属性

## Map 集合：一种储存着许多键值对的有序列表，其中键名和对应的值支持所有的数据类型。键名的等价性判断是通过`Object.is()`实现的。因此 5 和"5"会被判断为两种类型。Map 可以支持对象作为属性键名

**初始化**：

- `let map = new Map();`
  `map.set("name") = "wxl";`

- `new Map([["name","wxl"],["age",20])`

**方法**：  
`has(key)`:检索  
`set(key,value)`：设置属性  
`get(key)`：获取属性  
`delete(key)`：移除键名及其对应的值  
`clear()`：移除所有键值对

forEach 方法回调函数接受 3 个参数：

- Map 集合中下一次索引的位置
- 值对应的键名
- Map 集合本身

## Weak Map 集合

一种存储着许多键值对的无序列表，列表的**键名必须是非`null`类型的对象**，键名对应的值则可以是任意类型
支持方法：`delete()`和 `has()`方法，不支持`clear()`
**私有对象数据**：
ES5 实现：

```
var Person = (function(){
	var privateData = {},
  	privateId = 0;

  function Person(name){
  	Object.defineProperty(this,"_id",{value:privateId++});

    privateData[this._id] = {
    	name:name
    };
  }

  Person.prototype.getName = function(){
  	return privateData[this_id].name;
  };

  return Person;
}());

console.log(Person);
```

这种方法带来的问题：如果没有实现主动管理，那么私有数据就永远不会消失。
使用 Weak Map 实现：

```
let Person = (function(){
	let privateData = new WeakMap();

  function Person(name){
  	privateData.set(this,{name:name});
  }

  Person.prototype.getName = function(){
  	return privateData.get(name);
	};

  return Person;
}());
```
