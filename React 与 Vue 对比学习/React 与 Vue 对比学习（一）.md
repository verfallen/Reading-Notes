## 写在前面
`React` 和 `Vue` 都是用于构建用户界面的`JavaScript`库。二者既有相同之处，又各有优势。例如`React`拥有庞大的生态系统 ，而`Vue`更加轻量级，上手简单。

由于二者有很多相同的地方，包括使用`虚拟Dom`，`组件化`等等。在只熟悉一种框架的情况下进行对比学习，可降低学习成本。

## 最简单的例子
首先，我们从一个简单的例子开始，二者都将渲染一个`Hello World`标题。

***[React 版本 Hello World](https://codepen.io/gaearon/pen/ZpvBNJ?editors=0010)***  

`html` 代码：
```htmlbars
<div id="root"></div>
```

`js`代码 ：
```javascript
ReactDOM.render(
	<h1>Hello, world!</h1>,
	document.getElementById('root')
);
```

[Vue 版本Hello World](https://jsfiddle.net/chrisvfritz/50wL7mdz/)
html文件：
```htmlbars
<div id="app">
	{{message}}
</div>
```
`js`文件：
```javascript
new Vue({
	el:'#app',
	data:{{
		message:'Hello World'
	}}
})
```
## 安装到本地
经过上述简单的例子，我们可能会想要在本地运行一个实际的项目。下面，我们来学习怎样安装。

***React 安装***

可以使用下列方式进行安装：
+ 引入式 ：将以下代码添加到`html`中, 这里直接引用`cdn`,也可以将核心库下载到本地进行引用。需要注意的是，以下链接使用的是开发版本，如果想要在生产环境中使用，需要进行替换。

 ```javascript
 <script src="https://unpkg.com/react@16/umd/react.development.js"> </script>
 <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
 <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
 ```
+ 官方构建工具`create React App` 
 ```
 npm install -g create-react-app
 create-react-app my-app

 cd my-app
 npm start
 ```
+ 使用包管理工具进行安装，例如`npm`

 ```
 npm install --save react react-dom
 ```

***Vue 安装***
+ 引入式
 ```
 <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
 ```
 
+ 官方构建工具`vue-cli`, 可以选择现有模板构建项目，具体可参考[这里](https://github.com/vuejs/vue-cli/tree/v2#vue-cli--)
 ```
 npm install -g vue-cli
 vue init <template-name> <project-name>
 ```
+ 使用包管理工具

 ```
 npm install vue --save
 ```
