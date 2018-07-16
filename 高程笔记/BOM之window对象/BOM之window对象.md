## BOM 之window 对象
[TOC]
### `window` 概念
`window`对象，表示的是浏览器的一个实例，`window`对象有两重意义，一是作为`javascript`访问浏览器的一个接口，二是作为`ECMAScript`规定的全局对象。
### 全局作用域
在全局作用域下定义的变量和函数都会变成`window`对象的属性和方法。

```javascript
var age = 18;
console.log(window.age);  //18

var foo = function() {
	return 'foo'
};

console.log(window.foo === foo);  // true
```
全局变量与`window`d对象的属性存在两点差别：
1. 全局变量不可以使用`delete`删除，而直接在`window`上定义的属性可以删除。
``` javascript
 var age = 18;
 window.color = 'red';

 delete window.color // true
 delete window.age //false
```
2. 访问未定义变量会报错，访问`window`未定义属性返回`undefined`
```javascript
obj; // Uncaught ReferenceError: obj is not defined
window.obj //underfined
```
### 窗口信息
#### 窗口位置
`screenTop`：窗口相对于上边的位置
`screenLeft`:窗口相对于左边的位置
在**FireFox** 中，`screenX`和`screenY`属性提供与上述相同的**窗口位置信息**。
##### 跨浏览器取得窗口左边和上边的位置（非精确值）
``` javascript
var left = typeof (window.screenLeft === "number")
  ? window.screenLeft
  : window.screenX;
var top = typeof (window.screenTop === "number")
  ? window.screenTop
  : window.screenY;
```
#### 移动窗口位置的方法(可能被禁用)
`moveTo(x,y)`：移动窗口到固定位置，x、y分别表示新位置的坐标
`moveBy(x,y)`：将当前窗口移动到相对现在水平和垂直方向上的距离
#### 窗口大小
#####属性
+ `outerWidth` 和`outerHeight`：浏览器窗口本身的宽、高。
+ `innerWidth`和`innerHeight`：页面视图区对应宽、高。

在IE9 以下，不支持以上属性，不过 可以通过`DOM`提供页面可见区域的相关信息。
`document.documentElement.clientWidth` 和 `document.documentElement.clientHeight`提供页面视口信息，**不包括**外边距，边框和垂直滚动条的距离。在**IE6**中，只有页面在标准模式下，才支持这些属性。在混杂模式下，必须通过`documtent.body.clientWidth`和`documtent.body.clientHeight`才能取得相同信息。
#### 跨浏览器取得页面视口大小(非精确值)

``` javascript
if (typeof pageWidth === "number") {
  // 判断页面是否处于标准模式
  if (document.compatMode === "CSS1Compat") {
    pageWidth = document.documentElement.clientWidth;
    pageHeight = document.documentElement.clientHeight;
  } else {
    pageWidth = document.body.clientWidth;
    pageHeight = document.body.clientHeight;
  }
}
```

#### 调整窗口大小的方法（可能被禁用）
+ `resizeTo`：接收浏览器窗口新的宽度及高度
+ `resizeBy`：接收新窗口与原窗口的宽度和高度之差
#### 窗口方法及相关属性
+ `window.open()`导航到一个既定的`url`，并返回一个指向新窗口的引用
+ `window.close()` 关闭新打开的窗口，仅适用于通过`window.open`打开的弹出窗口
+ `opener`：是`window`对象的一个属性，如果该窗口是使用`window.open()`方法打开的，则指向打开它的原始对象，否则为`null`
### `setTimeout`和 `setInterval`
`setTimeout` 即超时调用，在指定的时间过后执行代码，接收两个参数：要执行的代码和以毫秒表示的时间
`clearTimeout`：取消超时调用
``` javascript
var timer = setTimeout(function() {
  console.log("settimeout");
}, 1000);

clearTimeout(timer);
```
`setInterval`：间歇性调用，传入参数同`setTimeout`，返回一个ID，可用于在将来某个时刻取消间歇调用
`clearInterval`：取消间歇调用
``` javascript
var timer = setInterval(function() {
  console.log("setInterval");
}, 1000);

clearInterval(timer);
```
### 系统对话框
+ `alert()`：接收一个字符串并显示一个对话框给用户
![Alt text](./1531035654222.png)

+ `confirm()`：接收一个字符串，显示一个确认对话框，并返回用户单击确定按钮的布尔值
![Alt text](./1531035934129.png)

+ `prompt()`：显示“提示框”，接收两个参数，要显示给用户的文本和输入框默认值，该方法返回用户输入的值
![Alt text](./1531751185572.png)

+ `print()`：显示打印对话框
+ `find()`：传入一个字符串，返回在该页面 查找结果的布尔值
### 窗口关系及框架(frame 在web标准中已经被废除)
#### 框架层级关系

`top`对象始终指向最外层框架，也就是`window`。
`parent`指向当前框架的直接上层框架。
`self`指向`window`，两者可以互换使用
#### 访问框架对象
每个框架都拥有自己的`window`对象，并保存在`frames`集合中。在`frames`集合中，可以通过**数值索引**(从`0`开始，从左到右，从上到下)或者**框架名称** 来访问相应的`window`对象
``` htmlbars
<html>

<head>
  <meta charset="utf-8" />
  <title></title>
</head>

<body>
  <frameset>
    <frame src="frame.html" name='topFrame'>
      <frameset cols="50% 50%">
        <frame src="left.html" name='leftFrame'>
        <frame src="right.html" name='rightFrame'>
      </frameset>
  </frameset>
</body>

</html>
```
以上代码中 创建了3个框架，若要访问名为`topFrame`的框架，可以通过以下方式：
+  `window.frames[0]`
+  `window.frames["topFrame"]`
+  `frames[0]`
+  `frames["topFrame"]`
+  `top.frames[0]`
+  `top.frames["topFrame"]`