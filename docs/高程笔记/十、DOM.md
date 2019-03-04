# 十、`DOM`

`DOM`是针对`HTML`和`XML`文档的一个`API`，用于描述层次化的节点树。

[toc]

## 节点层次

### 节点类型

节点可以分为不同的类型，表示文档中不同的信息及标记。总共有`12`种类型的节点，都继承自一个基类型。

在`DOM1`级中定义了`Node`接口，该接口由所有的节点类型实现。每个节点都拥有一个`nodeType`属性，标明节点的类型。在`Node`节点中定义了`12`个常量属性来表示节点类型。

#### 节点属性

由于节点都继承自`Node`类型，所以所有节点都共享着相同的属性和方法。基本属性：

- `nodeType`，其值为 1-12 的数字。
- `nodeName` 元素标签名
- `nodeValue` 其值为`null`

#### 节点关系

**子节点**：`childNodes`属性，保存一个`NodeList`对象，这是一种类数组结构，保存一组有序节点。

**特殊子节点**：`firstChild`和`lastChild`属性，分别指向父节点的第一个子节点和最后一个子节点。

**父节点**：`parentNode`属性，指向文档树中的父节点。
兄弟节点：`previousSibling`和`nextSibling`属性，分别指向该节点的前一个兄弟节点和后一个兄弟节点。
文档节点：所有节点都拥有的属性`ownerDocument`

访问`NodeList`中的节点，可以通过`2`种方式，方括号 或者 `item()`方法。第一种方法更加简洁。

```
var firstChild = someNode.childNodes[0];
var secondChild =  someNode.childNodes.item(1);
```

#### 操作节点

**插入节点：`2`种方法**

- `appendChild()` 用于向`childNodes`列表的末尾添加一个节点，返回新增的节点。
- `insertBefore()`将节点放在`childNodes`列表中某个特定位置上，参数：要插入的节点和参照节点，如果参照节点为`null`，效果和`appendChild()`一样。

**移除节点**：使用`removeChild()`方法，传入要移除的节点。

**替换节点**：`replaceChild()`方法，传入要插入的节点和替换的节点。

**其他方法**

1. `cloneChild()` 复制节点，传入布尔值表示是否执行深复制。深复制就会复制节点及其子节点。
2. `normalize()` 在文本节点上使用，删除空白文本节点或合并两个相邻的文本节点。

### `Document`类型

`Document`类型表示文档，在浏览器中可以通过`document`对象访问到。

#### 文档子节点

**访问**：

1. 通过`documentElement`属性，该属性指向`HTML`页面中的`<html>`元素
2. 通过`childNodes`列表

**其他属性：**

- `body`属性，指向`<body>`元素
- `docType`属性，文档类型声明

#### 文档属性

- `title`文档标题，可写

与网页请求有关的`3` 个属性：

- `URL`页面完整`url`
- `domain` 域名
- `referrer`链接到当前页面的那个页面的`url`

#### 查找元素

- `getElementById()` 接受一个元素的`id`，返回该元素，不存在则返回`null`
- `getElementByTagName()` 接受元素标签名，返回包含`0`个或多个元素的`NodeList`
- `getElementByName()`接受元素`name`值，返回带有给定`name`属性的`NodeList`

#### 一致性检测

`document.implementation` 保存浏览器对于`Dom`的具体实现。具体使用`hasFeature()`来进行检测。传入参数，要检测的`Dom`功能及版本号。

```javascript
var hasXmlDom = document.implemetation.hasFeature("XML", "1.0");
```

#### 文档写入

`write()` 和 `writeln()`都可以将输出流写入到网页中。唯一的区别是，`writeln()`会在字符串后加一个换行。
`open()`及`close()`分别用于打开和关闭网页的输出流。

### `Element`类型

`Element`类型可以用于`XML`和`HTML`元素，提供对元素标签名、子节点和特性的访问。
访问元素标签名，可以通过`nodeName`或者`tagName`。后者在`HTML`中返回大写。

#### `HTMl`元素

表示为`HTMLElement`类型，继承自`Element`类型，并增加了以下属性：

- `id` 元素唯一标识符
- `className` 与元素的`class`特性对应
- `title`附加信息
- `dir`语言方向

#### 方法

1. 取得特性：`getAttribute(propName)`
2. 设置特性：`setAttribute(propName,value)`
3. 移除特性：`removeAttribute(propName)`
4. 创建元素：`documtent.createElement(tagName)`

#### `attributes`属性

`Element`类型都拥有一个`attributes`属性，这是一个**动态集合**。包含以下方法：

- 返回属性节点：`getNamedItem(name)`
- 移除属性节点：`removeNamedItem(name)`
- 添加属性节点：`setNamedItem(node)`
- 返回特定位置的节点：`item(pos)`

`attributes`实际上是一系列属性节点的列表，亦可以通过以下方式操作，以`id`为例：

1. `获取特性`

```
  var id = element.attributes.getNamedItem("id").nodeValue;
```

2. `遍历特性`

```
for (i=0, len=element.attributes.length; i < len; i++){
    attrName = element.attributes[i].nodeName;
    attrValue = element.attributes[i].nodeValue;
    pairs.push(attrName + "=\"" + attrValue + "\"");
}
```

3. 设置特性，先取得特性节点，设置该节点`nodeValue`的值

### `Text`类型

#### 方法

- 创建文本节点：`document.createTextNode(value)`
- 文本规范化，将多个文本节点合并为一个`parentNode.normalize()`
- 根据位置分割文本节点：`textNode.splitText()`

## `DOM`操作

### 动态脚本

创建的动态脚本有两种方式：

1. 包含外部文件

```js
function loadScript(src) {
  var script = document.createElement("script");
  script.src = src;
  document.body.appendChild(script);
}
```

2. 直接插入`JavaScript`代码

```javascript
function loadScriptString(code) {
  var script = document.createElement("script");
  try {
    script.appendChild(document.createTextNode(code));
  } catch (ex) {
    script.text = code;
  }
  document.body.appendChild(script);
}
```

### 动态样式

创建动态样式同样有两种方式：

1. 使用`<link>`元素包含外部文件

   ```javascript
   function loadStyle(href) {
     var link = document.createElement("link");
     link.rel = "stylesheet";
     link.type = "text/css";
     link.href = href;
     var head = document.getElementByTagName("head")[0];
     head.appendChild(link);
   }
   ```

2. 使用`<style>`元素包含嵌入式`CSS`

   ```javascript
   function addStyle() {
     var style = document.createElement("style");
     style.type = "text/css";
     style.appendChild(document.createTextNode("body{background-color:red}")); //error in IE
     var head = document.getElementsByTagName("head")[0];
     head.appendChild(style);
   }
   ```

### 操作表格

#### `table`

属性：保存着对表格元素的引用

- `caption`
- `tBodies`
- `tFoot`
- `rows` 所有行

创建元素方法：

- `createTHead()`
- `createTFoot()`
- `createCaption()`

删除元素元素：

- `deleteTHead()`
- `deleteTFoot()`
- `deleteCaption()`
- `deleteRow(pos)`删除指定位置的行

#### `tbody`

- `insertRow(pos)` 在指定位置插入一行
- `deleteRow(pos)`删除指定位置的行
- `cells`
- `deleteCell(pos)`
- `insertCell(pos)`
