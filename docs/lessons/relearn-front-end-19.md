# JavaScript 执行-函数调用切换上下文

## 函数家族

1. 普通函数：用 function 关键字定义的函数。

   ```javascript
   function foo() {
     // code
   }
   ```

2. 箭头函数：用 => 运算符定义的函数。

   ```javascript
   const foo = () => {
     // code
   };
   ```

3. 方法：在 class 中定义的函数

   ```javascript
   class C {
     foo() {
       //code
     }
   }
   ```

4. 生成器函数：用 function \* 定义的函数。

   ```javascript
   function foo*(){
       // code
   }
   ```

5. 类：用 class 定义的类，实际上也是函数。

   ```javascript
   class Foo {
     constructor() {
       //code
     }
   }
   ```

6. 异步函数：普通函数、箭头函数和生成器函数加上 `async` 关键字
   `javascript async function foo(){ // code } const foo = async () => { // code } async function foo*(){ // code }`
   不同函数的差异：在于 this 关键字

## this 关键字

### Reference 类型

**组成部分**： 一个对象和一个属性值。

**使用：**作算术运算或者其他运算时，Reference 类型被解引用，获取真正的值来参加运算。进行类似函数调用、delete 等操作时，需要用到 Reference 类型中的对象。

### this 值

1. 普通函数的`this`值是由调用它所使用的引用决定的。我们在获取函数的表达式实际上返回的并非函数本身，而是 Reference 类型。
2. 箭头函数，不论用什么引用来调用它，都不影响它的 this 值。
3. 生成器函数、异步生成器函数和异步普通函数跟普通函数行为是一致的。异步箭头函数与箭头函数行为是一致的。

### this 关键字的机制
