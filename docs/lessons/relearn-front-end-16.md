# HTML 元信息标签

## 一、元信息标签

**元信息**： 描述自身的信息。
**元信息标签**： HTML 用于描述文档自身的一类标签，通常出现在**head 标签中**。一般不会显示。
**标签应用**：一些元信息可以产生实际的行为，例如 title,另一些仅仅是对页面描述，可以使得页面和各种浏览器和搜索引擎更好的结合。

### head 标签

**作用**： 作为盛放其他语义类标签的容器。
**使用**

- 必须是 html 标签中的第一个标签
- 必须包含一个 title，如果是 iframe ，或者有其他方式指定文档标题时，可以不包含
- 最多包含一个 base

### title 标签

**作用**：表示文档的标题。

### heading 和 title 的区别

- title 应该完整地概括整个网页内容，**原因**：title 是元信息标签，可能被用在浏览器收藏夹等多种场景
- heading 仅仅用于页面展示，它默认具有上下文，并且有辅助链接，所以可以简写。

### base 标签

**作用**： 给页面上所有的 URL 相对地址提供一个基础，历史遗留标签。
**使用建议**： base 标签会改变全局的链接地址，建议不要使用该标签，使用 JavaScript 来替代。

### meta 标签

**定义**：是一组键值对，一种通用的元信息表示标签。
**使用**：一般的 meta 标签由 name 和 content 两个属性来定义，name 表示元信息的名, content 表示元信息的值。
**基本用法示例**

```htmlbars
<meta name=application-name content="lsForums" >
```

### meta 的其他变体

**作用**：简化书写方式或者声明自动化行为。

#### 具有 charset 属性的 meta

**作用：**描述了 HTML 文档自身的编码形式。
**建议**：该标签放在 head 的第一个。

#### 具有 http-equiv 属性的 meta

**作用**：执行一个命令，这样的 meta 标签可以不需要 name 属性了。
**支持的命令**

- content-type 内容类型
- content-language 指定内容的语言
- default-style 指定默认样式表
- refresh 刷新
- set-cookie 模拟 http 头 set-cookie ，设置 cookie
- x-ua-compatible 模拟 http 头 x-ua-compatible ，声明 ua 兼容性
- content-security-policy 模拟 http 头 content-security-policy 声明内容安全策略

### name 为 viewport 的 meta

**定义**： 其 name 属性为 viewport ，它的 content 是一个复杂结构，是用逗号分隔的键值对，键值对的格式是 key=value。
**viewport 表示的全部属性**：

- width 宽度，可为数字，或是 device-width，表示设备宽度
- height 高度，可谓数字，或是 device-height，表示设备高度
- initial-scale 初始缩放比例
- minimum-scale 最小缩放比例
- user-scalable 是否允许用户缩放
  **移动端适配 meta 标签**: 禁止用户缩放功能，宽度为设备宽度。

```htmlbars
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no">
```

### HTML 中定义的 meta 标签的 name

- application-name： 如果页面是 web application，这个标签表示应用名称
- author： 页面作者
- description： 页面描述，可能用于搜索引擎或其它。
- generator： 生成页面的工具，主要是用于可视化编辑器，手写的 HTML 页面不需要加这个。
- keywords ： 页面关键字，用于 SEO
- referrer： 跳转策略，是一个安全考量
- theme-color : 页面风格颜色，不实际影响页面，但浏览器可能根据此来调整页面之外的 UI。
