# 手写 DOM 库


<!--more-->

​	

## 前言

[本节代码地址](https://github.com/samrks/dom-1)

​	

​	

## 什么叫封装

>   可以理解成「把一些复杂的东西，打包成盒」，通过简单的命令就可使用

### 举例

-   电脑笔记本就是 CPU、内存、硬盘、主板、显卡 的封装
-   用户只需要接触显示器、键盘、鼠标、触控板等设备
-   即可操作复杂的计算机

### 接口

-   被封装的东西需要暴露一些功能给外部
-   这些功能就是**接口**，如 USB 接口、HDMI 接口
    -   接口都是需要有规范的文档来说明的
    -   全世界厂商都可以根据 USB 接口文档，来生产具有 USB 接口的硬件
    -   深圳华强北就是根据各种文档，很快的复制生产出苹果数据线 … （功能差不多、价格更便宜）
    -   这就是接口的好处，只要知道它的功能和实现细节，所有厂商都能做
-   设备只要支持这些接口，即可与被封装的东西通讯
  
    -   比如在键盘上打字，计算机就能接收到我们敲了哪个键
-   比如键盘、鼠标支持 USB 接口
-   显示器支持 HDMI 接口
    -   全世界所有显示器厂商的产品，都可以连接到任何一台电脑，就是因为有**接口的统一标准**存在
    -   旧的有：VGA 接口（体积大、传输慢）
    -   最新的有：雷电接口、HDMI 接口 （都有新的标准）

	

（示意图）

 <img src="https://i.loli.net/2020/10/22/7AkXGVYdCRwqB53.png" alt="image-20201022031151166" style="zoom: 50%;" />



 <img src="https://i.loli.net/2020/10/22/xD6BaYXcRmWoe1z.png" alt="image-20201022031203955" style="zoom: 50%;" />

本节的实现的《我的库》里面就封装了 DOM 的各种奇葩操作

+   document.getElementById   单词太长，封装后就叫 get 或者 find
+   封装成一个 create 就可以实现创建元素，不需要写 document.createElement … 这么复杂的单词
+   封装出来的  get、find、create  这些函数，就是接口
+   所有的页面中，都可以调用这些接口

	

	

	

## 术语

### 库

-   我们把提供给其他人用的工具代码，叫做「库」
    -   就是把一些好用的函数统一放到一个地方，这个地方就是「库」
-   比如 jQuery、Underscore 它们就是库（提供了很多函数，供用户调用）

### API

+   「库」暴露出来的函数或属性（功能）叫做 API（应用编程接口）
+   API： Application Programming Interface

### 框架

-   当你的库变得很大，并且**需要学习才能看懂**
-   那么这个库就叫「框架」，比如 Vue / React

### 注意

-   编程界的术语大部分都很随便，没有固定的解释
    -   可能程序员写了套东西，涵盖很多内容，作者本人也搞不清楚应该怎么定性，就随意的称为「库」
    -   如果遇到有人反驳，那就慢慢讨论、定性
-   所以意会即可
    -   我们就把「小的功能」叫库，「大的功能」叫框架

	

	​	

	

## 封装技术

>   下面我们开始学习封装技术

>   会用两种不同的风格，封装 DOM 操作
>
>   1.  对象风格（命名空间风格）
>   2.  链式风格（ jQuery 风格）

​	

​	

## DOM 库的初始化 ⭕️

>   创建 dom-1 项目目录  >  src 目录  >   index.html、main.js、dom.js

### index.html

```html
<body>
  示例
  <!-- 注意：要先引入 dom.js；
       否则 main 中先引用了dom.js的 API 就会报错：dom is not defined -->
  <script src="dom.js"></script>
  <script src="main.js"></script>
</body>
```

### dom.js

>   dom 库（对象） 和 封装的函数（create），有两种呈现关系的形式

1

```js
window.dom = {}
dom.create = function () {}   // window.dom.create 省略前缀 window
```

2

```js
window.dom = {
  create: function () {}
}
//  👆可进一步简化：省略 function   // ES6
window.dom = {
  create() {}
}
```

### 举例：封装 create 代码

>   更多代码，请直接查看[地址](https://github.com/samrks/dom-1)

>   create  创建节点

#### 写法 1

+   调用时填入要创建的标签名

```js
window.dom = {}
dom.create = function (tagName) {
  return document.createElement(tagName)
}
```

#### 写法 2

##### 初版有 bug

+   调用时直接填入标签结构
+   但填入 td / tr / tbody … 这种表格内的标签，就会返回 undefined。
+   这些标签不能直接放入 div 中，通常需要外层有 table 标签包裹才行

```js
dom.create = function (html) {
  // const container = document.createElement('div')  
  const container = document.createElement('template')
  // 如果容器是 div ，不能容纳 td ... 等
  // 使用 <template></template> 作为容器，可以容纳任意元素。
  container.innerHTML = html
  return container.children[0]
}
```

##### 正确代码

```js
dom.create = function (html) {
  const container = document.createElement("template")
  container.innerHTML = html.trim()
  // trim 去掉字符串的两端空格
  // 因为使用firstChild获取元素，如果传入的html前面有空格，就会只获取到空格(文本元素)，而不是标签元素。
  // 所以必须提前trim()一下
  return container.content.firstChild
}
```

### main.js

#### 写法 1 的调用

```js
const div = dom.create("div")
console.log(div)   // (标签) <div></div>
```

#### 写法 2 的调用

```js
const div = dom.create("<div><span>123</span></div>")
console.log(div)   // (标签) <div><span>123</span></div>
```







## 对象风格

### 也叫 命名空间风格

+   window.dom 是我们提供的全局对象

	

> 下面从增删改查 4 个方面，来说明 window.dom 是干什么的（代码量很大哦）、

>   [本节代码地址](https://github.com/samrks/dom-1)

### 增

>   [after()](https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/after)：2020刚出的
>
>   [insertBefore](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/insertBefore) 语法：`父节点.insertBefore(要插入的子节点，插入到哪个子节点的前面)`

```js
dom.create（'<div>hi</div>')   // 用于创建节点
dom.after(node,node2)          // 用于新增弟弟
dom.before(node,node2)         // 用于新增哥哥
dom.append(parent,child)       // 用于新增儿子
dom.wrap(`<div></div>`)        // 用于新增爸爸
```

代码

```js
window.dom = { 
  create(string){ // 创建节点
    const container = document.createElement("template")
    container.innerHTML = string.trim()
    return container.content.firstChild
  },
  after(node, node2){ // 新增兄弟节点
    node.parentNode.insertBefore(node2, node.nextSibling)
  }, 
  before(node, node2){ // 新增兄弟节点
    node.parentNode.insertBefore(node2, node)
  },
  append(parent, node){ // 新增子节点
    parent.appendChild(node)
  }, 
  wrap(node, parent){ // 新增父节点
    dom.before(node, parent) 
    dom.append(parent, node) 
  } 
}；
```

​		

### 删

```js
dom.remove(node)用于删除节点 
dom.empty(parent)用于删除后代
```

代码

```js
window.dom = {
  remove(node){
    node.parentNode.removeChild(node)
    return node
  },
  empty(node){
    let array = []
    let x = node.firstChild   
    // 这块是讲数据结构时最常用的思路，类似用循环实现的递归（不停找下一个，直到全删完了）
    while (x) {
      array.push(dom.remove(x))
      x = node.firstChild 
    }
    return array
  }
}
```

​		

### 改

>   用到了【重载】和【适配】
>
>   +   重载：传不同个数的参数，执行不同的代码
>   +   适配：做很多判断（js 数据类型），什么情况下执行这句、什么情况下执行那句

```js
dom.attr(node, 'title', ?)        // 用于读写属性 
dom.text(node, ?)                 // 用于读写文本内容 
dom.html(node, ?)                 // 用于读写HTML内容 
dom.style(node, {color: 'red'})   // 用于修改 
dom.class.add(node, 'blue')       // 用于添加 
dom.class.remove(node, 'blue')    // 用于删除
dom.on(node, 'click', fn)         // 用于添加事件监听 
dom.off(node, 'click', fn)        // 用于删除事件监听
```

代码

```js
dom.attr = function (node, name, value) {
  if (arguments.length === 3) {
    node.setAttribute(name, value)
  } else if (arguments.length === 2) {
    return node.getAttribute(name)
  }
}

dom.text = function (node, string) { 
  if (arguments.length === 2) { 
    if ("innerText" in node) {
      node.innerText = string
    } else {
      node.textContent = string
    }
  } else if (arguments.length === 1) {
    if ("innerText" in node) {
      return node.innerText
    } else {
      return node.textContent
    }
  }
}

dom.html = function (node, string) {
  if (arguments.length === 2) {
    node.innerHTML = string
  } else if (arguments.length === 1) { 
    return node.innerHTML
  }
}

dom.style = function (node, name, value) {  // 3种调用形式
  if (arguments.length === 3) {
    node.style[name] = value
  } else if (arguments.length === 2) {
    if (typeof name === "string") {
      return node.style[name]
    } else if (name instanceof Object) { 
      const object = name
      for (let key in object) {
        node.style[key] = object[key]
      }
    }
  }
}

dom.class = {
  add(node, className) {
    node.classList.add(className)
  },
  remove(node, className) {
    node.classList.remove(className)
  },
  has(node, className) {
    return node.classList.contains(className)
  }
}

dom.on = function (node, eventName, fn) {
  node.addEventListener(eventName, fn)
}

dom.off = function (node, eventName, fn) {
  node.removeEventListener(eventName, fn)
}
```

​	

### 查

```js
dom.find('选择器')    // 用于获取标签或标签们
dom.parent(node)     // 用于获取父元素
dom.children(node)   // 用于获取子元素
dom.siblings(node)   // 用于获取兄弟姐妹元素
dom.next(node)       // 用于获取弟弟
dom.previous(node)   // 用于获取哥哥
dom.each(nodes, fn)  // 用于遍历所有节点
dom.index(node)      // 用于获取排行老几
```

代码

```js
dom.find = function (selector, scope) {
  return (scope || document).querySelectorAll(selector) 
}

dom.parent = function (node) {
  return node.parentNode
}

dom.children = function (node) {
  return node.children
}

dom.siblings = function (node) {
  return Array.from(node.parentNode.children).filter(n => n !== node)
}

dom.next = function (node) {
  let x = node.nextSibling
  while (x && x.nodeType === 3) { 
    x = x.nextSibling
  }
  return x
}

dom.previous = function (node) {
  let x = node.previousSibling
  while (x && x.nodeType === 3) {
    x = x.previousSibling
  }
  return x
}

dom.each = function (nodeList, fn) {
  for (let i = 0; i < nodeList.length; i++) {
    fn.call(null, nodeList[i])
  }
}

dom.index = function (node) {
  const list = node.parentNode.children
  let i
  for (i = 0; i < list.length; i++) {
    if (list[i] === node) {
      break
    }
  }
  return i
}
```

​	

## 总结1

+   上面代码，除了「创建节点，用了 template」，其他方法基本都是使用 DOM 的原生 API 来实现
+   不管是多么高深的库，最后都是用 if-else、for循环、while循环 就搞定了
    +   不论什么语言，实现逻辑只需要三种表达形式：顺序执行、if/else、循环

​	

## 总结2

+   最好的学习方法，就是去看作者的思路，照着他的思路实现一下。比如，vue的作者，就把所有思路写在 vue 文档里，就去看文档就好了。

​	

>   这是许多程序员多年摸索出来的经典代码。
>   你只需要站在巨人的肩膀上，继续向上探索

​	

