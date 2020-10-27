# jQuery 的设计思想（下）


<!--more-->

​	

## 命名风格

>   命名风格：我们在写代码时都会有一些风格，这些风格可能是业界共识、也可能是自己的小技巧
>
>   +   下面介绍一个命名风格（以前可能是业界共识，但现在已经不太常用了，因为 jQuery 很少人用了）

### 下面的代码令人误解

```js
const div1 = document.querySelector('.test')
const div2 = $('.test')
// div2 到底是 DOM 对象，还是 jQuery 对象? 
```

+   「DOM 对象」只能使用 DOM API，如 querySelector、appendChild …
+   「jQuery 对象」只能使用 jQuery 的 API，如 find、 addClass …

```js
const div = $('div#test')
```

-   我们会误以为 div 是一个 DOM 
-   实际上 div 是 jQuery 构造的 api 对象
-   怎么避免这种误解呢？

### 改成这样

```js
const elDiv1 = document.querySelector('.test')
const $div2 = $('.test')
```

>   -   声明变量用来表示 DOM 对象，可以变量名可以前置： el  （可选）
>
>   -   声明变量用来表示 jQuery 产生的 api 对象，变量名前 + $

```js
const $div = $('div#test')
```

-   $div.appendChild 不存在，因为它不是 DOM 对象
-   $div.find 存在，因为它是 jQuery 对象



>   代码中，所有 $ 开头的变量，都是 jQuery 对象
>
>   +   这是约定，除非特殊说明

​	

​	

​	



## jQuery 代码

>   当前已经实现的代码

```js
window.$ = window.jQuery = function (selectorOrArray) {
  /*
  * elements 表示通过选择器找到的目标元素组成的伪数组
  * */
  let elements
  if (typeof selectorOrArray === "string") {  // 重载
    elements = document.querySelectorAll(selectorOrArray)
  } else if (selectorOrArray instanceof Array) {
    elements = selectorOrArray
  }
  // ↓ api 可以操作 elements（this 就是 jQuery 返回的 api）
  return {
    addClass(className) {
      for (let i = 0; i < elements.length; i++) {
        elements[i].classList.add(className)
      }
      return this
    },
    find(selector) {
      console.log(`elements`, elements)
      let array = []
      for (let i = 0; i < elements.length; i++) {
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      console.log(`array`, array)
      array.oldApi = this
      return jQuery(array)
    },
    oldApi: selectorOrArray.oldApi,  // 在 find 中，通过 array 保存下来的旧的 api
    end() {
      return this.oldApi
    },
    each(fn) {
      for (let i = 0; i < elements.length; i++) {
        fn.call(null, elements[i], i, elements)
      }
      return this // this 就是 api !!!
    },
    print() {
      console.log(elements)
      return this
    },
    parent() {
      const array = []
      // 遍历父元素 ↓
      this.each(node => {
        if (array.indexOf(node.parentNode) === -1) {  // 去重
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
          array.push(...node.children)
        }
      })
      array.oldApi = this
      return jQuery(array)
    }
    /******************************************/
  }
}
```

### 调用

```js
const api1 = $('.red')  // api1 用来操作 red 元素
const api2 = $('.blue')  // api2 用来操作 blue 元素
/*
* 由上，可以发现 api1 和 api2 重复了 （?!! 什么意思）
* */
```

## 发现问题 🎃 api1 和 api2 重复了

### 分析 🎃

>   api1 和 api2 重复了 （?!! 什么意思）

<img src="https://i.loli.net/2020/10/25/NVrI7HihbMPqkYn.png" alt="image-20201025172830173" style="zoom: 50%;" />

+   api1 对应一块内存 #101
    +   在 #101 中，有 find （#201）、each（#209）
    +   内存 #201 对应一个find函数、内存 #209 对应一个each函数
+   api2 对应内存 #409
    +   在 #409 中，有 find （#509）、each（#519）
    +   内存 #509 对应一个find函数、内存 #519 对应一个each函数
+   可以比较清楚的发现：
    +   两个 find 、两个 each 实际上应该是同一个函数的实现
    +   但 jQuery 每创建一个 api ，这些函数也都被再次创建了一遍  
    +   201和509（209和519）是完全一样的两块内存
+   这就是前面提到的「 api1 和 api2 重复了」

### 解决方法 🎃

+   find 和 each 应该作为「共用属性」

+   那为什么不把共用的属性写到一个对象上去呢？

    +   用一个 \_\_proto\__ 属性保存下共有属性的地址，即可
    +   把这个包含共有属性的对象，放到 jQuery 上，让 jQuery.prototype 等于这个对象

    ![image-20201025174554791](https://i.loli.net/2020/10/25/5qYbPpcQJHlfyn2.png)

#### 套用原型公式

```js
对象.__proto__ === 其构造函数.prototype
```

```js
const api1 = $('.red')
api1.__proto__ === jQuery.prototype   // 原型公式
const api2 = $('.blue')
api2.__proto__ === jQuery.prototype
```

​	

​	

## 使用原型 改造 jQuery ⭕️

### 第一版 jQuery 代码

>   先看看之前的版本

```js
window.$ = window.jQuery = function (selectorOrArray) {
  let elements
  if (typeof selectorOrArray === "string") {  // 重载
    elements = document.querySelectorAll(selectorOrArray)
  } else if (selectorOrArray instanceof Array) {
    elements = selectorOrArray
  }
  // ↓ api 可以操作 elements（this 就是 jQuery 返回的 api）
  return {
    jquery: true,
    oldApi: selectorOrArray.oldApi,  // 通过 array 保存下上一次的api
    // 下面是各种功能函数
    addClass(className) {...},
    find() {...},
    each(fn) {...},
    appendTo() {...},
    ...
  }
}
```

### 使用原型进行改造 ⭕️

#### ① 共有属性（函数）转移到原型上

>   把所有共有属性，都移到 jQuery 的原型 prototype 上
>
>   别忘了 constructor

+   这里是直接给 jQuery.prototype 赋新值，这样写很方便，但是会导致原本原型上的 [constructor]() 被覆盖丢失，所以要手动加回去 [constructor]()

```js
jQuery.prototype = {
  constructor: jQuery,
  jquery: true,
  // 下面是各种功能函数
  addClass(className) {...},
  find() {...},
  each(fn) {...},
  appendTo() {...},
  ...
}
  
```

#### ② 把 prototype 赋予 jQuery 创建的对象

+   [Object.create()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) ：创建对象，并给对象指定原型 \_\_proto__

```js
window.$ = window.jQuery = function (selectorOrArray) {
  let elements
  // ...
  
  const api = Object.create(jQuery.prototype)  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
  // 创建 api对象，并指定其原型是 jQuery.prototype
  // 相当于 const api = {__proto__: jQuery.prototype}
  ...
  
  return api
}
```

#### ③ 往 api 上添加 elements 、oldApi

>   问题分析：
>
>   当我们把所有功能函数都从 api（独立函数） 上拿走，放到原型（独立对象）上之后
>   原型上的函数就获取不到 jQuery 里的 elements ，elements 仅作用在在 window.jQuery 函数中

```js
window.jQuery = function(p1){ 
  let elements = document.querySelectorAll(p1)
  return api
};
jQuery.prototype = { 
  get(index){ return elements[index] },    // 这里的elements获取不到上面的 elements
  end(){return this.oldApi},
  find(){ }, 
  // ...
};
```

+   那 find、get … 怎么操作 elements 呢？

    +   每生成新的 elements，都会创建、返回新的 api（api 的原型上就是这些功能函数）

    +   **需要找到它们之间的联系**

        ```js
        $('.test').addClass('red')  
        // addClass 函数中的 this 指向函数调用者 $('.test')
        // 而 $('.test') === api ，所以函数中的 this 指向的就是 api
        ```

    +   所以要想在函数内部，获取到 elements，可以把 elements 放到 api 上

+   那就在 api 上添加一个属性 elements，用来保存  jQuery 创建的 elements

+   在原型里的函数中，通过 [this.elements]()，就可以访问到这个【目标元素的数组】，加以操作

>   综上：
>
>   -   在原型里的函数，要获取到 jQuery 里的变量。必须通过桥梁【 api 、this 】
>       -   **桥梁：只要 api 上有 jQuery 里的变量，函数就能通过 this 关键字获取到这个变量**

注：oldApi 代码如下

```js
end(){
  return this.oldApi  
  // end 原本就是要操作【调用者api】中的 oldApi
  // 所以 this(api) 上必须有 oldApi，end才能操作到，所以同样也需要往 api 上添加 oldApi
}
```

[完整代码](D:\Jirengu\第1阶段\7-JS编程接口\4-jQuery中的设计模型（下）\dom-2-github-prototype\src)

+   [Object.assign](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)：把后面对象的属性，复制到前面的对象上（注意是浅复制，JS 本身没有深复制）

```js
window.$ = window.jQuery = function (selectorOrArray) {
  let elements
  // ...  elements = document.querySelectorAll(selectorOrArray)
 
  const api = Object.create(jQuery.prototype)
  // 把所需变量放到 api 上（两种方式）
  // （方式一 👇）
  // api.elements = elements 
  // api.oldApi = selectorOrArray.oldApi // 等同于 ↓
  // （方式二 👇）
  Object.assign(api, {  // Object.assign（两个参数）把后面对象的属性，复制到前面的对象上
    elements: elements,
    oldApi: selectorOrArray.oldApi
  })
  
  return api
}

jQuery.prototype = {  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 函数在这里
  constructor: jQuery,
  jquery: true,

  // 所有对 elements 的调用，都改成 this.elements
  get(index) {
    return this.elements[index]
  },
  addClass(className) {
    for (let i = 0; i < this.elements.length; i++) {
      const element = this.elements[i]
      element.classList.add(className)
    }
    return this
  },
  end() {
    return this.oldApi // this 就是新 api
  }
  find(selector) {
    let array = []
    for (let i = 0; i < this.elements.length; i++) {
      const elements2 = Array.from(this.elements[i].querySelectorAll(selector))
      array = array.concat(this.elements2)
    }
    array.oldApi = this // find 返回值会覆盖原本的 api，所以提取保存下旧的 api（this）
    return jQuery(array)
  },
  each(fn) {...},
  appendTo() {...},
  ...
}
```

+   [完整代码](D:\Jirengu\第1阶段\7-JS编程接口\4-jQuery中的设计模型（下）\dom-2-github-prototype\src)

	

	

### 补充： jQuery.fn

```js
jQuery.fn = jQuery.prototype = {...}
```

+   在 jQuery 的源码中，你会发现还多赋值了一个 jQuery.fn （如上）。用 fn 来表示 prototype 原型
    +   可能是嫌弃 prototype 这个单词太长了，所以想用 fn 来表示
    +   也可能是想兼容一些不太理解 prototype 的开发者，所以干脆换一个更短的单词 fn
    +   总之，就是 jQuery 的源码中，对 jQuery 的 prototype 原型取了一个别名叫： fn

	

	

### 总结

>   $  指代 jQuery

-   把共用属性（函数）全都放到  $.prototype

-   ```js
    $.fn = $.prototype  // 名字太长不爽，再起个别名 fn
    ```

-   然后让 api.\_\_proto__ 指向 $.fn （也就是让 api.\_\_proto__ 指向了 \$.prototype）

	

###  完整代码

```js
window.$ = window.jQuery = function (selectorOrArrayOrTemplate) {
  let elements
  if (typeof selectorOrArrayOrTemplate === "string") {
    if (selectorOrArrayOrTemplate[0] === "<") {
      // 创建 div
      elements = [createElement(selectorOrArrayOrTemplate)]
    } else {
      // 查找 div
      elements = document.querySelectorAll(selectorOrArrayOrTemplate)
    }
  } else if (selectorOrArrayOrTemplate instanceof Array) {
    elements = selectorOrArrayOrTemplate
  }

  function createElement(string) {
    const container = document.createElement("template")
    container.innerHTML = string.trim()
    return container.content.firstChild
  }

  // api 可以操作elements
  const api = Object.create(jQuery.prototype) // 创建一个对象，这个对象的 __proto__ 为括号里面的东西
  // const api = {__proto__: jQuery.prototype}
  Object.assign(api, {
    elements: elements,
    oldApi: selectorOrArrayOrTemplate.oldApi
  })
  // api.elements = elements
  // api.oldApi = selectorOrArrayOrTemplate.oldApi
  return api
}
```

#### 原型

```js
jQuery.fn = jQuery.prototype = {
  constructor: jQuery,
  jquery: true,
  get(index) {
    return this.elements[index]
  },
  appendTo(node) {
    if (node instanceof Element) {
      this.each(el => node.appendChild(el))
    } else if (node.jquery === true) {
      this.each(el => node.get(0).appendChild(el))
    }
  },
  append(children) {
    if (children instanceof Element) {
      this.get(0).appendChild(children)
    } else if (children instanceof HTMLCollection) {
      for (let i = 0; i < children.length; i++) {
        this.get(0).appendChild(children[i])
      }
    } else if (children.jquery === true) {
      children.each(node => this.get(0).appendChild(node))
    }
  },
  find(selector) {
    let array = []
    for (let i = 0; i < this.elements.length; i++) {
      const elements2 = Array.from(this.elements[i].querySelectorAll(selector))
      array = array.concat(this.elements2)
    }
    array.oldApi = this // this 就是 旧 api
    return jQuery(array)
  },
  each(fn) {
    for (let i = 0; i < this.elements.length; i++) {
      fn.call(null, this.elements[i], i)
    }
    return this
  },
  parent() {
    const array = []
    this.each(node => {
      if (array.indexOf(node.parentNode) === -1) {
        array.push(node.parentNode)
      }
    })
    return jQuery(array)
  },
  children() {
    const array = []
    this.each(node => {
      if (array.indexOf(node.parentNode) === -1) {
        array.push(...node.children)
      }
    })
    return jQuery(array)
  },
  print() {
    console.log(this.elements)
  },
  // 闭包：函数访问外部的变量
  addClass(className) {
    for (let i = 0; i < this.elements.length; i++) {
      const element = this.elements[i]
      element.classList.add(className)
    }
    return this
  },
  end() {
    return this.oldApi // this 就是新 api
  }
}
```





## 库封装完成

>   可以把代码公开了

-   发布到 GitHub

-   添加文档，告诉别人怎么用

-   获得称赞 ❤️ 🧡 💛 💚 💙 💜 🖤 🤍 🤎

    +   实现一个封装的库，提供给别人使用，好用的话，别人就会给你点赞

    -   jQuery 就是早期一个程序员写的库，并提供给所有开发者使用 👍👍

-   这就是程序员的社区。人人为我，我为人人

	

>   当然现在的水平，肯定不够指导别人。
>
>   就先把代码写完整，自己用成功一次即可
>
>   以后会学习「如何做单元测试」（当然「单元测试」可能比「封装jQuery」的代码还难）

>   现在需要做的就是：自己能动手写成至少 10% 的这么一个轮子
>
>   +   只要你能完成，那代码水平肯定会显著提高

​	

​	

​		

## jQuery 有多牛 X 

>   它是目前前端**最长寿**的库，2006年发布 （已经14岁了）
>
>   +    vue 、react 也才四五岁
>   +    在前端历史上，有数以万计的库，最终能够活下来并一直被使用的库，很少很少。
>   +    jQuery 是目前最长寿的一个

>   它是世界上使用**最广泛**的库，[全球80%的网站](https://trends.builtwith.com/javascript/jQuery)在用
>
>   +   可能现在新的科技公司不会再用 jQuery
>   +   但是老牌大公司，像是阿里巴巴、淘宝，一直都在用 jQuery



### 设计模式？

>   为什么 jQuery 这么牛 X   ？
>
>   +   因为 jquery 的代码设计，做的 特 \~ 别 \~ 的 好。
>   +   好到没办法改进
>   +   我们今天学习的、很多写代码的套路，都是从 jQuery 的源码中学来的（工作个四五年再去看 jQuery 源码学习，小白直接看无异于自杀）

### jQuery 用到了哪些设计模式

-   **不用 new 的构造函数**（jQuery做到了），这个模式没有专门的名字

    ```js
    const api = new JQuery('.test')   =>   const api = $('.test')  //在jQuery之前没人想到可以这样
    ```

-   \$(**支持多种参数**)，这个模式叫做**重载**

    -   可以传选择器、传数组、传 html 结构

-   **用闭包隐藏细节**，这个模式没有专门的名字

    -   闭包：在一个函数中，调用了函数外部的变量
    -   <u>用户永远无法直接操作 elements （隐藏细节），必须通过 api 中的函数才能操作到 elements</u>
    -   jQuery 每次生成 elements 后，这个 elements 可以一直存活。因为 jQuery 函数返回的 api 、api 中的函数里仍然在获取 elements 且函数返回值是 api。细想，这就导致 elements 一直在被访问，不断在函数的返回值中被调用。直到 api 消失，elements 才会消失

-   \$div.text() 即可读也可写，这个模式叫 **getter/setter**

    ```js
    getText()
    setText('newValue')  // 以前都是两个函数来实现读、写
    // 而jQuery中只用一个函数，根据参数个数，区分读写
    ```

-   \$.fn 是 $.prototype 的别名，这叫**别名**

    -   这的确是设计模式，因为在 jQuery 前并没有人这么干过

-   jQuery **针对不同环境使用不同代码**，这叫**适配器**

    -   电源适配器：你在日本，就调整成 110v，在中国，就调整成220v

    ```js
    if(){
    
    }else if(){
    
    }
    ```


​	

​	

### 设计模式到底是啥

-   老子这个代码写得太漂亮了，别人肯定也用得到（去掉 new、重载、闭包、getter/setter、别名、适配器…）
-   那就给这种写法取个名字吧，比如：适配器模式（if else）
-   设计模式，就是对通用代码取个名字而已
    -   实际上就是程序员的黑话、行话


​	

>   -   **适配器**：就是针对不同环境使用不同代码
>   -   **别名**：让一个名字等于另外一个名字、
>   -   **getter/setter**：一个函数，既可以get 、也可以 set （可读可写）
>   -   **闭包隐藏细节**：生成一个变量（elements），一个函数（addClass）去读这个变量
>   -   **重载**：一个函数支持多种形式的参数
>   -   **不用 new 的构造函数**：要知道是怎么一回事   `const api = new JQuery('.test')   =>   const api = $('.test')`

​	

​	

### 我应该学习设计模式吗？

#### 设计模式不是用来学的

-   你看了这些代码
-   但你并不知道这代码用来解决什么问题
-   看了白看

#### 设计模式是用来总结的

-   你只管去写代码

-   把你的代码尽量写好，不断重写

-   总结你的代码，把写得好的地方抽象出来

    -   看看符合哪个设计模式

        （并不是知道设计模式才这么写的，而是写完后发现，居然用到了设计模式）

    -   你就可以告诉别人你用到了这几个设计模式

    -   显得你特别高端



​		

​	

### 有人说不用学 jQuery

#### 真相

-   jQuery 这么简单、经典的库，为什么不学？
-   通过 jQuery 可以学会很多封装技巧，为什么不学？
    -   把一个变量放到函数里面、暴露出 api，api 可以操作变量，这就是 封装
-   连 jQuery 都理解不了，Vue / React 肯定学不好

#### 推荐文章

+   《[jQuery都过时了，那我还学它干嘛？](https://fangyinghang.com/why-still-jquery/)》


