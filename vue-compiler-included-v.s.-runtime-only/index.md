# 深入理解 Vue 完整版与非完整版


<!--more-->

## 深入理解两种区别

>   Vue 完整版
>
>   Vue 非完整版（运行版）

|               | Vue 完整版                               | Vue 非完整版（运行版）                  | 评价（区别）                       |
| ------------- | ---------------------------------------- | --------------------------------------- | ---------------------------------- |
| 特点          | 有 compiler                              | 没有 compiler                           | compiler 占 40% 体积               |
| 视图          | 写在 HTML 里<br />或者写在 template 选项 | 写在 render 函数里<br />用 h 来创建标签 | h 是尤雨溪写好传给 render 的       |
| CDN 引入      | vue.js                                   | vue.runtime.js                          | 文件名不同，生产环境后缀为 .min.js |
| webpack 引入  | 需要配置 alias                           | 默认使用此版                            | 尤雨溪配置的                       |
| @vue/cli 引入 | 需要额外配置                             | 默认使用此版                            | 尤雨溪、蒋豪群配置的               |

### 特点

>   「完整版」有 compiler，「非完整版」没有 compiler

+   compiler 编译器，有编译 HTML 的功能，这个功能很复杂、占空间
+   导致「完整版」比「非完整版」大 40% 的体积

### 视图

>   比如要写一个 button

+   使用「完整版」，button 可以写在 HTML 里或者写在 template 选项里
+   使用「非完整版」，只能写在 render 函数里。用 render 函数的参数 h 函数来创建标签

+   h 是尤雨溪写好并传给 render 的，所以开发者可以获取到这个 h 函数。
    +   注意：因为 h 函数是作为 render 函数的参数传递进来的，所以参数名 h 可以任意修改，但 vue 文档里用的是 h，所以推荐不要修改，仍使用 h 作为函数名

### 引入

#### 通过 CDN 引入

>   在 index.html，通过 script 标签引入
>
>   [官方文档](https://cn.vuejs.org/v2/guide/installation.html#%E7%9B%B4%E6%8E%A5%E7%94%A8-lt-script-gt-%E5%BC%95%E5%85%A5)：用 `<script>` 标签引入，Vue 会被注册为一个全局变量

+   文件名为  vue.js，表示「完整版」
+   文件名为  vue.runtime.js，表示「非完整版」
+   如果要上线部署（生产环境），需要使用后缀为 .min.js 的文件
    +   .min.js 是去掉注释、代码压缩之后的版本，体积更小
    +   vue.min.js 和 vue.runtime.min.js

​	

#### 通过 webpack 引入

>   用 webpack 引入 vue 的配置，比较麻烦

>   通过 webpack 引入，默认使用「非完整版」（这是尤雨溪配置的）

如果想通过 webpack 引入「完整版」，可以参考

+   ⚠️ [Vue 安装文档](https://cn.vuejs.org/v2/guide/installation.html?#%E8%BF%90%E8%A1%8C%E6%97%B6-%E7%BC%96%E8%AF%91%E5%99%A8-vs-%E5%8F%AA%E5%8C%85%E5%90%AB%E8%BF%90%E8%A1%8C%E6%97%B6) 、[webpack 文档](https://webpack.js.org/configuration/resolve/#resolvealias)
+   ⭕️ [webpack 引入 Vue 的两种方式](https://www.jianshu.com/p/fb265cdb920b)

​	

#### 通过 @vue/cli 引入

>   通过 @vue/cli 创建的项目，默认使用「非完整版」（这是尤雨溪、蒋豪群配置的）

如果想引入「完整版」，可以参考

+   ⚠️[Vue Cli webpack 文档](https://cli.vuejs.org/zh/guide/webpack.html#%E7%AE%80%E5%8D%95%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%B9%E5%BC%8F)
+   ⭕️ [Issues：how to set alias](https://github.com/vuejs/vue-cli/issues/2398)、[chainWebpack 文档](https://github.com/Yatoo2018/webpack-chain/tree/zh-cmn-Hans#%E9%85%8D%E7%BD%AE-resolve-%E5%88%AB%E5%90%8D)、[Vue脚手架中配置别名📌](https://www.jianshu.com/p/0c4260120c2b)

```js
// vue.config.js
const path = require("path")
module.exports = {
  // 方法一
  chainWebpack: config => {
    config.resolve.alias
      .set("vuehaha", path.resolve("./node_modules/vue/dist/vue.esm.js")) 
    	// 给「vue完整版」取个别名，"vuehaha"（慎用符号）
  }
  // 方法二
  // configureWebpack: {
  //   resolve: {
  //     alias: {
  //       "vuehaha": path.resolve("./node_modules/vue/dist/vue.esm.js")
  //     }
  //   }
  // }
}
```

```js
// main.js 入口文件
import Vue from "vuehaha"  // 👈👈👈
new Vue({
  el: "#app",
  // 👇 完整版，可以「从index.html中」or「从template选项中」直接获取视图
  template:`
	<div>
      {{n}}
      <button @click="add">+1</button>
  	</div>
	`,
  data: { n: 0 },
  methods: {
    add() { this.n += 1 }
  }
})
```

​	

### template 和 render

>   template 和 render 总是二选一使用。如果二者同时使用，必然有一个会失效

-   template 是给「Vue 完整版」使用的
    -   完整版必须要写 html 字符串，可以写到 template 选项里，也可以写到 HTML 文件里
-   render 是给「Vue 非完整版」使用的
    -   非完整版必须要写 render 函数

​	

## Vue 实例的使用

>   需求：实现点击按钮加一。实现四种方案下的 Demo

### 方案一

>   CDN 引入 Vue 完整版（vue.js 或 vue.min.js）
>
>   +   包含编译器 compiler，视图可以直接从 HTML 里 或者 template 选项里获取
>   +   [官方文档](https://cn.vuejs.org/v2/guide/installation.html#%E6%9C%AF%E8%AF%AD) —— 编译器：用来将模板字符串编译成为 JavaScript 渲染函数的代码

>   **[在线示例](https://codesandbox.io/s/beautiful-nobel-hdhj4?file=/src/main.js)：HTML**
>
>   **[在线示例](https://codesandbox.io/s/youthful-night-h7jiq?file=/src/main.js)：template**

### 方案二

>   CDN 引入 Vue 非完整版（vue.runtime.js 或 vue.runtime.min.js）
>
>   +   没有编译器，无法编译 HTML，只能从 JS 构建视图
>   +   开发者需书写 render 函数（ h 构建方法 ）[官方文档](https://cn.vuejs.org/v2/guide/installation.html#%E8%BF%90%E8%A1%8C%E6%97%B6-%E7%BC%96%E8%AF%91%E5%99%A8-vs-%E5%8F%AA%E5%8C%85%E5%90%AB%E8%BF%90%E8%A1%8C%E6%97%B6)
>   +   h 构建方法的代码非常复杂，所以不建议使用这种方案

>   **[在线示例](https://codesandbox.io/s/quizzical-swirles-vn4w1?file=/src/main.js)**

### 方案三

>   CDN 引入 Vue 非完整版，配合 Vue Loader（[官方文档](https://vue-loader.vuejs.org/zh/#vue-loader-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)）和 .vue 文件
>
>   +   Vue Loader 可以把一个**单独的 .vue 文件**翻译成一个对象，对象包含 render 函数（h 构建方法）
>   +   开发者可以在不写 h 函数的基础上，得到 h 函数

>   Vue Loader 是 webpack 的 loader，它允许你以一种名为**单文件组件 (SFCs)** 的格式撰写 Vue 组件
>
>   +   这就是 webpack 为前端带来的可能。允许以任意形式（如 .vue 文件）组织一个对象，只要最后能通过一个 loader 把它还原成这个对象即可

>   **[在线示例](https://codesandbox.io/s/relaxed-tdd-bjums?file=/src/Demo.vue)**

​	

### 最优方案 — 方案三

>   最佳实践：**总是使用非完整版，然后配合 vue-loader 和 .vue 文件**
>
>   +   vue-loader 用于加载/翻译 .vue 文件

具体思路（为什么说这个是最优方案）

1.  保证用户体验，用户下载的 JS 文件体积更小，但**只支持 h 函数**

2.  保证开发体验，开发者可直接在 vue 文件里写 HTML 标签，而**不写 h 函数**

3.  脏活让 loader 做，vue-loader 会把 .vue 文件里开发者写的 HTML **转为 h 函数**

    +   开发者可以在不写 h 函数的基础上 ,（通过 vue-loader）得到 h 函数
    +   既保证了开发体验，又保证了用户体验
    +   ps：把所有细节**封装**到一个（类、函数、插件…）里，这就是工程师要做的



​	

## 在线搭建 Vue 项目

通常，我们会使用 @vue/cli 创建 vue 项目（sh ：vue create xxx-demo）

但还有另一种更方便的方式 —— [codesandbox.io](https://codesandbox.io/)    

>   CodeSandbox 是一个在线的代码编辑器，主要聚焦于创建 Web 应用项目。可以把它看作是一个浏览器端的沙盒运行环境，支持多种流行的构建模板，例如 create-react-app、 vue-cli、parcel 等等。 可以用于快速原型开发、DEMO 展示、Bug 还原等等。

+   打开网址，点击 create sandbox，选择 vue 图标，即可在线创建出一个 vue 项目（如下图）
+   在线项目支持下载 ZIP ，解压即用（不过通常都是测试 demo 也无需下载）

![image-20201222202934247](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201222202934247.png)

>   **提示**
>
>   [codesandbox.io](https://codesandbox.io/) 登录后只能创建 50 个项目
>
>   但不登录可以创建无限个（注意清理 cookie），所以不要登录（Do Not Sign In）😉
