# antd 实战教程

## umi

umi 既是编译工具，也是前端框架。对 webpack，react-router 等进行封装。

### 使用

1. 安装

   ```
    npm install -g umi
   ```

2. 配置文件：二选其一
   - `config/config.js`
   - `.umirc.js`
3. 新建页面并配置路由,不配置则使用约定路由，按照`page`下的文件名映射

   1. 新建页面于 src/pages
   2. config.js 配置路由

   ```
   {routes:[{path:'/mypage',component:'./MyPage'}]}
   ```

4. 启动命令
   - 启动: umi dev
   - 构建: umi build
5. 添加 umi-plugin-react 插件

   1. 安装 npm install umi-plugin-react --save-dev
   2. 配置 config.js

   ```
   plugins: [
   	['umi-plugin-react',{// 这里暂时还没有添加配置，该插件还不会有作用，我们会在后面的课程按照需求打开相应的配置 }]
   ]
   ```

### 路由配置

- 在 `/config/config.js` 中 `exports.routes` 中 配置
- 配置项

  - `path` 表示页面访问路径
  - `component` 表示 page 下的文件名
  - `routes`：当需要有一个 layout 作为展示，可以设置 routes
  - `proxy` : 代理设置
  - `theme`：主题配置

  ```
   theme: {
      "@primary-color": "#30b767",
    }
  ```

### 测试

umi 内置了 jest 测试
文件夹：test/xxx.test.js
命令：umi test

## antd 组件

### 使用

1. 安装
2. 在 umi 配置中打开插件

```
plugins: [
	['umi-plugin-react', {
		antd: true
	}],
],
```

### 布局

- 配置路由

```
 routes: [{
      path: '/',
      component: '../layout',
      routes: [
        {
          path: 'helloworld',
          component: './HelloWorld'
        },
      ]
    }],
```

- 组成 - 顶部导航 Header
  - 侧边栏 Sider
  - 内容区 Content

### Form

创建高阶组件，为页面组件提供表单所需要的内容

1. `Form.create()(Component)`
2. `getFieldDecorator` 双向绑定

#### 自定义控件

1. 提供受控属性 value 或其它与 valuePropName 的值同名的属性。
2. 提供 onChange 事件或 trigger 的值同名的事件。
3. 不能是函数式组件。

### 国际化

- antd 国际化

  1. 引入内置语言资源
  2. 使用`LocalProvider` 组件包裹它，设置`locale`属性

- 业务组件国际化
- react-intl 1. 安装 2. `IntlProvider` 组件包裹
- umi 1. 配置插件 2. `src/locale` 文件放置语言资源 3. `formatMessage`方法 或 `FormatMessage`组件

## dva

基于 `redux`、`redux-saga` 和 `react-router` 的轻量级前端框架

### model

`name`: 命名空间
`state`：初始值
`reducers`：处理同步操作
`effects`：处理异步操作
`action`：`reducers` 及 `effects`的触发器

### 使用

1. 引入 DVA

```
  export default {
    plugins: [
      ['umi-plugin-react', {
        antd: true,
        dva: true,
      }],
    ],
    // ...
  }
```

2. 定义 model

   - `namespace`: 命名空间
   - `state`：初始值
   - `reducers`：同步操作

     `reducer`是纯函数，接收旧`state`和`action`中的`payload`作为入参，返回新的`state`

   - `effects`：处理异步操作

     局部上看 `effect` 就是一个一个的 `generator function`。宏观上看，effect 是一层中间件

     参数：`action`对象和 `effect`原语集。其中`call`与`yield`配合处理异步逻辑，`put`与`yield`配合用来派发一个`action`，功能与`dispatch`一样。

     ```
     getData: function* ({ payload }, { call, put }) {
     		  const data = yield call(SomeService.getEndpointData, payload, 'maybeSomeOtherParams');
     		  yield put({ type: 'getData_success', payload: data });
     }
     ```

   - `action`：触发器

     形如{type:'action',payload:{}}
     type: namespace 名称+"/"+reducer/effect 名称

3. `connect`将`state` 注入组件

```
 @connect(mapStateToProps,mapDispatchToProps)
```

mapStateToProps：入参为整个 state
mapDispatchToProps： 入参为 dispatch
