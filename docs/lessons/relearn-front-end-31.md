# 表达式语句

构成（优先级从高到低）

1. **Primary Expression 主要表达式**
   - 基本类型直接量，数字，字符串等
   - 对象类型直接量，函数，数组等，再试用函数，`{}`和 `class`开头的表达式语句时，应该加上括号避免语法冲突
   - `this`或变量
   - 任何表达式加上`()`
2. **MemberExpression 成员表达式**
   用于访问对象成员，共 6 种

   ```yaml
   # 标识符属性访问
   a.b;
   #字符串属性访问
   a["b"];
   #判断函数是否被new 调用
   new.target;
   #访问父类属性的语法
   super.b;
   # 带函数的末班，把模板的各个部分算好后传递给一个函数
   f`a${b}c`;
   # 带有参数列表的new运算
   new Cls();
   ```

3. **NewExpression New 表达式**
   指的是没有参数列表的表达式，成员表达式加上`new`就是 **NewExpression**，例如`new new Cls()`
4. **CallExpression 函数调用表达式**
   在成员表达式后加一个括号里的参数列表，或者可以用上`super`关键字代替成员表达式。

   ```javascript
   a.b(c);
   super();
   ```

5. **LeftHandSideExpression 左值表达式**
   放到等号左边的表达式，**调用表达式**和**New 表达式**的统称。

   ```javascript
   a().c = d;
   ```

6. **AssignmentExpression 赋值表达式**
   - 等号赋值
   - 结合运算符的赋值，如`a += b`
7. **Expression 表达式**
   使用`,`连接的赋值表达式，使用逗号分隔的表达式会顺次执行，整个表达式的结果就是**最后一个表达式的结果**
