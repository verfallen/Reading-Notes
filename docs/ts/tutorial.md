# `TypeScript`

[toc]

## 基础篇

### 一、数据类型

JavaScript 中数据类型分类两种：**原始数据类型**和**对象类型**。

#### 原始数据类型在 TypeScript 中应用：

##### 1. 布尔值：`boolean`

```
let isDone: boolean = false;
```

##### 2. 数值：`number`

    var num :number = 10;

##### 3. 字符串： `string`

    var name :string = "John";

##### 4. 空值：`void`

可以用于表示没有任何返回值的函数或者`undefined`和`null`

##### 5. `Null`和`Undefined`

使用`null`和`undefined`定义，是所有类型的子类型

    let num: number = undefined;

### 二、任意值：`Any`

任意值（Any）用来表示允许赋值为任意类型。

    let myFavoriteNumber: any

#### 属性和方法

任意值上可以访问任何属性或者调用任何方法，其返回的内容类型也是任意值。

#### 未声明类型变量被识别为任意值类型。

### 三、类型推论

1. 没有明确指定类型，则会根据变量的值推测出一个类型
2. 如果定义时没有赋值，则会被推断成`any`类型不被类型检查

### 四、联合类型

联合类型表示取值可以为**多种类型中的一种**。使用`|`连接。

    let myFavoriteNumber: string | number;

#### 访问属性或方法

1. 不确定变量类型时，**只能访问共有属性或方法**。
2. 联合类型变量被赋值是，会根据类型推论推断出一个类型。

### 五、对象的类型-接口

接口用来**定义对象的形状**。

    interface Person {
        name: string; //普通属性
        age?: number; //可选属性
        [propName: string]: any; //任意属性
        readonly id: number; //只读属性
    }

### 六、数组

#### 1. 「类型 + 方括号」表示法

    let fibonacci: number[] = [1, 1, 2, 3, 5];

#### 2. 数组泛型

    let fibonacci: Array<number> = [1, 1, 2, 3, 5];

#### 3. 接口

    interface NumberArray {
    	[index: number]: number
    }

### 七、函数

#### 函数定义

##### 1. 函数声明定义

    function sum(x: number,y: number): number{
        return x + y;
    }

##### 2. 函数表达式定义

    let mySum: (x: number,y: number)=> number = function(x: number,y: number){
    	return x + y;
    }

##### 3.接口定义

    interface SearchFun{
        (source: string,subString: string): boolean;
    }

#### 参数定义

##### 1. 可选参数：用`?`表示，必须在必填参数后面

##### 2. 默认参数：也是可选参数

##### 3. 剩余参数：使用数组的类型来定义

#### 重载

##### 1. 使用联合类型，_不能精确表达_

    function reverse(x: number | string): number | string{
        ...
    }

##### 2.重载多个函数类型

    function reverse(x: number): number;
    function reverse(x: string): string;
    function reverse(x: string | number): string | number{
    	...
    }

### 八、类型断言

**类型断言：**手动指定一个值的类型。

#### 语法

```
<类型>值
```

```
值 as 类型
```

### 九、声明文件

#### 1. 声明语句

用于声明第三方库的一些变量。

#### 2. 声明文件

将声明语句放到一个单独的文件中，必须以`d.ts`为后缀。

#### 3.书写声明

##### 1. 全局变量

```typescript

declare var //声明全局变量
declare let jQuery: (selector: string) => any;

declare function //声明全局方法
declare function jQuery(selector: string): any;


declare class //声明全局类,全局变量是一个类的时候，使用`declare class`来定义
declare class Animal {
    name: string;
    constructor(name: string);
    sayHi(): string;
}

declare enum //声明全局枚举类型
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

declare namespace //声明全局对象
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}

interface
type //声明全局类型
```

##### 2. 防止命名冲突

减少全局变量或者全局类型，将它们放到`namespace`下

```
declare namespace jQuery {
    interface AjaxSettings {
        method?: 'GET' | 'POST'
        data?: any;
    }
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

##### 3. 声明合并

组合多个声明语句，可以不冲突的合并起来。

```
declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

#### 4. npm 包的导出声明

##### 1. `export`导出变量

```
export const name: string;
export function getName(): string;
export class Animal {
    constructor(name: string);
    sayHi(): string;
}
```

##### 2. 混用`declare`和`export`

使用`declare`声明多个变量，最后使用`export`一次性导出

```
declare const name: string;

export { name };
```

##### 3. `export namespace`导出一个拥有子属性的对象

```
export namespace foo{
	const name: string;
}
```

##### 4. `export default`导出默认值

针对默认导出，一般将导出语句放在整个声明文件的最前面。

```
export default function foo(): string;
```

##### 5. `export =`

```
// 整体导出
module.exports = foo;
// 单个导出
exports.bar = bar;
```

#### 5.UMD 库

##### 1. `export as namespace`

```
export as namespace foo;
```

##### 2. 直接扩展全局变量

```
declare namespace JQuery {
    interface CustomOptions {
        bar: string;
    }
}
```

#### 6. 在 npm 包或 UMD 库中扩展全局变量

##### `declare global`

```
declare global {
    interface String {
        prependHello(): string;
    }
}

export {};
```

#### 7. 模块插件扩展：`declare module`

扩展模块是需要先引用原有模块然后进行扩展

    import * as moment from 'moment';

    declare module 'moment' {
        export function foo(): moment.CalendarKey;
    }

#### 8. 导入另外一个声明文件的类型

##### 1. import 导入

##### 2. 三斜线指令

- 书写全局变量的声明文件，必须**放在文件最顶端**
  `/// <reference types="jquery" />`
- 依赖一个全局变量的声明文件

#### 9. 自动生成声明文件

源码本身使用 ts 写成，则可以生成 .d.ts 声明文件。

##### 1. 命令

    tsc xxx.js -d

##### 2. tsconfig.json 添加 declaration 选项

```
{
    "compilerOptions": {
        "module": "commonjs", //模块遵循规范
        "outDir": "lib", //生成目录
        "declaration": true,
    }
}
```

#### 10. 发布声明文件

##### 1. 发布到源码（推荐），需要满足一下条件之一

- 给 package.json 中的 types 或 typings 字段指定类型声明文件地址
  `json { "types":"foo.d.ts" }`
- 在项目根目录下，编写 index.d.ts
- 针对入口文件，编写 .d.ts 文件

##### 2. 发布到 @types 下

## 进阶

### 一、`type`

#### 1. 类型别名

    type Name = string | number;

#### 2. 字符串字面量

    type EventName = "click" | 'scroll' | 'mousemove'

### 二、元组

不同类型的对象组成元组。

    let tom：[string, number]: ['Tom',52]

### 三、枚举

#### 1. 定义

##### 1. 自动赋值

枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。

    enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

##### 2. 手动赋值

    enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

枚举项与手动赋值项重复，之前的值会被覆盖。

    enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};
    console.log(Days["Sun"] === 3); // true
    console.log(Days["Wed"] === 3); // true
    console.log(Days[3] === "Sun"); // false
    console.log(Days[3] === "Wed"); // true

枚举项可以不是数字

    enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};

枚举项可以设置为小数或者负数，此时，未赋值的项依然递增 1。

    enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};
    console.log(Days["Tue"]) //2.5

#### 2.枚举项： 常数项 和 计算所得项

使用计算下个

```
enum Color {Red, Green, Blue = "blue".length}; //计算所得项
```

#### 3. 常数枚举`const enum`

常数枚举不能包含计算成员，会在编译阶段被删除。

    const enum Directions {Up, Down, Left, Right }

#### 4.外部枚举`declare enum`

只用于编译时的检查

    declare enum Directions {Up, Down, Left, Right}
