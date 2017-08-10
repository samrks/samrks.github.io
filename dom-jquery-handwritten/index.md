# jQuery 的设计思想（上）


<!--more-->

​	

## 前言

-   本节内容，把上节封装的 dom 代码，改用 jQuery 风格再次重新封装
-   jQuery 非常简单

​	

## 用 jQuery 风格重新封装

>   这节课你可能经常对自己说：我怎么没想到？！

## 准备工作

>   每节的准备工作都差不多，溜溜的用起来

新建项目目录 dom-2 >    src    >    index.html 、 main.js 、 jquery.js

### index.html

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>手写jQuery</title>
</head>
<body>
  你好
  <script src="jquery.js"></script>
  <script src="main.js"></script>
</body>
</html>
```

### jquery.js

```js
// 第一步，声明  window.jQuery 是一个函数（？！先不用管为什么是函数）
window.jQuery = function () {
  console.log(`我是jQuery`)
}
```

### main.js

```js
jQuery() // window.jQuery()    // output：我是jQuery
```

### 开启本地服务

```js
yarn global add parcel
parcel src/index.html
```

​	

​	

## 链式风格 ❤️

>   看一下我们就要实现一个什么的代码 👇

### 也叫 jQuery 风格

+   window.jQuery() 是我们提供的全局函数

### 特殊函数 jQuery

-   `jQuery(选择器) ` 用于获取对应的元素
-   但它却不返回这些元素
-   相反，它返回一个对象，称为 **jQuery 构造出来的对象** （也就是上节手写DOM中的那个 api）
-   这个对象可以操作对应的元素
-   听不懂？直接看代码！
-   [本地项目dom-2](第1阶段\7-JS编程接口\3-jQuery中的设计模型（上）)

​	

### 代码 ⭕️

#### index.html

```html
<body>
  <div class="test">你好1</div>
  <div class="test">你好2</div>
  <div class="test">你好3</div>
  <script src="jquery.js"></script>
  <script src="main.js"></script>
</body>
```

#### jquery.js

```js
// 第一步，声明  window.jQuery 是一个函数（？！）（先不用管为什么是函数）
window.jQuery = function (selector) {
  const elements = document.querySelectorAll(selector)  // 获取 selector 的全部元素（得到一个数组）
  // return elements
  // 常规操作：就直接返回这个通过选择器找到的元素。
  // 但jQuery做了反常规的操作：获取到元素后，没有返回这个元素，而是返回了可以操作这个元素的 api
  // 如下：
  // api 可以操作 elements
  // api 是个对象，里面包含各种可以操作 elements 的函数。
  // 如，addClass 就是给 elements 添加类名的函数
  const api = {
    // 函数内访问了函数外部的变量，这就是「闭包」
    addClass(className) {
      // elements 是 addClass 这个函数外部的变量
      for (let i = 0; i < elements.length; i++) { // 遍历所有获取到的元素，添加类名
        elements[i].classList.add(className)
      }
      // return null
      return api // 方法仍返回 api，api里又包含很多方法，可通过返回值继续调用.addClass 形成一个链条📌
      // 这就是链式风格 📌
    }
  }
  return api
}
```

#### main.js

```js
const api = jQuery(".test") // 通过选择器获取到元素，但返回的不是该元素
// 而是返回一个 api 对象，api 里包含很多可以控制该元素的方法
// console.log(api.addClass)
// 遍历所有获取到的元素，添加 .red 类名
api.addClass("red").addClass('blue')
// api.addClass 方法的返回值仍是 api，所以可以通过返回值继续调用.addClass 形成一个链条
```

<img src="https://i.loli.net/2020/10/24/cHqnrKFDk2jbIsi.png" alt="image-20201024184318522" style="zoom: 80%;" />

​	

###  jQuery 代码变型 1️⃣

>   下面的 return 的变化，必须理解

#### return 的骚操作  1️⃣

>   用 this 代替 api

```js
window.jQuery = function (selector) {
  const elements = document.querySelectorAll(selector)  // 获取 selector 的全部元素（得到一个数组）
  const api = {
    addClass(className) {
      for (let i = 0; i < elements.length; i++) { 
        elements[i].classList.add(className)
      }
      // return api
      return this  
      /*
      * 如果用一个对象来调用函数，那么这个函数中的this，就是前面的对象
      * obj.fn(p1) 等价于 ↓
      * obj.fn.call(obj, p1)   // 在fn中，this就是obj
      * 调用时 api.addClass("red") => 同理，在 addClass 中 this 就是 api，二者等价
      * 那 addClass 函数中，原本是 return api，就可以换成 return this
      * 注：this 的值，与调用时前面写了什么有关，只在函数被调用时才能确定this指代什么
      * */
    }
  }
  return api
}
```

#### return 的骚操作  2️⃣

>   完全去掉 jQuery 中的 `api`
>
>   +   既然先创建了 api 对象，然后返回 api 对象，那是不是可以直接返回对象，省略 api 的赋值环节 呢？
>   +   岂不是「多此一举」

```js
window.jQuery = function (selector) {
  const elements = document.querySelectorAll(selector) 
  const api = {  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
    addClass(className) {
      for (let i = 0; i < elements.length; i++) {
        elements[i].classList.add(className)
        
      }
      return this 
    }
  }
  return api    // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
}

// 省略 api 的赋值环节 👇 
window.jQuery = function (selector) {
  const elements = document.querySelectorAll(selector)  // 获取 selector 的全部元素（得到一个数组）
  // const api = {
  return {
    addClass(className) {
      for (let i = 0; i < elements.length; i++) {
        elements[i].classList.add(className)
        
      }
      return this
    }
  }
  // return api 
}
```

​	

​	

### jQuery 的核心思想

>   第一个核心点：闭包

1.  jQuery 函数，接收一个 css 选择器
2.  通过选择器，获取到这个元素 elements（但不会返回这个元素），它会返回一个对象
3.  返回的对象中，包含很多函数。这些函数都可以操作这个元素 elements

原理：

+   用「闭包」去维持这个 elements
    +   因为 addClass 函数在访问 elements。被访问的变量，是不会随便就被浏览器回收掉的
    +   这就是 jQuery 的核心思想之一

>   第二个核心点：链式操作

+   addClass 函数，肯定能猜到：用户在调用 addClass 时，肯定是通过` jQuery(选择器)` 得到的 api 来调用的
+   所以才会大胆的 return this。 
+   addClass 函数，希望把 「点 . 」前面的东西，作为 addClass 的返回值  `api.addClass("red")`
+   这样就相当于，api 从 addClass 函数前面，传递到了函数后面，这样就可以接着调用 addClass
    `👇api.addClass("red")👇.addClass("blue")`
+   这就是 「链式操作」

​	

### jQuery 代码变型 2️⃣

#### main.js 简化调用

>   去掉变量 x 

```js
const x = jQuery(".test")  // 声明出来 x ，接着直接使用。那赋值操作，显得多此一举
x.addClass("red").addClass('blue').addClass('green')

// 👇 最终写成 👇
jQuery(".test").addClass("red").addClass('blue').addClass('green')
```

​	

### 小总结

+   所谓高级的前端代码，就是把中间过程全部省掉了
+   把所有多次一举、无关紧要的东西，都尽量删掉
+   最后只留下一个最少信息的、最精炼的代码
+   虽然代码特别简洁、优雅，但对于学习者来说，就是看不懂。（说明「源码」真的不适合学习者）

​	

​	

## jQuery 是构造函数吗？

>   讲到这里可能会有这个疑问 👆

>   构造函数的特点：① 前面有 new、 ② 构造出对象
>
>   +   结合这两个特点，可以认为  jQuery 是构造函数，也可以认为不是构造函数

### 是

+   因为 jQuery 函数确实构造出了一个对象

### 不是

-   因为不需要写 new jQuery() 就能构造一个对象
-   以前讲的构造函数都要结合 new 才行

### 结论

-   jQuery 是一个不需要加 new （就可以构造出对象）的构造函数
-   jQuery 不是常规意义（严格意义）上的构造函数
-   这是因为 jQuery 用了一些技巧（目前没必要讲，讲了新手就更迷惑了）

​	

​	

## 术语

### 口头约定 👄

>   [前面](# 特殊函数 jQuery)提到：jQuery 函数，返回一个对象，称为 **jQuery 构造出来的对象 **  **（ 也就是最初代码中的那个 api ）**

口头约定：

+   以后说到 **jQuery对象** 就代指 **jQuery 函数构造出来的对象**  （为了省事，少说几个字）
+   不是说 「 jQuery 这个对象 」
+   一定要记清楚

​		

### 其他举例

-   Object 是个函数
-   **Object 对象**，表示 Object 这个构造函数 构造出来的对象（不是 Object 本身是对象）
-   Array 是个函数
-   **Array 对象/数组对象**，表示 Array 构造出来的对象（不是 Array 本身是对象）
-   Function 是个函数
-   **Function 对象 / 函数对象**，表示 Function 构造出来的对象（不是 Function 本身是对象）

​	

​	

## 更多功能的封装 ⭕️

>   链式风格

>   📌📌📌📌📌📌📌📌更多代码实现、解析、注释，请查看[本地项目 dom-2](第1阶段\7-JS编程接口\3-jQuery中的设计模型（上）) 📌📌📌📌📌📌📌📌

### 查

```js
jQuery('#xxx')                // 返回值并不是元素，而是一个api对象 
jQuery('#xxx').find('.red')   // 查找#xxx里的.red元素 
jQuery('#xxx').parent()       // 获取爸爸 
jQuery('#xxx').children()     // 获取儿子 
jQuery('#xxx').siblings()     // 获取兄弟 
jQuery('#xxx').index()        // 获取排行老几（从0开始） 
jQuery('#xxx').next()         // 获取弟弟 
jQuery('#xxx').prev()         // 获取哥哥 
jQuery('.red').each(fn)       // 遍历并对每个元素执行fn
```

#### 代码

[本地项目dom-2](第1阶段\7-JS编程接口\3-jQuery中的设计模型（上）)

```js
window.jQuery = function (selectorOrArray) {
  /*
  * elements 永远表示选择器的目标元素的集合（伪数组）
  * */
  let elements
  if (typeof selectorOrArray === "string") {  // 重载
    elements = document.querySelectorAll(selectorOrArray)
  } else if (selectorOrArray instanceof Array) {
    elements = selectorOrArray
  }
  // 👇 返回 jQuery函数 构造的对象 api（this就是这个api、api可以操作elements）
  return {
    addClass(className) {
      for (let i = 0; i < elements.length; i++) {
        elements[i].classList.add(className)
      }
      return this
    },
    find(selector) {
      let array = []
      for (let i = 0; i < elements.length; i++) {
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      return jQuery(array)  // <<<<<<<< 重点理解这句 【代码分析，见本地项目dom-2的注释】
    },
    oldApi: selectorOrArray.oldApi,
    end() {
      return this.oldApi
    },
    each(fn) {
      for (let i = 0; i < elements.length; i++) {
        fn.call(null, elements[i], i, elements)  // 遍历每项，对每一项都执行某个方法
      }
      return this
    },
    print() {
      console.log(elements)
      return this
    },
		parent() {
      const array = []
      this.each(node => {
        if (array.indexOf(node.parentNode) === -1) { // 去重 
          array.push(node.parentNode)
        }
      })
      array.oldApi = this
      return jQuery(array)
    },
    children() {
      const array = []
      this.each(node => {
        if (node.children) { 
          array.push(...node.children)  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
        }
      })
      array.oldApi = this
      return jQuery(array)
    },
    /********************* 下面是前面没提到的 *********************/
    /*
    siblings()
    index()
    next()
    prev()
    */
  }
}
```

​	

#### 练习

```js
/*
  <div id="test">
    <div class="child">1</div>
    <div class="child">2</div>
    <div class="child">3</div>
  </div>
*/
window.jQuery = function(selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }
  return {
    addClass(className){
      this.each(n=>n.classList.add(className))
    },
    find(selector){
      let array = []
      this.each(n=>{
        array.push(...n.querySelectorAll(selector))
      })
      return jQuery(array)
    },
    each(fn){
      for(let i=0;i<elements.length;i++){
        fn.call(null, elements[i], i)
      }
    }
  }
}

window.$ = window.jQuery
$('#test').find('.child').addClass('red') // 请确保这句话成功执行
```



### 增

>   只捋一捋思路，[最终代码](https://github.com/FrankFang/dom-2-prototype/blob/master/src/jquery.js)

#### 代码

```js
// 先简单回顾 dom 创建节点 👇（两种方式）
const div = document.createElement('div')  // ①传入标签名
template.innerHTML = '<div></div>'  // ②传入html结构，最后返回 template.content.firstChild
```

```js
window.$ = window.jQuery = function(selectorOrArrayOrTemplate) {
  let elements;
  if (typeof selectorOrArrayOrTemplate === "string") {
    if (selectorOrArrayOrTemplate[0] === "<") {
      // 创建 div
      elements = [createElement(selectorOrArrayOrTemplate)];
    } else {
      // 查找 div
      elements = document.querySelectorAll(selectorOrArrayOrTemplate);
    }
  } else if (selectorOrArrayOrTemplate instanceof Array) {
    elements = selectorOrArrayOrTemplate;
  }

  function createElement(string) {
    const container = document.createElement("template");
    container.innerHTML = string.trim();
    return container.content.firstChild;
  }
	
  // 返回jQuery创建的api
  return{
    appendTo(node) {
      if (node instanceof Element) {
        this.each(el => node.appendChild(el));
      } else if (node.jquery === true) {
        this.each(el => node.get(0).appendChild(el));
      }
    },
    // ...
  }
}

// 创建 div，插入到 body 中
$('<div><span>1</span></div>').appendTo(document.body) 
```



### 删

>   和dom实现逻辑一样

```js
$div.remove() 
$div.empty()  
```



### 改

>   和dom实现逻辑一样

```js
$div.text(?) // 读写文本内容  // 传了参数就是「写」，不传参数就是「读」
$div.html(?) // 读写HTML内容  // 传了参数就是「写」，不传参数就是「读」
$div.attr('title', ?）  // 读写属性 
$div.css({color: 'red'})  // 读写style // 注意方法名是css
$div.addClass('blue') 
$div.on('click', fn) 
$div.off('click', fn) ·  
```

#### 注意

+   $div 大部分时候，对应了多个 div 元素
+   一定要默认  $div 是一个数组，然后遍历它 （每个操作都要遍历）

​	

​	

## window.$ = window.jQuery

>   ```js
>   jQuery('#test') // 每次使用都要这么写，很麻烦
>   ```
>
>   什么？你嫌  jQuery  太长
>
>   +   你是对的
>   +   jQuery 这个单词，确实不好拼写（还要大小写区分）
>   +   怎么让 jQuery 变得更短呢？
>   +   还记得 bash alias 吗，添加一个别名即可

```js
// 一定在代码最后添加
window.jQuery = function (selectorOrArray){...}
window.$ = window.jQuery
```

+   之后在任何地方使用 $ 就相当于使用 jQuery
+   还可以再省事 👇

```js
window.$ = window.jQuery = function (selectorOrArray){...}   
// 写在一行上，顺序是从右向左执行
// 先执行 window.jQuery = function(){}
// 然后再把 window.jQuery 的结果，赋值给 window.$
```

+   这就是很多高级程序员会使用的写法


