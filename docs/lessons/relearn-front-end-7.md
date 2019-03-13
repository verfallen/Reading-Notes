# `JavaScript`对象

1. 问题：为什么 JavaScript（直到 ES6）有对象的概念，但是却没有类的概念呢？  
   **答：**`JavaScript`对象设计使用了**原型**。
2. 对象的特征：
   1. 唯一标识性
   2. 具有状态
   3. 具有行为，行为可以改变对象的状态。
3. 在`JavaScript`中，对象的状态和行为都被抽象为了属性。对象具有唯一标识的内存地址，这是它的唯一标识性。
4. 为什么在 JavaScript 对象里可以自由添加属性，而其他的语言却不能呢？  
   **答：**`JavaScript`中对象的独有特色，对象具有高度的动态性，这是因为`JavaScript`赋予了使用者在运行时为对象改变状态和行为的能力。
5. `JavaScript`中的两类属性
   1. 数据属性，具有一下四个特征
      - `value` 属性值
      - `writable`是否可以赋值
      - `enumerable` 是否可以通过`for...in`枚举
      - `configurable` 是否可被删除或改变特征值
   2. 访问器属性，具有以下 4 个特征
      - `getter`：函数或 `undefined`，在取属性值时被调用。
      - `setter`：函数或 `undefined`，在设置属性值时被调用。
      - `enumerable`：决定 `for in` 能否枚举该属性。
      - `configurable`：决定该属性能否被删除或者改变特征值。
6. 创建数据属性

   ```javascript
   var o = { a: 1 }; //第一种
   o.b = 2; //第二种
   ```

7. 创建访问器属性：在创建对象时，使用`get`或者`set`关键字来创建访问器属性

   ```javascript
   var o = {
     get a() {
       return 1;
     }
   };
   ```

8. 查看属性特征：`Object.getOwnPropertyDescripter()`
9. 改变属性的特征或者定义访问器属性：`Object.defineProperty()`
10. `JavaScript` 对象的运行时就是一个“属性的集合”，属性以**字符串或者`Symbol`**为`key`，以**数据属性特征值**或者**访问器属性特征值**为`value`。
