# 八、`BOM`对象

## 8.1 `window`对象

**概念**：`window`对象，表示的是浏览器的一个实例，`window`对象有两重意义，一是作为`javascript`访问浏览器的一个接口，二是作为`ECMAScript`规定的全局对象。

## 全局作用域

因为`window`对象作为`ECMAScript`规定的全局对象，所以在**全局作用域下定义的变量和函数都会变成`window`对象的属性和方法**。

```javascript
var age = 18;
console.log(window.age); //18

var foo = function() {
  return "foo";
};

console.log(window.foo === foo); // true
```

<br>

全局变量与`window`对象的属性存在两点**差别**：

1. 全局变量不可以使用`delete`删除，而直接在`window`上定义的属性可以删除。

   ```javascript
   var age = 18;
   window.color = "red";

   delete window.color; // true
   delete window.age; //false
   ```

2. 访问未定义变量会报错，访问`window`未定义属性返回`undefined`
   ```javascript
   obj; // Uncaught ReferenceError: obj is not defined
   window.obj; //underfined
   ```

## 窗口关系和框架

**框架关系**：
每个框架都有自己的`window`对象，并且保存在`frames`集合中。可以通过 **_数值索引_** 或 **_`name`属性_** 访问。
`top`对象始终指向最外层框架，也就是浏览器窗口。
`parent`指向当前框架的直接上层框架。
`self`指向`window`，两者可以互换使用

## 窗口位置

**位置信息**：
`screenTop`：窗口相对于上边的位置
`screenLeft`:窗口相对于左边的位置
在**FireFox** 中，`screenX`和`screenY`属性提供与上述相同的窗口位置信息。

**跨浏览器取得窗口左边和上边的位置（非精确值）**

```javascript
var left = typeof (window.screenLeft === "number")
  ? window.screenLeft
  : window.screenX;
var top = typeof (window.screenTop === "number")
  ? window.screenTop
  : window.screenY;
```

**移动窗口**：(可能被禁用)

- `moveTo(x,y)`：移动窗口到固定位置，x、y 分别表示新位置的坐标
- `moveBy(x,y)`：将当前窗口移动到相对现在水平和垂直方向上的距离

## 窗口大小

**属性**：
`outerWidth` 和`outerHeight`：浏览器窗口本身的宽、高。
`innerWidth`和`innerHeight`：页面视图区对应宽、高。
在 IE9 以下，不支持以上属性，不过 可以通过`DOM`提供页面可见区域的相关信息。
`document.documentElement.clientWidth` 和 `document.documentElement.clientHeight`提供页面视口信息，**不包括**外边距，边框和垂直滚动条的距离。在**IE6**中，只有页面在标准模式下，才支持这些属性。在混杂模式下，必须通过`documtent.body.clientWidth`和`documtent.body.clientHeight`才能取得相同信息。
<br>
<br>
**跨浏览器取得页面视口大小(非精确值)**：

```javascript
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

**设置窗口大小**的方法：（可能被禁用）
`resizeTo`：接收浏览器窗口新的宽度及高度
`resizeBy`：接收新窗口与原窗口的宽度和高度之差

## 导航和打开窗口

**方法及属性**

- `window.open()`导航到一个既定的`url`，并返回一个指向新窗口的引用
- `window.close()` 关闭新打开的窗口，仅适用于通过`window.open`打开的弹出窗口
- `opener`：是`window`对象的一个属性，如果该窗口是使用`window.open()`方法打开的，则指向打开它的原始对象，否则为`null`

**检测弹出窗口是否被屏蔽**：通过检测`window.open()`返回的新窗口引用是否为`null`

```javascript
var blocked = false;

try {
  var wroxWin = window.open("http://www.wrox.com", "_blank");
  if (wroxWin == null) {
    blocked = true;
  }
} catch (ex) {
  blocked = true;
}

if (blocked) {
  alert("The popup was blocked!");
}
```

## 间歇调用和超时调用

**超时调用**：
`setTimeout` 即超时调用，在指定的时间过后执行代码，接收两个参数：要执行的代码和以毫秒表示的时间
`clearTimeout`：取消超时调用，很少使用，因为一次调用过后就会自行停止。

```javascript
var timer = setTimeout(function() {
  console.log("settimeout");
}, 1000);

clearTimeout(timer);
```

<br>

**间歇调用** ：
`setInterval`：间歇调用，按照指定的时间间隔重复执行代码，传入参数同`setTimeout`，返回一个 ID，可用于在将来某个时刻取消间歇调用
`clearInterval`：取消间歇调用

```javascript
var timer = setInterval(function() {
  console.log("setInterval");
}, 1000);

clearInterval(timer);
```

## 系统对话框

- `alert()`：接收一个字符串并显示一个对话框

- `confirm()`：接收一个字符串，显示一个确认对话框，并返回用户单击确定按钮的布尔值

- `prompt()`：显示“提示框”，接收两个参数，要显示给用户的文本和输入框默认值，该方法返回用户输入的值

**通过`JavaScript`打开的对话框：**

- `print()`：显示打印对话框
- `find()`：传入一个字符串，返回在该页面 查找结果的布尔值

## 8.3`location`对象

**概念**：提供了与窗口中加载的文档有关的信息，还提供了一些导航功能，它既是`window`对象的属性，也是`document`对象的属性。

**属性**：
以`https://www.baidu.com/s?keyword=javascript`为例，
属性名 | 说明 | 内容
------| ------|------
`hash`|url 中`#`后紧跟的字符串，如果没有则是空字符串 | ""
`host`| 服务器名称和端口号 | "www.baidu.com"
`hostname`| 不带端口号的服务器名称 | "www.baidu.com"
`href`| 当前加载页面的完整`url` | "https://www.baidu.com/s?keyword=javascript"
`pathname`|目录和文件名| "/s"
`port`|端口号 | ""
`protocol`| 协议，通常是`http:`或`https` |"https:"
`search`| 以?开头查询字符串 | "?keyword=javascript"

**查询字符串参数**

```javascript
function getQueryStringArgs(){

    //get query string without the initial ?
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
    var args = {},
    qs.split('&').forEach((value,key)=> {
		var items = value.split('=');
		arg[items[0]] = items[1];
	})

    return args;
}
```

**改变浏览器位置**：

1. 使用`location.assign()`方法，传入一个`url`
2. 设置`location.href` 为一个新的`url`值
3. 设置`window.location`为一个新的`url`值
4. `location.replace()`传入一个新的`url`值
5. `location.reload()`: 重载当前页面，不传递任何参数时从浏览器缓存中重新加载，如果传入`true`，则强制从服务器重新加载。

修改`location`的属性（`hash`除外），页面都会以新的`url`进行重载。

## 8.3`navigator`对象

**作用**：识别客户端浏览器

常用属性：

- `userAgent`：浏览器的用户代理字符串
- `plugins`：浏览器已经安 装的插件

## 8.4 `screen`对象

**用途**：表明客户端的能力，包括浏览器窗口外部的想使其信息

## 8.5`history`对象

**用途**：保存用户上网的历史记录

**方法**：

1. `go()`：在用户的历史记录中跳转。参数可以是整数或者字符串，如果是整数，整数表示前进，负数表示向后跳转。如果是字符创，就会挑战到历史记录中中包含该字符串的一个位置
2. `back()`等同于`go(-1)`,后退
3. `forward()` 等同于`go(1)`，前进

**属性**：`length`保存历史记录的数量
