# `JavaScript`对象：我们真的需要模拟类吗？

1. 原型系统概括：
   1. 如果所有对象都有私有字段 `[[prototype]]`，就是对象的原型；
   2. 读一个属性，如果对象本身没有，则会继续访问对象的原型，直到原型为空或者找到为止。
2. `ES6`提供的内置函数，直接地访问操纵原型
   1. `Object.create` 根据指定的原型创建新对象，原型可以是 `null`；
   2. `Object.getPrototypeOf` 获得一个对象的原型；
   3. `Object.setPrototypeOf` 设置一个对象的原型。
3. `new` 操作符具体做了那些事？
   **答：** 以构造器的`prototype`属性为原型，创建新对象；将`this`和调用参数传给构造器，执行；如果构造器返回的是对象，则返回，否则返回第一步创建的对象。
4. 用构造器模拟类的两种方法（不建议使用）
   1. 直接在构造器中修改`this`,给`this`添加属性
   2. 修改构造器的`prototype`属性指向的对象
5. 类的基本写法

   ```javascript
   class Rectangle {
     constructor(height, width) {
       this.height = height;
       this.width = width;
     }
     // Getter
     get area() {
       return this.calcArea();
     }
     // Method
     calcArea() {
       return this.height * this.width;
     }
   }
   ```

   6. 继承： 使用`extends`关键字实现继承，会自动设置`constructor`，并且自动调用父类的构造函数。
