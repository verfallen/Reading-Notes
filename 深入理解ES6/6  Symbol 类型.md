## Symbol和Symbol属性
symbol是程序创建并且可以用作属性键的值，并且它能避免命名冲突的风险。
**创建**：调用`Symbol()`创建一个新的symbol，接受一个可选参数，其可以让你添加一段文本描述即将创建的Symbol。它的值与其它任何值皆不相等。
**注意**：由于`Symbol`是原始值，因此调用`new Symbol()`会导致程序抛出错误。
**辨识方法**:`typeof`支持返回Symbol
###使用方法
`Symbol`可用于计算对象字面量属性名、`Object.defineProperty()`方法和`Object.defineProperties()`方法的调用过程中。
### 共享体系
**创建可共享的Symbol**:`Symbol.for()`
**检索**：`Symbol.keyFor()`，在Symbol全局注册表中检索与Symbol有关的键
**与强制类型转换**：除布尔值外，其余均不能转换
### Symbol属性检索
+ `Object.getOwnPropertySymbols(obj)`:检索对象中的Symbol属性，返回一个包含素有Symbol自有属性的数组。
+ `Reflect.ownKeys(obj)`
### 改变
+ `Object.defineProperty()`
+  `Object.defineProperties()`
### 通过well-known Symbol 暴露内部操作
ES6中开发了以前JavaScript中常见的内部操作，并通过预定义一些well-known Symbol来表示。每一个这类Symbol都是Symbol对象的一个属性。
#### `Symbol.hasInstance`方法：用于确定对象是否为函数的实例
注意：如果要触发`Symbol.hasInstance`调用，`instanceof`的左操作数必须是一个对象，如果为非对象会导致总会返回`false`。
#### `Symbol.isConcatSpreadable`：一个布尔值，用于表示当传递一个集合作为`Array.prototype.concat()`方法的参数时，是否应该讲元素规整到同一层级
如果该属性值为`true`,则表示对象有`length`属性和数字键，故它的数值型属性应该被独立添加扫`concat()`调用的结果中

```
let collection = {
  0:'hello',
  1:'world',
  length:2,
  [Symbol.isConcatSpreadable]:false
};
let message = ['hi'].concat(collection);

console.log(message.length);        //3
console.log(message);               //["hi", "hello", "world"]
```
#### `match`、`replace`、`search` 和 `split`属性
分别对应各自方法第一个参数应该调用的正则表达式参数的方法
#### `Symbol.toPrimitive`：返回对象原始值
#### `Symbol.toStringTag`：一个在调用Object.prototype.toString()方法时使用的字符串，用于创建对象描述。
#### `Symbol.unscopables`:定义一些不可被`with`语句引用的对象属性名称的对象集合。
