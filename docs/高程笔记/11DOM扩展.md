# DOM 扩展

## 选择符

+ `querySelector()`：传入`css`选择器，返回与该选择器模式匹配的第一个元素，没有匹配则返回`null`
+ `querySelectorAll()`：传入`css`选择符，返回匹配的所有元素。
+ `matchesSelector()`：暂时没有浏览器支持这个方法，但是有一些浏览器已经实现了带有前缀的实验性`api`。接受一个`css`选择符，如调用元素与该选择符匹配则返回`true`。

## `html5`与`DOM`相关扩展

### 与类相关

方法：`getElementsByClassName()`，接收一个包含一或多个类名的字符串，返回相匹配所有元素的`NodeList`

属性：`classList`，可以通过该属性添加、删除和替换类名。该属性是`DOMTokenList`类型，该类型包含以下方法：

+ `add(value)`: 将给定的字符串添加到列表中
+ `contains(value)`：表示列表中是否存在给定的值
+ `remove(value)`: 移除值
+ `toggle(value)` 如果存在，则删除该值，如果不存在，添加它

### 焦点相关

属性： `document.activeElement` 引用`DOM`中获得焦点的元素

方法：`document.hasFocus()`用于确定文档是否获得焦点

### `HTMLDocument`相关

`readyState`属性：文档是否加载完成，可能有`loading`或`complete`两种值。
`compatMode`属性：页面的兼容模式，有两种可能的值，`CSS1Compat`表示标准模式，`BackCompat`表示混杂模式。
`head`属性：引用文档的`<head>`元素
`charset`属性：文档中实际使用的字符集
`defaultCharset`属性：当前文档的默认字符集

### 元素相关

`data-*`：自定义属性，可以通过元素的`dataset`属性来访问

### 插入标记相关的`DOM`扩展

#### **innerHTML属性**

使用：在读模式下，返回调用元素的所有子节点对应的`html`标记。在写模式下，根据指定的值创建新的`DOM`树，替换原先的所有子节点。

**限制**：无法使用该属性插入`<script>`或`<style>`等无作用域元素

**解决方法**：在前面加入一个“有作用域”元素，比如一个文本节点或者是隐藏的`<input>`

#### `outerHTML`属性

使用：在读模式下返回它的元素及所有子节点的`html`标签。在写模式下，使用指定的`html`字符串替换元素

#### `insertAdjacentHTML()`方法

使用：接受两个参数，插入位置和要插入的`html`文本

#### 内存和性能

在删除带有事件处理程序或引用其他`javascript`对象子树时，有可能导致内存占用问题。所以，在使用插入标记的方法时，**最好先手动删除要被替换的元素的所有事件处理程序和`javascript`对象属性**。

### 滚动

+ `scrollToView()` 滚动元素，如果传入`true`，则页面滚动到元素顶部与视口顶部平行。如果传入`false`,则调用元素尽可能出现在视口中。这是一个所有浏览器都支持的方法。

## 专有扩展

文档模式：页面的文档模式决定可以使用什么功能。可以通过`document.documentMode`属性知道给定页面使用什么文档模式。

`children`属性：包含元素的元素子节点

`contains()`：判断一个节点是否是调用元素的后代

### 文本相关

`innerText`属性：在读模式下，返回元素的文本节点。在写模式下，将调用元素的子节点替换为指定值的文本节点
`outerText`属性：在读模式下和`innerText`相同。在写模式下，则替换调用元素的整个节点

### 滚动

+ `scrollIntoViewIfNeeded()`根据调用元素的位置来决定页面是否滚动。如果调用元素在当前视口中，则什么也不做。
+ `scrollByLines(lineCount)` 调用元素滚动指定行高
+ `scrollByPages(pageCount)`调用元素滚动指定页面高度
