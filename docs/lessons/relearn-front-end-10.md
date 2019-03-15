# `CSS`语法：除了属性和选择器的@规则

1. **系统性学习`CSS`的线索**：`CSS`语法。
2. **`CSS`顶层样式表规则构成**：一种是`at`规则，另一种是普通规则。
3. **`at`规则**：由一个`@`关键字和后续的一个区块组成，如果没有区块，则以分号结束。使用频率远远小于普通的规则。

   - **@charset**：用于提示`css`文件使用的字符编码方式，如果被使用则必须出现在最前面。

   ```css
   @charset "utf-8";
   ```

   - **@import**：用于引入一个`css`文件，除了`@charset`不会被引入，`@import`可以引入另一个文件的全部内容。可以支持`supports`和`media query`形式。

   ```css
   @import "mystyle.css";
   @import url("mystyle.css");
   ```

   - **@media**：能够对媒体的类型进行判断。在`media`区块中，包含普通规则列表。
     `css @media print { body {font-size: 10pt} }`
   - **@page**：用于在打印文档时修改某些 CSS 属性。你不能用`@page`规则来修改所有的`CSS`属性，而是只能修改 margin,orphans,widow 和 page breaks of the document。对其他属性的修改是无效的。

   ```css
   @page {
     size: 8.5in 11in;
     margin: 10%;

     @top-left {
       content: "Hamlet";
     }
     @top-right {
       content: "Page " counter(page);
     }
   }
   ```

   - **@counter-style**：产生一种数据，用于定义列表项的表现。

   ```css
   @counter-style triangle {
     system: cyclic;
     symbols: ‣;
     suffix: " ";
   }
   ```

- **@key-frames**：产生一种数据，用于定义动画关键帧。
  `css @keyframes diagonal-slide { from { left: 0; top: 0; } to { left: 100px; top: 100px; } }` + **@font-face**：用于定义一种字体，icon font 技术则是这样实现的。
  `@font-face { font-family: Gentium; src: url(http://example.com/fonts/Gentium.woff); } p { font-family: Gentium, serif; }` + **@supports**：检查环境的特性，和`media`比较类似。
  `css @supports (animation-name: test) { … /* 如果支持不带前缀的animation-name,则下面指定的CSS会生效 */ @keyframes { /* @supports是一个CSS条件组at-rule,它可以包含其他相关的at-rules */ … } }` + **@namespace**：用于跟`XML`命名空间配合的一个规则，表示内部的`css`选择器全部带上特定命名空间。 + **@viewport**: 用于设置视口的一些特性，多数被`HTML`的`meta`代替。 + 其他 + **@color-profile**：`SVG1.0`引入的`css`特性，实现状况不好。 + **@document**：被推迟到了`css4`中。 + **@font-feature-values**

4. **普通规则**：主要由**选择器**和**声明区块**构成。声明区块又由属性和值构成。
5. **选择器**：
   1. **结构连接**： 由几个符号连接，空格，**>**,**+**,**~**,**||**。其中，空格也就是后代选择器的优先级较低。
   2. **组成**：如果不是伪元素，则由**标签类型选择器**，**id**，**class**,**属性**和**伪类**。如果是伪元素，则在这个结构之后追加伪元素，只有伪元素可以出现在伪类之后。
6. **声明区块**：属性和值。**属性**是由中划线、下划线，字母等组成的标识符。但是不支持使用两个中划线开头，这样的属性被看做**CSS 变量**。使用时，配合`var`函数。

   ```css
   :root {
   	--main-color: #06c;
   	--accent-color： #006；
   }
   #foo h1{
   	color: var(--main-color);
   }
   ```

7. **`CSS`属性值**可能的类型：
   - `CSS`范围的关键字：`initial`，`unset`，`inherit`,任何属性都可以的关键字
   - 字符串：例如`content`属性
   - URL：使用`url()`函数的 URL 值。
   - 整数/实数：比如`flex`属性
   - 维度：单位的整数/实数，比如`width`属性。
   - 百分比：大部分维度都支持
   - 颜色：比如`color`属性
   - 图片：比如`background-image`属性
   - 2D 位置：比如`background-position`属性
   - 函数：来自函数的值，比如`transform`
8. **计算型函数**：
   - `calc()`：基本的表达式运算，支持加减乘除四则运算。在进行维度计算式，支持不同单位混合运算。

     ```css
		 section { width: calc(100%/3 - 2*1em - 2*1px); }
		 ```

   - `max()`取两数中较大的一个
   - `min()`取两数中较小的一个
   - `clamp()` 给一个值限定一个范围，超出范围外则使用范围的最大值或者最小值。
   - `toggle()` 在选中多个元素时生效，会在几个值之间来回切换。
   - `attr()` 允许`CSS`接受属性值的控制。
