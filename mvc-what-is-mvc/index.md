# 浅析 MVC


<!--more-->

​	

## MVC 是啥

>   MVC 是一种非常出名的架构模式（设计模式）。
>
>   如何设计一个程序的结构，这是一门专门的学问，叫做架构模式（architectural pattern），属于编程的方法论。

MVC，将代码分为三个模块，写成三个对象

-   M - Model（数据模型）负责操作所有数据
-   V - View（视图）负责所有 UI 界面
-   C - Controller（控制器）负责其他

MVC 没有严格的定义，每个程序员对 MVC 的理解都可能存在分歧，唯一统一的就是对 M / V / C 三个单词的认知

>   使用 MVC 模式的目的，简单来说就是希望 **通过将代码分离以提高代码的灵活性和复用性**。

### MVC 伪代码

```js
// 数据层，关于数据的操作放在这里
const m = {
  data: { // 数据初始化
    n: parseInt(localStorage.getItem('number') || 100)  
  },
  update: function (data) { /*更新数据*/ },
  delete: function (data) { /*删除数据*/ },
  get: function (data) { /*获得数据*/ }
}

// 视图层，关于视图的操作放在这里
const v = { 
  el: '挂载点（容器）',
  html: '需要插入元素内的HTML内容',
  render(data){ /*（获取数据）渲染html视图*/ }
}

// 控制层，关于事件监听的放到这里
const c = { 
  // 找到重要的元素绑定事件
  // 如果触发事件调用更改数据方法及渲染方法
  a: $('找到a'),
  b: $('找到b'),
  c: $('找到c'),
  bindEvents: function(){   // bindEvents 在 render 时执行
    a.on('click', function(){
      // 调用数据层方法更改数据
      // 调用视图层方法渲染页面
    })
    b.on('click', function(){/**/})
    b.on('click', function(){/**/})
  }
}
```



### 为什么用 MVC

>   对于「惯用简单、朴素的思想（监听事件、改变DOM元素）来写代码」的人来说，可能认为「改用 MVC 的方式去实现某个功能」会更复杂、麻烦

+   虽然朴素的代码逻辑没有什么问题，但如果代码量增大，功能相似的代码可能出现大量重复，后期维护会非常麻烦，而且还存在变量污染的可能。这样的代码复用性很低。
+   套用 MVC 架构的过程虽然麻烦、需要很多调试，但是后期维护成本低。每一部分代码都以一个对象（模块）的方式储存在一个独立的空间，负责某一项功能，我们更容易找到对应代码，并且在其内部修改不会对外部的代码造成很大影响

```js
// 找到重要的元素
const $button1 = $("#add1")
const $button2 = $("#minus1")
const $button3 = $("#mul2")
const $button4 = $("#divide2")
const $number = $("#number")
// 获得数据
const n = localStorage.getItem("n")
$number.text(n || 100)

// 监听事件，改变数据
$button1.on("click", () => {  // 加1
  let n = parseInt($number.text())
  n += 1
  localStorage.setItem("n", n)
  $number.text(n)
})
$button2.on("click", () => {  // 减1
  let n = parseInt($number.text())
  n -= 1
  localStorage.setItem("n", n)
  $number.text(n)
})
$button3.on("click", () => {  // 乘2
  let n = parseInt($number.text())
  n *= 2
  localStorage.setItem("n", n)
  $number.text(n)
})
$button4.on("click", () => {  // 除2
  let n = parseInt($number.text())
  n /= 2
  localStorage.setItem("n", n)
  $number.text(n)
})
```

​	

​	

## 模块化

>   [MDN：模块](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules)

在项目中实现「模块化」，通俗来讲，就是创建多个 js 文件，把相关功能的代码聚集到同一个 js 文件中，这样就实现了模块化。

随着应用的功能不断增加，业务逻辑越来越复杂，代码也会变得更加复杂。如果仍将所有功能代码放在一个 js 文件中，不同功能的代码散乱一团难以查找辨别，可能起变量名都会变得非常费劲，最终导致代码的可读性、复用性极差，后期难以维护。

所以，为了保证「代码能有清晰的结构」、「方便查找某个功能对应的代码区」，我们依据功能不同，将代码拆分成不同的模块（文件），使各个模块之间实现「解耦」。

+   解耦：每个模块的代码都独立存在，不需要依赖其他模块。（甚至一个模块用 Vue、一个用 React、一个用 jQuery 都没问题。只不过体积会大一点）
+   就像我们玩的积木一样，各个积木可以组合在一起形成一个形状，又可以拆分，又可以替换，因为各个积木块都是独立的，只要他们之间的接口（形状）匹配，就可以灵活地组合在一起，解耦就是为了逐渐达到这种理想的状态。

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201212163039745.png" alt="image-20201212163039745" style="zoom: 67%;" />

划分模块的一个准则是「高内聚、低耦合」

+   高内聚，是指一个软件模块是由相关性很强的代码组成，只负责一项任务，也就是常说的单一责任原则。
+   低耦合，是指模块之间的联系越少越好，接口越简单越好，实现低耦合，细线通信。
+   如果各个模块之间接口很复杂，说明功能划分有不合理之处、模块之间的耦合太高，同时也说明单个模块的内聚不高。

​	

​	

## EventBus

### 通信

>   上面我们说了模块化，既然我们把每个功能都分成不同的模块（文件），那么问题来了 —— 如果文件 C 中检查到用户的操作，需要通知文件 M 修改数据，M 修改了数据需要通知文件 V 进行页面渲染怎么办？

「eventBus」用于实现各个模块之间的通信 

+   eventBus 也是一种设计模式或者框架，主要用于组件/对象间通信的优化简化。
+   eventBus 包含很多方法，on 方法可以监听事件，trigger 方法可以触发事件，off 方法可以卸载监听
+   不管是 jQuery 还是 Vue 中都有类似于 eventBus 的存在，只不过叫法不一样，不过它们的功能是相似的，都是负责组件（模块）间的通信。

>   下面演示用 jQuery 生成的 eventBus

```js
// 伪代码
import $ from 'jquery'
const eventbus = $(window) // 返回一个包含eventbus的所有方法的对象

const model = { // 数据层
  data:{'数据':1},
  update(data){
    Object.assign(model.data,data) // 更新数据
    eventbus.trigger('更新数据') // 触发事件 
  }

}
const view = {
  el: '挂载点',
  html: '<div>{{内容}}</div>',
  init(container){
    view.el = $(container)
  },
  render(data){
    $(view.html.replace('{{n}}', n)).appendTo(view.el) // 更换新的(数据)内容，渲染进页面
  }
}
const control = {
  init(container){
    view.init(container)  // 拿到挂载点（元素容器）
    view.render(m.data.n) // 初始化页面
    autoBindEvents()
    eventbus.on('更新数据',() => { 
      // 监听数据层的 eventbus.trigger
      // 如果有被触发，说明数据有更新，从而进行渲染
      view.render(m.data.n)
    })
  },
  add() { // 改变数据
    m.update({n: m.data.n + 1})
  },
  minus() { // 改变数据
    m.update({n: m.data.n - 1})
  },
  autoBindEvents() {  // 监听改变数据的按钮
    view.el.on('click', 'app1', 'add')
  }
}
```

### EventBus 类

>   当需求更复杂的时候（多个应用功能都须用到 eventBus），我们将 eventBus 单独写成一个类 EventBus.js
>
>   让生成的实例对象继承 EventBus，这样每个实例都拥有了可以触发和监听的功能，相当灵活

>   遵循「事不过三」原则
>
>   -   同样的代码写三遍，就应该抽成一个函数
>   -   同样的属性写三遍，就应该做成【**共用属性（原型或类）**】
>   -   同样的原型写三遍，就应该用继承
>
>   代价：有的时候会造成继承层级太深，无法一下看懂代码。可以通过写文档、画类图解决

```js
// EventBus.js
import $ from "jquery"

class EventBus {
  constructor() {
    this._eventBus = $(window)
  }
  on(eventName, fn) {
    return this._eventBus.on(eventName, fn)
  }
  trigger(eventName, data) {
    return this._eventBus.trigger(eventName, data)
  }
  off(eventName, fn) {
    return this._eventBus.off(eventName, fn)
  }
}

export default EventBus
```

```js
// app.js
import EventBus from "./base/EventBus.js"
const e = new EventBus()
e.trigger('xxx')          // 触发 xxx 事件
e.on('xxx', () => {...})  // 监听 xxx 事件，执行函数
e.off('xxx')              // 删除 xxx 事件
```

#### 类的继承

>   遵循「事不过三」原则
>
>   -   同样的代码写三遍，就应该抽成一个函数
>   -   同样的属性写三遍，就应该做成共用属性（原型或类）
>   -   同样的原型写三遍，就应该用【**继承**】
>
>   代价：有的时候会造成继承层级太深，无法一下看懂代码。可以通过写文档、画类图解决

```js
// Model.js
import EventBus from "./EventBus"
class Model extends EventBus {  // extends 👈👈👈👈👈
  ...
}
```

```js
// app.js
import Model from "./base/Model.js"
const m = new Model()
console.log(m.trigger)
...
```

![image-20201212171718170](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201212171718170.png)

### Vue 中的 EventBus

>   Vue 有没有继承 EventBus  ？ 答：有

```js
// 验证
const v = new Vue({
  ...
})
console.dir(v)
// 第一层是 Vue 赋予的属性
// 第二层里有 $on（事件监听）、$emit（事件触发trigger） 、$off（取消监听）、$once ... 
// 用 $ 开头，都是 Vue 内置的方法
```

>   由上可知， Vue 也可以做 eventBus

```js
// const eventBus = $(window)
const eventBus = new Vue()  
console.log(eventBus.$on)
console.log(eventBus.$emit)
console.log(eventBus.$off)
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201210114438499.png" alt="image-20201210114438499" style="zoom: 67%;" />

​		

​	

## view = render(data)

>   这个思维，引导了 React 的诞生

-   比起操作 DOM 对象，直接 render 更简单
-   只要改变 data，就可以得到对应的 view

### 代价

>   render 粗扩的渲染肯定比 DOM 操作**浪费性能**

-   例：用户切换到 tab 1，**DOM 操作**直接找到选中的 tab，添加 class 激活即可。

    但 **render 思维**是在数据修改后，将当前元素容器全部移除，再依据新的数据重新渲染元素，肯定比之前更费性能

-   当然，render 的代价可以通过「虚拟 DOM」来弥补，让 render 只更新该更新的地方
    
    -   「虚拟 DOM」render 时，会对比第一次和第二次的区别，只有发生变化的地方才会重新 render

### 图示 ⭕️

>   对比 DOM 操作和 render 思维
>
>   -   黑字思路：数据从右边流向左边，最后再渲染回右边
>   -   绿字思路：数据永远保持在左边，最后被渲染到右边  ✔️✔️✔️
>       -   数据的流向更稳定

 <img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201205203053565.png" alt="image-20201205203053565" style="zoom: 50%;" />



### 例

#### 初步代码 💩

>   操作 DOM

```js
const c = {
  init(container) {
    v.init(container)
    c.bindEvents() 
  },
  bindEvents() {
    v.el.on("click", "#add1", () => {
      m.data.n += 1
      v.render()
    })
    v.el.on("click", "#minus1", () => {
      m.data.n -= 1
      v.render()
    })
    v.el.on("click", "#mul2", () => {
      m.data.n *= 2
      v.render()
    })
    v.el.on("click", "#divide2", () => {
      m.data.n /= 2
      v.render()
    })
  }
}
export default c
```

```js
// main.js
import x from "./app1.js"   // x 就是 c 的地址
x.init("#app1")
```

```html
<!-- index.html -->
<body>
  <div class="page">
    <section id="app1"></section>
  </div>
  <script src="main.js"></script>
</body>
```

#### 转换

>   改写为 「 view = render(data) 思维」

```js
const c = {
  init(container) {
    v.init(container)
    v.render(m.data.n) // view = render(data) 第一次渲染
    c.autoBindEvents()
  },
  // ... ,
  // ... ,
  bindEvents() {
    // 事件委托
    v.el.on("click", "#add1", () => {
      m.data.n += 1
      v.render(m.data.n) // view = render(data)
    })
    v.el.on("click", "#minus1", () => {
      m.data.n -= 1
      v.render(m.data.n) // view = render(data)
    })
    v.el.on("click", "#mul2", () => {
      m.data.n *= 2
      v.render(m.data.n) // view = render(data)
    })
    v.el.on("click", "#divide2", () => {
      m.data.n /= 2
      v.render(m.data.n) // view = render(data)
    })
  }
}
```





## 表驱动编程

表驱动编程（Table-Driven Methods）是一种编程模式。

适用场景：**消除代码中频繁的 if else 或 switch case 的逻辑结构代码**，使代码更加简化

+   事实上，任何信息都可以通过表来挑选。在简单情况下用逻辑语句是更简单的，但是一旦判断条件增多，那可能要写大量重复的判断语句，这时候我们通过**遍历**表来实现条件判断，将事半功倍。



### 例1

>   需求：写一个函数，传入年月，返回对应天数
>
>   +   [闰年](https://blog.csdn.net/xuehyunyu/article/details/73556048/)满足：（四年一润 且 百年不润） 或 （四百年再润）

常规写法

```js
function getDay(year, month) {
  let isLeapYear = year % 4 === 0 && year % 100 !== 0 || year % 400 === 0 ? 1 : 0
  if (month === 2) {
    return 28 + isLeapYear
  } else if (month===1||month===3||month===5||month===7||month===8||month===10||month===12) {
    return 31
  } else if (month === 4 || month === 6 || month === 9 || month === 11) {
    return 30
  }
}
console.log(getDay(2020, 12))  // 31
console.log(getDay(2020, 2))   // 29
console.log(getDay(2019, 2))   // 28
```

表驱动写法

```js
const monthDays = [
  [31,28,31,30,31,30,31,31,30,31,30,31],
  [31,29,31,30,31,30,31,31,30,31,30,31]
]
function getDay(year, month){
  let isLeapYear = year % 4 === 0 && year % 100 !== 0 || year % 400 === 0 ? 1 : 0
  return monthDays[isLeapYear][month-1]
}
console.log(getDay(2020, 12)) // 31
console.log(getDay(2020, 2))  // 29
console.log(getDay(2019, 2))  // 28
```

### 例2

>   监听元素绑定事件

常规写法

+   常规写法，看起来逻辑直白，。如果监听事件有10个… 100个，那么代码量将非常之大

```js
add1(){ ... }
min1(){ ... }
mul2(){ ... }
div2(){ ... }
document.querySelector('#add1').addEventListener('click', add1)
document.querySelector('#min1').addEventListener('click', min1)
document.querySelector('#mul2').addEventListener('click', mul2)
document.querySelector('#div2').addEventListener('click', div2)
document........
```

表驱动写法

```js
const controller = {
  add1(){  },
  min1(){  },
  mul2(){  },
  div2(){  },
  events: { // 表驱动编程（对象）
    "click #add1": "add1", // key 的前半为要监听的事件，后半为监听的元素，value 为要执行的方法
    "click #min1": "min1",
    "click #mul2": "mul2",
    "click #div2": "div2"
  },
  autoBindEvents() {
    for(let key in this.events){ // 遍历对象获得对应的 key 去做赋值操作
      const handler = this[this.events[key]]
      const [event, selector] = key.split(" ")  // ["click", "#min1"]
      $("容器").on(event, selector, handler) // 将提取出来的值去监听事件
    })
  }
}
```

>   「常规写法」的代码虽然更简单直白，但代码过于重复。随着数据规模的增大，如果监听事件有10个100个，那么这种写法的代码量也在加剧
>
>   「表驱动编程」让代码具有一个**稳定的复杂度**，不论数据规模多大，都能保持简单。
>
>   +   拒绝重复，保持**稳定的简单**，这才是程序员所追求的

​					

​	

## 参考文章

[前端MVC变形记](https://efe.baidu.com/blog/mvc-deformation/)：https://www.techug.com/post/mvc-deformation.html

[MVC浅析](https://juejin.cn/post/6844904030825611278#heading-3)
