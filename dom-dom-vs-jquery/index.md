# DOM vs jQuery（API篇）


对比 DOM 和 jQuery <!--more-->

​		

## JavaScript 的组成

- ECMAScript：JavaScript 的语法标准。包括变量、表达式、运算符、函数、if 语句、for 语句等。
- **DOM**：文档对象模型，操作**网页上的元素**的 API。比如让盒子移动、变色、轮播图等。
- **BOM**：浏览器对象模型，操作**浏览器部分功能**的 API。比如让浏览器自动滚动。



​	

## 事件的三要素

### 事件源、事件、事件驱动程序

比如，我用手去按开关，灯亮了。这件事情里，事件源是：开关。事件是：按开关。事件驱动程序是：灯的开和关。

再比如，网页上弹出一个广告，我点击右上角的`X`，广告就关闭了。这件事情里，事件源是：`X`。事件是：onclick。事件驱动程序是：广告关闭了。

于是我们可以总结出：谁引发的后续事件，谁就是事件源。

​	

**总结如下：**

- **事件源**：引发后续事件的 html 标签。
    - 触发事件的对象
    - 在 js 中就是 dom 对象
    - 在 jQuery 中 是包装过的 dom 对象
- **事件**：js 已经定义好了（常见事件 见下图）。
    - js 中 指的是 onclick , onmouseenter/onmouseleave , onmouseup/onmousedown，……
    - jQuery 中 就是 click , mouseenter/mouseleave , mouseup/mousedown，……
- **事件驱动程序**：对样式和 html 的操作。也就是 DOM。

常见的事件如下：

| JavaScript 事件 | jQuery 事件   | 当一下情况发生时,出现此事件    |
| :-------------- | ------------- | ------------------------------ |
| onabort         |               | 图像加载被中断                 |
| onblur          | blur( )       | 元素失去焦点                   |
| onchange        | change( )     | 用户改变域的内容               |
| onclick         | click( )      | 鼠标点击某个对象               |
| ondblclick      | dblclick( )   | 鼠标双击某个对象               |
| onerror         |               | 当加载文档或图像时发生某个错误 |
| onfocus         | focus( )      | 元素获得焦点                   |
| onkeydown       | keydown( )    | 某个键盘的键被按下             |
| onkeypress      | keypress( )   | 某个键盘的键被按下或按住       |
| onkeyup         | keyup( )      | 某个键盘的键被松开             |
| onload          | load( )       | 某个页面或图像被完成加载       |
| onmouseenter    | mouseenter( ) | 鼠标指针进入（穿过）元素       |
| onmouseleave    | mouseleave( ) | 鼠标指针离开元素               |
| onmousedown     | mousedown( )  | 某个鼠标按键被按下             |
| onmousemove     | mousemove( )  | 鼠标被移动                     |
| onmouseout      | mouseout( )   | 鼠标从某元素移开               |
| onmouseover     | mouseover( )  | 鼠标被移到某元素之上           |
| onmouseup       | mouseup( )    | 某个鼠标按键被松开             |
| onreset         |               | 重置按钮被点击                 |
| onresize        | resize( )     | 窗口或框架被调整尺寸           |
| onselect        | select( )     | 文本被选定                     |
| onsubmit        | submit( )     | 提交按钮被点击                 |
| onunload        | unload( )     | 用户退出页面                   |

​	

​	

​	

## 什么是 DOM

DOM：Document Object Model，文档对象模型。DOM 为文档提供了结构化表示，并定义了如何通过脚本来访问文档结构。目的其实就是为了能让 js 操作 html 元素而制定的一个规范。

DOM 就是由节点组成的。

​	

### 解析过程

HTML 加载完毕，渲染引擎会在内存中把 HTML 文档，生成一个 DOM 树，getElementById 是获取内中 DOM 上的元素节点。然后操作的时候修改的是该元素的**属性**。

​	

### DOM 树（一切都是节点）

DOM 的数据结构如下：

<img src="https://i.loli.net/2020/10/26/jiv6FJKgPLk4nNh.png" alt="image-20201026123934479" style="zoom:80%;" />

上图可知，**在 HTML 当中，一切都是节点**：（非常重要）

- **元素节点**：HMTL 标签。
- **文本节点**：标签中的文字（比如标签之间的空格、换行）
- **属性节点**：：标签的属性。

整个 html 文档就是一个文档节点。所有的节点都是 Object。



### DOM 可以做什么

- 找对象（元素节点）
- 设置元素的属性值
- 设置元素的样式
- 动态创建和删除元素
- 事件的触发响应：事件源、事件、事件的驱动程序

​	

​	

​	

## 什么是 jQuery

### 为什么要学 jQuery

究其原因是因为原生 js 在进行 dom 操作时代码量多而且容错性差，不够简练。那么 jQuery 就是为了解决这些问题而出现的。



### jQuery 是什么

+ jQuery 是一个快速、简洁的 JavaScript 框架，是继 Prototype 之后又一个优秀的 JavaScript 代码库（或 JavaScript 框架）。
+ jQuery 设计的宗旨是“write Less，Do More”，即倡导写更少的代码，做更多的事情。
+ jQuery 的作用：**它封装 JavaScript 常用的功能代码，提供一种简便的 JavaScript 设计模式，优化 HTML 文档操作、事件处理、动画设计和 Ajax 交互。 **
+ 目前这个阶段，主要介绍如何来使用 jQuery 操作 DOM，其实就是学习 jQuery 封装好的那些功能方法，这些方法叫做 API（Application Programming Interface 应用程序编程接口）。



### 对 JavaScript 进行封装

**jQuery 对 JavaScript 的哪些方面进行了封装 ？**

比如说 ：

+ 获取事件源

+ 类名操作

+ 样式操作

+ 内容操作

+ 通过关系查找元素，也进行了封装

+ 对 简单的动画 进行封装

+ ……

    那我们需要学习它的使用方法(语法/规则) 





---

---





## DOM 和 jQuery 的基本使用区别

+ 这里是网上找的一个，别人总结出来的两者区别，可以阅读一下
    + [jQuery与DOM的区别](https://blog.csdn.net/u012060033/article/details/90295613)
    + [jQuery对象和DOM对象的区别](https://www.cnblogs.com/EmilyGarden/p/8513643.html)



### 事件处理的区别

- **事件源**
    1. 触发事件的对象
    2. 在 js 中就是 dom 对象
    3. 在 jQuery 中 是包装过的 dom 对象
- **事件**
    1. js 中 指的是 onclick , onmouseenter/onmouseleave , onmouseup/onmousedown
    2. jQuery 中 就是 click , mouseenter/mouseleave , mouseup/mousedown
- **事件处理程序 ↓↓ **

```javascript
// 1. js中
obj.onclick = function() {
  // 事件处理
};

// jQuery中
$(obj).click(function() {
  // 事件处理
});

// 区别
// obj.onclick  dom对象调用属性
// obj.click()  jQuery对象调用方法
```



### 完整代码书写步骤

#### DOM书写格式

- （1）获取事件源：document.getElementById(“box”);
- （2）绑定事件： 事件源 box.事件 onclick = function(){ 事件驱动程序 };
- （3）书写事件驱动程序：关于 DOM 的操作。

最简单的代码举例：（点击 box1，然后弹框）

```HTML
<body>
	<div id="box1"></div>

	<script>
		// 1、获取事件源
		var div = document.getElementById("box1");
		// 2、绑定事件
		div.onclick = function() {
			// 3、书写事件驱动程序
			alert("我是弹出的内容");
		};
	</script>
</body>
```



#### jQuery书写格式

+ （0）先下载 jQuery 源文件，引入页面 （想使用 jQuery，必须先引入 jQuery 文件代码）

```html
<script src="js/jquery-3.3.1.min.js"></script>
<!-- 如果没有引入，浏览器会报错：
Uncaught ReferenceError: $ is not defined -->
```

- （1）获取事件源：$( ' .box ' ) ;
- （2）绑定事件： 事件源 box.事件 click ( *function* ( ) {  事件驱动程序 } ) ; 
- （3）书写事件驱动程序：关于 jQuery 的操作。

最简单的代码举例：（点击 box1，然后弹框）

```html
<body>
	<div id="box1"></div>
    
     <!-- 先下载jQuery源文件 -->
     <!-- 引入 -->
     <!-- 同时注意引用顺序，引入之后才能使用，否则会报错 $ is not defined -->
    <script src="js/jquery-3.3.1.min.js"></script>

	<script>
		// 1、获取事件源 2、绑定事件 3、书写事件驱动程序
		$('#box1').click(function() {
			alert("我是弹出的内容");
		})
	</script>
</body>
```





### DOM对象 和 jQuery对象

+ DOM对象，即是我们用传统的方法（javascript）获得的对象

+ jQuery对象，即是用 jQuery类库 的选择器获得的对象

    > + jQuery对象就是通过jQuery包装DOM对象后产生的对象，它是jQuery独有的。
    > + 如果一个对象是jQuery对象，那么就可以使用jQuery里的方法

```html
<script>
    var domObj = document.getElementById("id"); // domObj 是一个 DOM对象
    var $obj = $("#id"); // $obj 是一个 jQuery对象;
</script>
```



+ 对于一个dom对象，只需要用\$( )把dom对象包装起来，就可以获得一个jquery对象了，方法为$(dom对象)

```html
<script>
    
    var cr=document.getElementById("cr"); //dom对象
    var $cr = $(cr); //转换成jquery对象

    //转换后可以任意使用jquery中的方法了.
    //建议:如果获取的对象是 jquery对象，那么在声明变量时，变量名前面加上$,这样方便容易识别出哪些是jquery对象
    var $variable = jquery对象;
    var variable = dom对象; 
    
</script>
```



如何理解这句话 ↓↓ ？

>  在 jquery 当中，有两个变量 $ 和 jQuery ，他们是等价的

+ jquery获取事件源的方式 $('.box')    等价于  jQuery ('.box')
    + 在jq中 用 $( )   ==  jQuery( )  来获取数据源

+ dom获取事件源的方式  document.querySelector('.box')  
    + 在dom中，用 document 来获取事件源

```html
<script>
    var boxEl = document.querySelector('.box'); //boxEL => dom对象
	var box = $('.box'); //box => jquery对象
    // box 和 boxEl  并不等价 
    // 因为 用jquery 获取事件源 拿到的 不是 dom 对象(拿到的是进行了二次封装的dom对象),把它叫做jq对象
    
    boxEl.click(function () {
            console.log(1); //并不会打印,不会进行任何操作,因为压根没有click这个方法
        });
        // boxEl 是原生dom,并没有click 这个方法, jq对象 才有
        // 所以进行dom操作时,搞清楚 操作的是 原生dom对象 还是 jq对象 
</script>
```



### 入口函数

#### 入口函数的功能

> 入口函数的作用是 页面加载完成后，再执行 function（）{ 里面的代码 } ，有了入口函数，就可以实现将 js代码 写在页面基本结构的 标签 之前
>
>  +  如果没有入口函数，js 会被浏览器逐行解析，此时书写 js 必须注意存在顺序
>     +  此时若将 \<script> 代码写在最上面，则可能导致浏览器先执行了js的标签操作，却还没解析生成标签，就会报错

jQuery 中的入口函数 可以用来**取代** 原生 JS 中的 入口函数（写法见下）



#### 原生 JS 中的入口函数的写法

+ **window.onload = function ( )  { … }  **
+ *onload 事件比较特殊，这里单独讲一下。***当页面加载（文本和图片）完毕的时候，触发 onload 事件。**

+ 作用是 页面加载完成后，再执行 function（）{ 里面的代码 } ，有了这个函数，就可以将 js代码 写在页面基本结构的 标签 之前



#### jQuery 中的入口函数的写法

+ **$ ( document ) . ready ( function ( )  { … } ) ;**
    + 当文档对象执行完的时候，就执行 function 这个函数
    + ready 这个事件 就等价于 onload 这个事件
    + jQuery中的很多功能，实现起来的效果与原生 JS 一样，其实是 jQuery 对原生 JS 进行了封装，换成一种直接调用的方式，使得原生 JS 中原本实现书写 很复杂、麻烦的语句，变得非常简单
        + jQuery 学起来和原生 JS 相比，就是换了一种写法
        + jQuery 能实现的东西，原生 JS 都能实现
        + 原生 JS 能实现的，jQuery 不一定能实现，因为 jQuery 只是封装了原生 JS 中的常用功能代码，不是全部
+ **jQuery 中的入口函数可以直接简写成  ==>   $ ( function ( ) { … } ) ;   这是一种常见写法**
    + ***推荐***  *这种形式的写法 ↑↑*



#### js 和 jQuery 入口函数的区别

- 执行时间

window.onload 必须等到页面内包括图片的所有元素加载完毕后才能执行。

$(document).ready()是 DOM 结构绘制完毕后就执行，不必等到加载完毕。只加载了 dom 框架，对于大的图片需要时间，这个不等。

- 编写个数

window.onload 不能同时编写多个，如果有多个 window.onload 方法，只会执行一个。

$(document).ready()可以同时编写多个，并且都可以得到执行。





---

---





## 获取事件源

```html
<body>
    <p id="p1">下雨了</p>
    <div class="box">不凡</div>
    <p>你好</p>
</body>
```

### JS 获取事件源 (dom元素,dom对象)

#### 1. 通过选择器获取

DOM 节点的获取   有三种方式：

+ 方式（1）：通过 id 获取 单个标签：get Element By Id

```javascript
var pEl = document.getElementById('p1'); // 拿到的对象pEl是  dom元素，也叫dom对象
```

+ 方式（2）：通过 类名 获得 标签数组，所以有s
    + get Element**s** By Class Name
    + 数组形式(类数组) 具备长度、下标属性，但是不具备数组的方法

```javascript
var box = document.getElementsByClassName('box');
var boxEl = box[0]; // 通过下标 获取到具体的 dom 元素
var boxL= box.length; // box为数组形式的dom对象，可以通过.length属性，获取box数组长度
```

+ 方式（3）：通过 标签名 获得 标签数组，所以有s
    + get Element**s** By Tag Name

```javascript
var p = document.getElementsByTagName('p')[1]; // 拿到的是 p标签中的第2个<p>你好</p>
```



既然方式（2）、方式（3）获取的是标签数组，那么习惯性是**先遍历之后再使用**。

**特殊情况：数组中的值只有 1 个。**即便如此，这一个值也是包在数组里的。这个值的获取方式如下：

```javascript
document.getElementsByTagName("div1")[0]; //取数组中的第一个元素
document.getElementsByClassName("hehe")[0]; //取数组中的第一个元素
```





#### 2. querySelector 获取方式—H5新增

**html5 新选择器**：参数是 css 选择器参数

+ `document.querySelector("selector")` 选择选中的第一个

+ `document.querySelectorAll("selector")` 选择多个，拿的是数组（符合这个选择器的所有元素）







---





### jQ 获取事件源 ( jQuery对象 )

#### 通过选择器获取



#####  jQuery 的基本选择器

| 符号               | 说明                                    | 用法                                  |
| ------------------ | :-------------------------------------- | :------------------------------------ |
| $('#demo')         | 选择 id 为 demo 的元素                  | $('#demo').css('color','red')         |
| $('.demo')         | 选择 class 为 demo 的所有元素           | $('.demo').css('color','red')         |
| $('div')           | 选择所有 div 标签元素                   | $('div').css('color','red')           |
| $('*')             | 选择所有标签元素                        | $('*').css('color','red')             |
| $('.arr.arr-left') | 选择同时具有 arr 和 arr-left 类名的元素 | $('.arr.arr-left').css('color','red') |

看起来和 css 的选择器没什么两样!



##### jQuery 的其他选择器

- 层级选择器    

    **(书写css样式时，同样适用 ↓↓ )**

| 符号 | 说明                     | 用法                             |
| ---- | :----------------------- | :------------------------------- |
| 空格 | 后代选择器               | $('div span').css('color','red') |
| >    | 子代选择器               | $('div>span').css('color','red') |
| +    | 紧邻选择器：下一个兄弟   | $('div+p').css('color','red')    |
| ~    | 兄弟选择器：后边所有兄弟 | $('div~p').css('color','red')    |

- 属性选择器

​       **(书写css样式时，同样适用 ↓↓ )**

| 符号                        | 说明                                                        | 用法                                           |
| --------------------------- | :---------------------------------------------------------- | :--------------------------------------------- |
| $('a[href]')                | 具有 href 属性的 a 标签                                     | $('a[href]').css('color','red')                |
| $('a[href='baidu']')        | href 属性为'baidu'的 a 标签                                 | $('a[href='baidu']').css('color','red')        |
| $('a[href!='baidu']')       | href 属性不为'baidu'的 a 标签,包括不具有 href 属性的 a 标签 | $('a[href!='baidu']').css('color','red')       |
| $('a[href^='www']')         | href 属性以'www'开头的 a 标签                               | $('a[href^='www']').css('color','red')         |
| \$('a[href$='cn']')         | href 属性以'cn'结尾的 a 标签                                | $('a[href$='cn']').css('color','red')          |
| $('a[href*='i']')           | href 属性包含'i'的 a 标签                                   | $('a[href*='i']').css('color','red')           |
| $('a\[href][title='内容']') | 具有 href 属性且 title 属性为'内容'的 a 标签                | $('a\[href][title='内容']').css('color','red') |

- 基本筛选选择器

| 符号       | 说明(index 从 0 开始)        | 用法                             | 说明                                |
| ---------- | :--------------------------- | :------------------------------- | ----------------------------------- |
| :eq(index) | 匹配一个给定索引值的元素     | $('li:eq(1)').css('color','red') | 匹配 li 中 **下标为1**的元素        |
| :gt(index) | 匹配所有大于给定索引值的元素 | $('li:gt(1)').css('color','red') | 匹配 li 中 **下标>1**的所有元素     |
| :lt(index) | 匹配所有小于给定索引值的元素 | $('li:lt(2)').css('color','red') | 匹配 li 中 **下标<2**的所有元素     |
| :odd       | 匹配所有索引值为奇数的元素   | $('li:odd').css('color','red')   | 匹配 li 中 **下标为奇数**的所有元素 |
| :even      | 匹配所有索引值为偶数的元素   | $('li:odd').css('color','red')   | 匹配 li 中 **下标为偶数**的所有元素 |
| :first     | 获取匹配的第一个元素         | $('li:first').css('color','red') | 匹配 li 中 **第一个元素**           |
| :last      | 获取匹配的最后一个元素       | $('li:last').css('color','red')  | 匹配 li 中 **最后一个元素**         |

- 其他选择器

| 符号            | 说明(index 从 0 开始)                | 用法                     | 说明                                             |
| --------------- | :----------------------------------- | :----------------------- | ------------------------------------------------ |
| :empty          | 匹配所有不包含子元素或者文本的空元素 | $('li:empty')            | 匹配 li 中 **不包含子元素 或 文本** 的所有空元素 |
| :contains(text) | 匹配包含给定文本的元素               | $('li:contains('john')') | 匹配 li 中 **包含文本john** 的所有元素           |



---

---









---

---





## 通过关系获取节点

###  DOM：节点关系获取元素

DOM 的节点并不是孤立的，因此可以通过 DOM 节点之间的相对关系对它们进行访问。如下：

![img](D:\Jirengu\第1阶段\7-JS编程接口\img\dom1.96b01af9.png)

节点的访问关系，是以**属性**的方式存在的。

JS 中的**父子兄**访问关系：

![img](D:\Jirengu\第1阶段\7-JS编程接口\img\dom2.f94d591a.png)



这里我们要重点知道**parentNode**和**children**这两个属性的用法。下面分别介绍。



```html
<div class="box">
    <div class="inner-box">
        内部盒子
    </div>
</div>

<ul>
    <li class="li-item">li1</li>
    <li class="li-item">li2</li>
    <li class="li-item">li3</li>
    <li class="li-item">li4</li>
    <li class="li-item li5">li5</li>
    <li class="li-item">li6</li>
</ul>
```

#### 获取父节点 .parentNode

调用者就是节点。一个节点只有一个父节点，调用方式就是

```javascript
// 节点.parentNode;
var inner = document.getElementsByClassName('inner-box')[0];
var wBox = inner.parentNode; // 拿到inner的父节点，赋值给wBox
console.log(wBox);
```



#### 获取兄弟节点

##### **1、下一个节点**

> Sibling 的中文是**兄弟**。



（1）nextSibling：

```javascript
var li5 = document.getElementsByClassName('li5')[0];
var next = li5.nextSibling;
```

- 火狐、谷歌、IE9+版本：都指的是下一个节点（包括标签、空文档和换行节点）。
- IE678 版本：指下一个元素节点（标签）。



（2）nextElementSibling：

```javascript
var nextEl = li5.nextElementSibling;
```

- 火狐、谷歌、IE9+版本：都指的是下一个元素节点（标签）。



**总结**：为了获取下一个**元素节点**，我们可以这样做：在 IE678 中用 nextSibling，在火狐谷歌 IE9+以后用 nextElementSibling，于是，综合这两个属性，兼容写法 ↓↓ 

```javascript
// 下一个兄弟节点 = 当前节点.nextElementSibling || 当前节点.nextSibling;
```



##### **2、前一个节点 **

> previous 的中文是：前一个。

（1）previousSibling：

```javascript
var next = li5.previousSibling;
```

- 火狐、谷歌、IE9+版本：都指的是前一个节点（包括标签、空文档和换行节点）。
- IE678 版本：指前一个元素节点（标签）。



（2）previousElementSibling：

```javascript
var next = li5.previousElementSibling;
```

- 火狐、谷歌、IE9+版本：都指的是前一个元素节点（标签）。



**总结**：为了获取前一个**元素节点**，我们可以这样做：在 IE678 中用 previousSibling，在火狐谷歌 IE9+以后用 previousElementSibling，于是，综合这两个属性，兼容写法  ↓↓

```javascript
// 前一个兄弟节点 = 节点.previousElementSibling || 节点.previousSibling;
```



#### 获取单个子节点

##### **1、第一个子节点 **

（1）firstChild：

```javascript
var ulEl = document.getElementsByTagName('ul')[0];
var first = ulEl.firstChild;
```

- 火狐、谷歌、IE9+版本：都指的是第一个子节点（包括标签、空文档和换行节点）。
- IE678 版本：指第一个子元素节点（标签）。



（2）firstElementChild：

```javascript
var first = ulEl.firstElementChild;
```

- 火狐、谷歌、IE9+版本：都指的是第一个子元素节点（标签）。



**总结**：为了获取第一个**子元素节点**，我们可以这样做：在 IE678 中用 firstChild，在火狐谷歌 IE9+以后用 firstElementChild，于是，综合这两个属性，兼容写法  ↓↓

```javascript
// 第一个子元素节点 = 节点.firstElementChild || 节点.firstChild;
```



##### 2、最后一个子节点

（1）lastChild：

```javascript
var last = ulEl.lastChild;
```

- 火狐、谷歌、IE9+版本：都指的是最后一个子节点（包括标签、空文档和换行节点）。
- IE678 版本：指最后一个子元素节点（标签）。



（2）lastElementChild：

```javascript
var last = ulEl.lastElementChild;
```

- 火狐、谷歌、IE9+版本：都指的是最后一个子元素节点（标签）。



**总结**：为了获取最后一个**子元素节点**，我们可以这样做：在 IE678 中用 lastChild，在火狐谷歌 IE9+以后用 lastElementChild，于是，综合这两个属性，兼容写法  ↓↓

```javascript
// 最后一个子元素节点 = 当前节点.lastElementChild || 当前节点.lastChild;
```



#### 获取所有的子节点

（1）**childNodes**：标准属性。返回的是指定元素的**子节点**的集合（包括元素节点、所有属性、文本节点）。是 W3C 的亲儿子。

- 火狐 谷歌等高版本，会把 换行、空格、…  都看做 是子节点。

用法：

```javascript
// 子节点数组 = 父节点.childNodes; 
// 获取所有节点。

var childN = ulEl.childNodes;  // 所有子节点,包括换行符
```



（2）**children**：非标准属性。返回的是指定元素的**子元素节点**的集合。  ==【重要】==

- 它只返回 HTML 节点，甚至不返回文本节点。
- 在 IE6/7/8 中包含注释节点（在 IE678 中，注释节点不要写在里面）。

用法：（**用的最多**）

```javascript
// 子节点数组 = 父节点.children; 
// 获取所有子节点。用的最多。

var children = ulEl.children; // 所有标签节点 ; 组成伪数组
```



兼容问题：

```javascript
// 自己封装方法 实现  获取元素的  素有子元素节点
function myChildren(el) {
	var children = el.children;
	var myChildrenArr = [];
	console.log(children);
	for (var i = 0; i < children.length; i++) {
		// 如何 判断 他是 注释节点  还是 元素几点
		// nodeType = 8   注释节点
		// nodeType = 1   元素节点
		// nodeType =  3  是文本节点
		if (children[i].nodeType === 1) {
			myChildrenArr.push(children[i]);
		}
	}
	return myChildrenArr;
}
var ulElChildren = myChildren(ulEl);
```



#### nodeType 属性

这里讲一下 nodeType 属性。   ↑↑  案例见上

- **nodeType == 1 表示的是元素节点**（标签） 。记住：元素就是标签。
- nodeType == 2 表示是属性节点。
- nodeType == 3 是文本节点。
- nodeType == 8 注释节点



#### 获取除自己以外的所有兄弟节点

```javascript
function getSiblings(el) {
	var siblings = [];
	var pNode = el.parentNode;
	var children = pNode.children;
	for (var i = 0; i < children.length; i++) {
		// 兼容ie8
		if (children[i].nodeType === 1) {
			// 不是el再push
			if (children[i] !== el) {
				siblings.push(children[i]);
			}
		}
	}
	return siblings;  // siblings是一个数组，是由除了el元素以外的所有兄弟元素组成的数组
}
```

+ 这个函数实现过程，在 jQuery 中被封装成现成的方法
    + 在 JQ 中，可以直接调用  . siblings( )  方法 ，实现获取除自己以外的所有兄弟节点



---

---



### jQuery：通过关系获取 JQ 对象

```html
<ul class="nav">
    <li class="nav-item">li-1</li>
    <li class="nav-item active">li-2</li>
    <li class="nav-item">li-3</li>
    <li class="nav-item">li-4</li>
    <li class="nav-item">li-5</li>
    <li class="nav-item li6">li-6</li>
    <li class="nav-item"><p>li-7</p></li>
</ul>

<script src="js/jquery-3.4.1.min.js"></script>

<script>
    var currentLi = $('.nav-item.active'); // li-2
    //...
</script>
```



#### 获取父元素

```javascript
currentLi.parent(); // 父节点  // [0]ul.nav

currentLi.parents(); // 所有父节点  // [0]: ul.nav  [1]: body   [2]: html

currentLi.parentsUntil('html') // '<html>'之前的所有父节点  //  [0]: ul.nav  [1]: body
```

注：所有输出结果是 在控制台打印后 才可见   console.log ( ) ; 



#### 获取兄弟元素

##### 1. 后边的兄弟

```javascript
currentLi.next(); // 下一个兄弟节点  // li-3  length=1

currentLi.nextAll(); // 后面的所有兄弟节点  // li-3~7  length=5

currentLi.nextUntil('.li6'); // 后面的兄弟节点,直到('...') // li-3~5  length=3
```



##### 2. 前边的兄弟

```javascript
currentLi.prev(); // 上一个兄弟节点 

currentLi.prevAll(); // 前面的所有兄弟节点  

currentLi.prevUntil('.li6'); // 前面的兄弟节点,直到('...') 
```



#### 获取除自己以外的所有兄弟节点

```javascript
currentLi.siblings();
```



#### 获取子节点

+    . children ( )

```javascript
$('.nav').children(); // .nav里的所有子节点  // li-1~7

var children = $('.nav').children();
children.eq(4); // 拿到所有子节点中 下标为4 的那个  // li-5

$('.item:eq(4)'); // 写在括号里的 :eq(index)  是作为筛选选择器
children.eq(4); // children调用的 .eq(index)  是方法
```



#### 获取符合条件的后代节点

```html
<ul class="nav">
    <li class="nav-item">li-1</li>
    <li class="nav-item active">li-2</li>
    <li class="nav-item">li-3</li>
    <li class="nav-item">li-4</li>
    <li class="nav-item">
        <p>li-5</p>
    </li>
    <li class="nav-item li6">li-6</li>
    <li class="nav-item">
        <p>li-7</p>
    </li>
</ul>

<script>
    $('.nav').find('p'); // 获取.nav后代节点中的 所有p标签节点 // [0]:p li-5 , [1]:p li-7
</script>
```



### ==总览==

#### **jQuery 方法**

| jQuery 方法             | 说明                    |
| :---------------------- | ----------------------- |
| $( ) . eq ( index )     | 拿到对应下标的节点      |
| $( ) . find ( )         | 符合条件的后代节点      |
| $( ) . siblings ( )     | 除自己外的所有兄弟节点  |
| $( ) . children ( )     | 所有孩子节点            |
| $( ) . next ( )         | 下一个兄弟节点          |
| $( ) . nextAll ( )      | 后面的所有兄弟节点      |
| $( ) . nextUntil ( )    | 后面的兄弟节点，直到... |
| $( ) . prev ( )         | 上一个兄弟节点          |
| $( ) . prevAll ( )      | 前面的所有兄弟节点      |
| $( ) . prevUntil ( )    | 前面的兄弟节点，直到... |
| $( ) . parent ( )       | 父节点                  |
| $( ) . parents ( )      | 所有父节点              |
| $( ) . parentsUntil ( ) | 所有父节点，直到...     |

#### DOM 属性

| DOM 属性                     | 说明                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| dom . parentNode             | 父节点                                                       |
| dom . nextElementSibling     | 下一个兄弟节点（IE8以下不兼容）                              |
| dom . nextSibling            | 下一个兄弟节点（包括标签、空文档和换行节点）                 |
| dom . previousElementSibling | 上一个兄弟节点（IE8以下不兼容）                              |
| dom . previousSibling        | 上一个兄弟节点（包括标签、空文档和换行节点）                 |
| dom . firstElementChild      | 第一个子节点 （IE8以下不兼容）                               |
| dom . firstChild             | 第一个子节点（包括元素节点、所有属性、文本节点）             |
| dom . children               | 所有子节点                                                   |
| dom . childNodes             | 所有子节点（包括元素节点、所有属性、文本节点）               |
| dom . nodeType == 1/2/3/8    | 判断节点类型 == 元素节点（ 标签）/ 属性节点 / 文本节点 / 注释节点 |



---

---





## 事件绑定方式

### js绑定事件的三种方式

#### 1. 行内绑定

基本语法：

<标签 属性列表 事件=”事件的处理程序” />

例：\<input type='button' onclick='display()' />

示例代码：

![u=1055290830,2756449191&fm=173&app=25&f=JPEG](D:\Jirengu\第1阶段\7-JS编程接口\img\u=1055290830,2756449191&fm=173&app=25&f=JPEG.jpg)

以上代码就是最典型的行内绑定，虽然可以完成我们需要的功能，但是其把结构+样式+行为都绑定在同一个标签中，不利于后期维护。



#### 2. 动态绑定

基本语法：

dom对象.事件 = 事件的处理程序（通常是一个匿名函数）

通过动态绑定这种思想改进上题，效果如下图所示：

![fJPEG](D:\Jirengu\第1阶段\7-JS编程接口\img\fJPEG.jpg)





#### 3. 事件监听器 addeventlistener

> 绑定事件的第三种方式，可以更精细的控制事件 
>
> + 允许绑定多个



**① 采用一般事件获取方式，直接绑定函数时，只能绑定一次**

```javascript
var box = document.querySelector('.box');
box.onclick = function () {
    console.log(1);  // 不会打印，因为已经被后面的绑定事件操作覆盖
}
// 会覆盖,不能绑定多个
box.onclick = function () {
    console.log(2);  // 打印 2
}
```



**② 采用一般事件获取方式后**，借助 **“ 事件监听器 ” ** 绑定函数：**可多次绑定**

+ **target . addEventListener ( type , listener , useCapture ) ; **
    + target : 给谁绑定
    + type : 事件类型    不加 on 
    + listener : 要执行的函数 ( 处理程序 )
    + **useCapture** : 使用捕获还是 冒泡  ( true 代表 捕获 ) ( false 代表冒泡 )   不写默认 false

```javascript
box.addEventListener('click', function () {
    console.log(1); // 会打印 1
})
box.addEventListener('click', function () {
    console.log(2); // 同时也会打印 2
})
// 点击一次 box，会打印两行，分别打印 1 和 2
```



##### 触发机制

+   由 useCapture 拓展 “ 触发机制 ” 
    + 触发机制：先捕获后冒泡 
    + 每个事件绑定时，可以指定绑在哪个阶段  (  捕获阶段 和 冒泡阶段  )
    + 捕获：就是 从外往里 触发 
    + 冒泡：从里到外 触发

```javascript
d1.addEventListener('click',function(){
    alert('d1);
},true)

d2.addEventListener('click',foo,false)

function foo(){
    alert('d2');
}
```

+ 实例 ↓↓ 

![image-20191224220712109](https://i.loli.net/2020/10/26/nezGvUriVqANjx7.png)

```javascript
//当点击w3时，弹出顺序是 w1 w3 w2

w1.addEventListener('click', function () {  
    alert('w1');
}, true)//true,则事件在从外向里的捕获阶段,就会触发

w2.addEventListener('click', function () {
    alert('w2');
}, false)//false,则事件在从外向里的捕获阶段,不会触发;捕获阶段走完,开始走冒泡阶段时,才会触发

w3.addEventListener('click', foo, true)  //true,则事件在从外向里的捕获阶段,就会触发

function foo() {
    alert('w3');
}

//所以当点击了w3时,事件开始从捕获阶段进入,先依次触发了w1,w3,进入冒泡阶段后,触发了w2
```

![capture.jpg](D:\Jirengu\第1阶段\7-JS编程接口\img\cQAd2iBJTRU9b4q-1577196471778.jpg)





##### removeEventListener 移除绑定

- 如果同一个监听事件分别为“事件捕获”和“事件冒泡”注册了一次，一共两次，这两次事件需要分别移除。两者不会互相干扰。
- 移除的事件必须为外部事件（外部封装的函数）。
- 总结来讲，就是移除时，必须和绑定时一一对应。

```javascript
d2.addEventListener("click", foo, false);
d2.removeEventListener("click", foo, false);
function foo() {
	alert("d2");
}
```



#####  IE8 以下兼容问题

- target.attachEvent(type, listener);
- target.detachEvent(type,listener);

```javascript
/**
 * 兼容IE8和标准浏览器
 * el 绑定元素
 * type 事件类型,IE8要加on
 * func 执行方法
 **/
function myAddEventListener(el, type, func) {
	// attachEvent 是IE 专有的方法
	if (el.attachEvent) {
		el.attachEvent("on" + type, func);
	} else {
		el.addEventListener(type, func);
	}
}
```





---

---





## 判断元素节点类型

![img](D:\Jirengu\第1阶段\7-JS编程接口\img\011527440322088.png)

| 节点类型 | 说明                                                    | 值   |
| -------- | ------------------------------------------------------- | ---- |
| 元素节点 | 每一个HTML标签都是一个元素节点，如 <div> 、 <p>、<ul>等 | 1    |
| 属性节点 | 元素节点（HTML标签）的属性，如 id 、class 、name 等。   | 2    |
| 文本节点 | 元素节点或属性节点中的文本内容。                        | 3    |
| 注释节点 | 表示文档注释，形式为<!-- comment text -->。             | 8    |
| 文档节点 | 表示整个文档（DOM 树的根节点，即 document ）            | 9    |



### 1. target.nodeName

```html
<!--使用javascript判断节点名称-->
<div id="oneDiv">一段文本</div><!--注释文本-->

<script type="text/javascript">
  var div = document.getElementById("oneDiv");
  console.log(div.nodeName); //输出DIV，元素节点为标签大写
  
  var divText = div.firstChild;
  console.log(divText.nodeName) //输出#text，文本节点使用nodeName时永远为#text
  
  var divAttr = div.getAttributeNode("id");
  console.log(divAttr.nodeName) //输出id，属性节点为属性名
  
  var comment = div.nextSibling;
  console.log(comment.nodeName) //输出#comment，注释节点使用nodeName时永远为#comment
</script>
```



### 2. target.tagName

要访问元素的标签名，可以使用tagName或nodeName属性；这两个属性返回的值相同

假设HTML如下：

```html
<div id="a"></div>
```

获取标签名：

```js
var ele = document.getElementById('a');

console.log(a.tagName)      //DIV
console.log(a.nodeName)     //DIV
console.log(a.nodeName == a.tagName)        //true
```

**在HTML中，标签名会以全部大写表示;而在XML中（部分XHTML）中，标签名则始终保持与源代码一致。**

基于上面的特性，在判断标签名的时候，需要做些兼容处理：

```js
var ele = document.getElementById('a');

if( ele.tagName.toLocaleLowerCase() == 'div' ){
  console.log( 'div success' );
}
```



有关它们的区别，这位老师总结的很详细，https://blog.csdn.net/borishuai/article/details/571922





### 3. target.nodeType

```html
<!--使用javascript判断节点类型-->
<div id="oneDiv">一段文本</div><!--注释文本-->

<script type="text/javascript">
  var div = document.getElementById("oneDiv");
  console.log(div.nodeType); //输出1，元素节点
  
  var divText = div.firstChild;
  console.log(divText.nodeType) //输出3，文本节点
  
  var divAttr = div.getAttributeNode("id");
  console.log(divAttr.nodeType) //输出2，属性节点
  
  var comment = div.nextSibling;
  console.log(comment.nodeType) //输出8，注释节点
</script>
```







### 4. target.nodeValue

```html
<!--使用javascript判断节点值-->
<div id="oneDiv">一段文本</div><!--注释文本-->

<script type="text/javascript">
  var div = document.getElementById("oneDiv");
  console.log(div.nodeValue); //输出null，元素节点对于nodeValue不支持
  
  var divText = div.firstChild;
  console.log(divText.nodeValue) //输出一段文本，文本节点输出文本值
  
  var divAttr = div.getAttributeNode("id");
  console.log(divAttr.nodeValue) //输出oneDiv，属性节点输出属性值
  
  var comment = div.nextSibling;
  console.log(comment.nodeValue) //输出注释文本，注释节点输出注释内容
</script>
```





## 理解Node类型

> 理解Node类型——不应被忽视的 nodeType、nodeName、nodeValue
>
> [原地址](https://blog.csdn.net/zwkkkk1/article/details/80229923 )

### Node 类型

DOM1级定义了 Node 接口，该接口将由 DOM 中的所有节点类型实现。这个 Node 接口在 JavaScript 中是作为 Node 类型实现的；除了 IE 之外，在其他所有浏览器中都可以访问到这个类型。JavaScript 中的所有节点类型都继承自 Node 类型，因此所有节点类型都共享着相同的基本属性和方法。

这篇讲讲 Node 类型常会被忽视的三个属性：nodeType、nodeName、nodeValue。

### nodeType 属性

每个节点都有一个 nodeType 属性，用于表明节点的类型，节点类型由 Node 类型中定义12个常量表示：

![image-20200526150026534](https://i.loli.net/2020/10/26/r1Nwu9IZnyfQHsg.png)

**注：红色加粗为常用的节点类型**

来看下面这个例子，看如何去 `Node` 类型节点，以及它 `NodeType` 属性。

> ps: 可以直接复制出去运行

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="box">hello nodeType~</div>

<script type="text/javascript">
	var element = document.getElementById("box");
	var attr = document.getElementById("box").getAttributeNode("id");
	var text = document.getElementById("box").firstChild;

	console.log(element.nodeType);     //1
	console.log(attr.nodeType);        //2
	console.log(text.nodeType);        //3
</script>
</body>
</html>
```



### nodeName 属性与 nodeValue 属性

要了解节点的具体信息，可以使用 nodeName 和 nodeValue 这两个属性。这两个属性的值完全取决于节点的类型。

一般来说：

- 元素节点的 `nodeName` 是标签名称（大写）
- 属性节点的 `nodeName` 是属性名称
- 文本节点的 `nodeName` 永远是 `#text`
- 文档节点的 `nodeName` 永远是 `#document`

这里可以改下上面的例子，打印这样的：

```js
console.log(document.nodeName)    //#document
console.log(element.nodeName);    //DIV
console.log(attr.nodeName);       //id
console.log(text.nodeName);       //#text
```

- 对于文本节点，`nodeValue` 属性包含文本。
- 对于属性节点，`nodeValue` 属性包含属性值。
- 文档节点和元素节点，`nodeValue` 属性的值始终为 `null`。

```js
console.log(document.nodeValue)    //null
console.log(element.nodeValue);    //null
console.log(attr.nodeValue);       //box
console.log(text.nodeValue);       //hello nodeType~
```





---

---





## 节点的增删改查==（重要）==

### DOM 节点的增删改查

上一段的内容：节点的**访问关系**都是**属性**。

本段的内容：节点的**操作**都是**函数**（方法）。



#### 创建节点

格式如下：

```javascript
	新的标签(元素节点) = document.createElement("标签名");
```

比如，如果我们想创建一个 li 标签，或者是创建一个不存在的 adbc 标签，可以这样做：

```html
<script type="text/javascript">
	var a1 = document.createElement("li"); //创建一个li标签
	var a2 = document.createElement("adbc"); //创建一个不存在的标签

	console.log(a1); // <li></li>
	console.log(a2); // <adbc></adbc>

	console.log(typeof a1); // object
	console.log(typeof a2); // object
</script>
```





####  插入节点

+ 往页面中添加节点，依赖于父节点

+ 插入节点有**两种方式**，它们的含义是不同的。
+ 多次插入同一个节点的时候 ,相当于剪切效果
+ 注意：如果是往 body 里插入，直接 `var body = document.body  ` 即可获取到 body 节点，不需要 getElements
+ 将同一个元素，前后设置插入到不同的地方，并不会出现在两个地方，而是后次设置对前次设置进行了剪切

举例 ↓↓

```html
<div class="box">
    <div class="innerbox">
        <p class="txt">天气很好</p>
    </div>
</div>
```

方式 1：

```javascript
父节点.appendChild(新的子节点);
```

解释：父节点的最后插入一个新的子节点。

```javascript
// 创建节点
var el = document.createElement('p');  // <p></p>
// 插入节点
// 将el这个节点 作为innerbox的子节点 插入进去, 从后添加
var innerbox = document.getElementsByClassName('innerbox')[0];
innerbox.appendChild(el);
```

```html
<div class="box">
    <div class="innerbox">
        <p class="txt">天气很好</p>
        <p></p>   <!-- 执行结果是: 这里插入了一个p标签 -->
    </div>
</div>
```



方式 2：

```javascript
父节点.insertBefore(新的子节点, 作为参考的子节点);
```

解释：

- 在参考节点前插入一个新的节点。
- 如果参考节点为 null，那么他将在父节点里面的最后插入一个子节点。

```javascript
// 创建一个div节点
var el1 = document.createElement('div'); // <div></div>
// 获取类名为txt的节点
var pEl = document.getElementsByClassName('txt')[0];
// 插入节点
// box.appendChild(el1);    //方式1: el1作为子节点插入到box内部的最后面
box.insertBefore(el1, pEl); //方式2: el1作为子节点插入到box内部的pEl节点之前


// 如果参考节点为null，那么他将在节点最后插入一个节点。
box.insertBefore(pEl, imgEl);
```

```html
<div class="box">
    <div class="innerbox">
        <div></div>   <!-- 执行结果是: 这里插入了一个div标签 -->
        <p class="txt">天气很好</p>
    </div>
</div>
```



#### 删除节点

格式如下：

```javascript
父节点.removeChild(子节点);
```

解释：**用父节点删除子节点**。必须要指定是删除哪个子节点。

```javascript
box.removeChild(pEl); // 删除 box节点中 的子节点 pEL

//如果想删除自己这个节点：自己(先调用属性获取到父节点)删除自己
pEl.parentNode.removeChild(pEl); // pEl调用parentNode属性拿到父节点，然后从父节点中删除pEl

// 本质上，删除节点操作 仍然是依赖父节点的
```



#### 复制节点（克隆节点）

格式如下：

```javascript
要复制的节点.cloneNode(); // 括号里不带参数和带参数false，效果是一样的：只复制节点本身

要复制的节点.cloneNode(true); // 括号里带参数true，是深复制：复制节点本身及其所有子节点
```

括号里带不带参数，效果是不同的。解释如下：

- **不带参数/带参数 false**：只复制节点本身，不复制子节点。
- **带参数 true**：既复制节点本身，也复制其所有的子节点。——**深复制**



```javascript
var temp = box.cloneNode(true);
var tempF = box.cloneNode(false);

console.log(temp);
console.log(tempF);
```

```HTML
<!-- 控制台输出结果 -->

<!-- temp 深复制 -->   
<div class="box">
    <div class="innerbox">
        <p class="txt">天气很好</p>
    </div>
</div>

<!-- tempF -->
<div class="box">
</div>
```



---



### jQuery 节点的增删改查

```html
<div class="box"></div>
<ul>
    <li>1</li>
    <li>2</li>
    <li class="active">3</li>
    <li>4</li>
</ul>
```



#### 创建节点

格式如下：

```javascript
var 变量名 =  $('闭合标签');   // 注意是写在'引号'里面，字符串形式
```

+ 创建过程，会自动把  “ 字符串 ” 里的 标签，包装成一个节点
+ 注意是 “ 字符串 ”，且是一个闭合标签
+ 创建时，除了写标签外，也可以选择书写内容和属性；也可以后面用 attr ( ) 设置属性

```javascript
var jqNode1 = $('<img src="img/m1.jpg" alt="">');
var jqNode2 = $('<p>不凡</p>'); 
console.log(jqNode1); // 打印出 img标签 的节点
console.log(jqNode2); // 打印出 p标签 的节点
```

![image-20191223214355839](https://i.loli.net/2020/10/26/niRxvEG4LhyoPcB.png)

如图打印结果，就是将  `$('<img src="img/m1.jpg" alt="">');`   包装成一个 img 节点（jQuery对象）

+ 打印结果为 `k.fn.init [ ]`  就说明是  jQuery 对象



#### 插入节点

```html
<div class="box"></div>
<div class="box1"></div>
<div class="box2"></div>
```



- append()
    - 参数 `jq对象` 或 `标签字符串` 或 `DOM对象` ，也就是可以传 jq对象，标签字符串，原生dom
    - 作用：在被选元素内部从前面追加内容或节点。
    - 如果是页面中存在的元素，那么调用 append ( ) 后，会把这个元素放到相应的目标元素里面去；但是，原来的这个元素，就不存在了。**(剪切)** **对一个元素多次进行追加到其他位置的操作，就是剪切 **

```javascript
$('.box').append($p);  //参数是jq对象
$('.box1').append('<p>不凡</p>');//参数是标签字符串

var pEl = createElement('p');//原生js方法，创建了一个p标签 => pEl是一个dom对象
$('.box2').append(pEl);// 参数是原生dom
```



+ 如果是给多个目标追加元素，那么方法的内部会复制多份这个元素，然后追加到多个目标里面去。

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
<script>
    $('li').append(pEl);  // $('li')对象，代表了多个元素，此时会往每个li里都插入pEl，
    //而pEl原来的位置是被创建插入到.box2中，经过此次append后，.box2中已经没有pEl了，但每个li中都会有pEl
</script>
```



- appendTo ( )

作用：把`$(selector)`追加到`node`中去

+ append ( ) 作用是：在被选元素内部，从前面追加内容或节点。
+ 二者是，把字句 和 被字句 的关系

```javascript
$(selector).appendTo(node);

$('.box2').append(pEl); // 在box2里追加 pel  
$(pEl).appendTo($('.box2')); // 把pel追加到box2里
//这两句代码是等价的
//通常使用 append（）即可
```



- prepend()

    - 参数 `jq对象` 或 `标签字符串` 或 `DOM对象`

    - 作用：在被选元素内部从前面追加内容或节点。
    - 注意问题与 append ( ) 基本一致



- after()

作用：在被选元素之后，作为兄弟元素插入内容或节点

```text
$(selector).after(node);
```



- before()

作用：在被选元素之前，作为兄弟元素插入内容或节点

```text
$(selector).before(node);
```



> 对于这些添加方法，node 可以是`jq对象` 或 `标签字符串` 或 `DOM对象`。





#### 清空元素

共有3个方式：

+ 不需要再借助父节点

```javascript
$(selector).empty(); // 清空selector里面的内容，selector依然存在
$(selector).html('...'); // 清空selector里面的内容，但是会把“引号”里的内容追加进去
$(selector).remove();  //清空selector里面的内容，同时会把selector自己也删除
```

 . html（ "   " ）：这个原本是做内容操作，用来追加内容，

+ 但是它本身机制，是讲被选元素里的内容，全部替换为 引号里的，如果引号不填内容，实际上就是变相的实现了 清空元素的作用；
    + 引号里填了内容，就是实现完全替换的作用
+ .text（）：同理，也可以用来清空元素； 区别是text()不能识别标签，如果有替换元素，则仅以文本形式存在



#### 复制元素

格式如下：

```javascript
$(selector).clone();
```

- 参数，与dom相同：要复制的节点 . cloneNode ( ) 
- **不带参数/带参数 false**：只复制节点本身，不复制子节点。
- **带参数 true**：既复制节点本身，也复制其所有的子节点。——**深复制**

```javascript
$('.box').clone();

var clone = $('.box').clone();
console.log(clone);
```





---

---





## 样式操作

### DOM 的样式修改

#### dom对象 . style . 属性名 =  '  属性值 '

```html
<div class="box" style="color:orange;">不凡</div>

<script>
    var box = document.getElementsByClassName('box')[0];
    box.onclick = function () {
        box.style.color = 'blue';
        box.style.width = '300px';
        // font-size  写成 fontSize  (‘-’拼接字符命名的属性名 改为 驼峰命名)
        box.style.fontSize = '24px';
        // 这些属性，修改的都是 标签的行内样式
    }
</script>
```



#### date-* — H5新增

+ **自定义属性 data-* **
    + 功能强大：可以修改样式，但这只是data-*的基础操作，有待深挖研究

```html
<ul class="nav">
    <li class="nav-item active" data-color="red" data-info="你好">li1</li>
    <li class="nav-item" data-color="blue">li2</li>
    <li class="nav-item" data-color="orange">li3</li>
    <li class="nav-item" data-color="pink">li4</li>
    <li class="nav-item" data-color="green">li5</li>
    <li class="nav-item" data-color="gray">li6</li>
</ul>
<script>
    // 自定义属性 data-* 
    // 如何获取自定义属性值  Node.dataset['color'] 我们便可以获取到自定义的属性值。
    
    console.log(liArr[0].dataset);  //  {color: "red", info: "你好"}
    console.log(liArr[0].dataset['color']); //  red

    for (var i = 0; i < liArr.length; i++) {
        var temp = liArr[i].dataset['color']; //获取每一个li的data-color值，存在temp里
        liArr[i].style.backgroundColor = temp; // 然后赋给 每个li的背景颜色样式中
    }
</script>
```

![image-20191222211253685](https://i.loli.net/2020/10/26/hdNLArxSsCUMpIZ.png)



---



### jQuery 的样式修改 . css( )

+ $ ( ' 选择器 ' ) . css ( ' 属性名 ' ,   ' 属性值 ' ) 

```html
<div class="box" style="color:orange;">不凡</div>

<script>
    $('.box').click(function () {
        // $('.box').style.color = 'red'; 错误
        // 获取样式
        var tempColor = $('.box').css('color');
        console.log(tempColor); // 没有设置字体颜色时,默认是rgb(0, 0, 0) 黑色

        // 修改样式
        $('.box').css('color', 'red');

        // 设置多个样式: css里写一个对象,对象中写入,多个样式信息
        $('.box').css({
            color: 'blue',
            width: '200px',
            height: '200px',
            backgroundColor: 'red'
        })
    })
</script>
```

>  . css ( ) 写法 和 . attr ( ) 写法一致，书写方式一致：都可以设置修改 单个 或 多个 





---

---





## 属性操作

### DOM 的属性操作

```html
<div class="box" title="你好">不凡</div>
<img src="" alt="">

<script>
    var img = document.getElementsByTagName('img')[0];
    var box = document.getElementsByClassName('box')[0];
    //...
</script>
```

#### 设置节点属性

+ 方式（1）：dom对象 . 属性名 = " 属性值 " 

```javascript
img.src = 'img/m1.jpg';
box.title = '天气';
box.className = 'd1';
```

+ 方式（2）： setAttribute ( 属性名 , 属性值 )

```javascript
box.setAttribute('class', 'box1');  // 类名修改为box1
box.setAttribute('title', '你好吧');  // 设置title为“你好吧”
img.setAttribute('src', 'img/m2.jpg'); // 设置<img>的图片路径
```

#### 获取节点属性

+ 方式（1）：dom对象 . 属性名

+ 方式（2）：getAttribute( 属性名 )

```javascript
console.log(box.title); // 你好吧
console.log(box.getAttribute('title'));  // 你好吧 
```

#### 删除属性

+ removeAttribute ( " 属性名 " )

```javascript
box.removeAttribute('title');
```



---



### jQuery 的属性操作  . attr ( )

```html
<img src="images/05.jpg" alt="">

<script src="js/jquery-3.4.1.min.js"></script>

<script>
//...
</script>
```



#### 设置节点属性

+ 方式（1）：$ ( ' 选择器 ' ) . attr ( ' 属性名 ' , ' 属性值 ' )  设置修改单个属性  

```javascript
$('img').attr('src', 'images/06.jpg'); // 设置图片路径
```

> $ ( ' 选择器 ' ) . prop ( ) ：常用来 设置 修改 属性值为（ true / false ）的属性                                                     [浅谈 jQuery 中 prop( ) 和 attr( )](https://blog.csdn.net/WuLex/article/details/85130327)  



+ 方式（2）：$ ( ' 选择器 ' ) . attr ( {  属性名  :  ' 属性值 ' ，属性名  :  ' 属性值 ' } )   设置修改多个属性 ( 对象 )

```javascript
$('img').attr({
    src: 'images/02.jpg',    //注意：对象形式中，每个属性间隔用“,”逗号
    alt: 'sorry'
})
```

> . attr ( ) 写法和 . css ( ) 写法一致：都可以设置修改 单个 或 多个



#### 获取节点属性

```javascript
var src = $('img').attr('src');
console.log(src);  // images/05.jpg
```



#### 删除属性

+ $ ( ' 选择器 ' ) . removeAttr ( " 属性名 " )

```javascript
 $('img').removeAttr('alt')
```





---

---





## 类名操作

### DOM 的类名操作_—_H5新增

```html
<ul>
    <li class="li-item">li1</li>
    <li class="li-item">li2</li>
</ul>

<script>
    var lis = document.getElementsByClassName('li-item');
    //...
</script>
```

+ **追加类名：dom. classList . add ( ' 类名 ' )  **

```javascript
lis[0].classList.add('danger');  // <li class="li-item danger">li1</li>
```



+ **删除类名：dom. classList . remove ( ' 类名 ' )  **

```javascript
lis[0].classList.remove('danger');  // <li class="li-item">li1</li>
```



+ **有则删除，无则添加：dom. classList . toggle ( ' 类名 ' )  **

```javascript
lis[0].classList.toggle('strong'); // 切换类名 <li class="li-item danger">li1</li>
```



+ **判断是否有该类名：dom. classList . contains ( ' 类名 ' )  **

```javascript
console.log(lis[1].classList.contains('danger'));// 返回 true/false
												 // false
```

> . contains 检测是否存在 class 非常好用，但是出现的太晚了 。。。T o T



---



### jQuery 的类名操作

```html
<p class="txt">不凡</p>

<script src="js/jquery-3.4.1.min.js"></script>

<script>
//...
</script>
```

+ **追加类名**：$ ( ' 选择器 ' ) . addClass ( ' 类名 ' )    

```javascript
$('p').addClass('danger');  // 向被选元素添加一个或多个类  
                            // <p class="txt danger">不凡</p>
```



+ **删除类名**：$ ( ' 选择器 ' ) . removeClass ( ' 类名 ' )    

```javascript
 $('p').removeClass('danger'); // 从被选元素删除一个或多个类
							   // <p>不凡</p>
```



+ **有则删除，无则添加**：$ ( ' 选择器 ' ) . toggleClass ( ' 类名 ' )   

```javascript
$('p').toggleClass('danger');  // 对被选元素进行添加/删除类的切换操作
                               // <p class="danger">不凡</p>
```



+ **判断是否有该类名**：$ ( ' 选择器 ' ) . hasClass ( ' 类名 ' )    

```javascript
console.log($('p').hasClass('danger'));  // 判断被选元素是否存在类，返回值true/false
										 // true
```





---

---





## 内容操作

### DOM 的内容操作：调用 . 属性

> 调用属性，直接等号赋值 （  dom . 属性名 = ’   ‘  ） 

> DOM 对象的属性和 HTML 的标签属性几乎是一致的。例如：src、title、className、href 等。



#### innerHTML 和 innerText

+ ↑↑  这俩属性实现的是 对元素内部的内容的完全替换

    + 即会先删除元素内部的所有内容，再将 等号= 后的值添加到元素中

        + 也就是说如果等号=后为空，则实现了清空元素内部所有内容的效果

        + ```js
            box.innerHTML = ''; // 将box元素里的内容替换为‘’,即清空box元素里的内容
            ```

            

**dom对象 . innerHTML  =  "  ….  "  ;    **

+ 双闭合标签里面的内容（识别标签）。**可以用来动态的生成页面**

```html
<div class="box">不凡</div>
<ul class="box"> </ul>

<script>
    var box = document.getElementsByClassName('box')[0];
    var box1 = document.getElementsByClassName('box')[1];
    //...
</script>
```

```javascript
box.innerHTML = '<h1>今天天气很好</h1>';  // 识别标签
									// <div class="box"><h1>今天天气很好</h1></div>
```

```javascript
// **动态生成**的标准写法：采用for循环>累加的思想
var str = '';
for (var i = 0; i <= 100; i++) {
    str += '<li>li' + i + '</li>';
}
box1.innerHTML = str;
```



**dom对象 . innerText  =  "  ….  "  ;  **

+ 双闭合标签里面的内容（不识别标签）。 ( 老版本火狐用 textContent )

```javascript
box.innerText = '<h1>今天天气很好</h1>'; //（不识别标签）
									   // <div class="box">今天天气很好</div>
```



#### 表单的内容操作

```html
<!-- 输入框 -->
<input type="text" value="请输入内容">

<!-- 单选框 -->
男:<input type="radio" class="sex" name="sex">
女:<input type="radio" class="sex" name="sex">
<button>选择男生</button>

<!-- 复选框 -->
篮球：<input type="checkbox" class="cb" name="like">
足球：<input type="checkbox" class="cb" name="like">

<!-- 下拉框 -->
<select>
    <option value="1" class="s1" >选项内容1</option>
    <option value="2" class="s2" >选项内容2</option>
</select>

<script>
    //...  ↓↓
</script>
```



**dom对象 . value = " … "   **      直接调用 输入框 的 value 属性    .value = ’  ‘

```javascript
var input = document.getElementsByTagName('input')[0];
input.onfocus = function () {  // .onfocus ：获得焦点
    input.value = '';  // 输入框value值，设为空
}
```



**dom对象 . checked = " true / false "  **       单选框 和 复选框 的 checked 属性。

```javascript
var btn = document.getElementsByTagName('button')[0];
var men = document.getElementsByClassName('sex')[0];
var cb1 = document.getElementsByClassName('like')[0];
btn.onclick = function () { //点击 按钮
    men.checked = true;		//men的input的checked属性，设为true，即-被选中
}
cb1.checked = true; // 复选框 默认选中 篮球
```



**dom对象 . selected = " true / false " **        下拉菜单 的 selected 属性。

```javascript
var select2 = document.getElementsByTagName('s2')[0];
select2.selected = 'true';  // 下拉菜单 默认显示 .s2 的选项内容
```



---



### jQuery 的内容操作：调用方法 ( )

#### html()、text()、value()、prop()

- **可以取值、设值**

| 方法      | 说明                                    |      | 实例                                           |
| --------- | --------------------------------------- | ---- | ---------------------------------------------- |
| html ( )  | \$ ( ' p ' ) . html ( )                 |      | $ ( " p " ) . html ( ' html 代码 ' )           |
| text ( )  | \$ ( ' p ' ) . text ( )                 |      | $ ( ' p ' ) . text ( ' 内容 ' )                |
| value ( ) | \$ ( ' input ' ) . value ( )            |      | $ ( ' input ' ) . value ( ' 姓名 ' )           |
| prop ( )  | \$ ( ' input ' ) . prop ( ' checked ' ) |      | $ ( ' input ' ) . prop ( ' checked ' , false ) |

+ html()、text() 同时也具有清空的功能

    + 因为它俩实现效果是：将调用元素内部的所有子元素都替换为html方法的（）中的内容，也就是说，如果（）里没有内容，则实现的是清空的功能

    + ```js
        $('div').html();  // 将div内的所有子元素替换为()中的内容，也就是替换为空，即清空div子元素
        ```


    + ```js
        $('div').html();  // 将div内的所有子元素替换为()中的内容，也就是替换为空，即清空div子元素
        ```


​        





#### 延伸：prop() 和 attr() 的区别

prop ( ) ：常用来设置、修改属性值为 true / false 的属性： checked属性、selected属性   ……

attr ( ) ：可以用来设置修改所有属性的值

>  两者都可以修改属性，那有什么区别 ？？   [浅谈 jQuery 中  prop ( )  和  attr ( ) ](https://blog.csdn.net/WuLex/article/details/85130327)   





---

---









# DOM 补充

## arguments 对象

+ arguments 对象：函数的实参对象，只能在函数内部使用
+ console.log(arguments)   =>   输出 类数组：保存了传进来的 实参信息（如图 ↓ ）

![image-20191222213329109](https://i.loli.net/2020/10/26/NlxEjebQtGa6Ryp.png)

```html
<script>
    // arguments 对象 :  函数的实参对象,只能在函数内部使用
    function add(x, y, z) {
        // console.log(arguments) => 输出形式：类数组,保存了传进来的 实参信息
        console.log(arguments); //[1, 3, 5, callee: ƒ, Symbol(Symbol.iterator): ƒ]
        console.log(arguments[1]); //3
        console.log(arguments.callee); // 打印出了 这个函数本身 //多数情况下没什么用,but递归
        return x + y + z;
    }
    add(1, 3, 5);

    // 既然 arguments.callee 是函数本身，符合递归思想：可以实现自己调用自己
    // 可以用来写递归函数 ↓↓
    function foo(n) {
        if (n === 1) {
            return 1
        }
        // return foo(n - 1) * n;
        return arguments.callee(n - 1) * n;
    }
</script>
```



## 定时器 

### 延迟定时器 setTimeout ( )

```javascript
setTimeout(function(){console.log('你好');}, 1000) // 触发后 延迟1秒，函数开始执行，执行1次
```

+ 作用：可以让写在其内部的 function 函数，延迟执行
    + 函数作为参数，称为回调函数

+ 第一个参数：要执行的函数（时间到了就执行 ）

+ 第二个参数：设置参数延迟时间（ 以毫秒为单位 ）

+ 执行次数：只会执行一次



### 循环定时器 setInterval ( )

```javascript
setInterval(function(){console.log('不凡')}, 1000) // function 每隔1秒，执行1次
```

+ 作用：可以让写在其内部的 function 函数，每隔一段时间，就执行一次
    + 函数作为参数，称为回调函数

+ 第一个参数：要执行的函数（时间到了就执行 ）

+ 第二个参数：设置参数延迟时间（ 以毫秒为单位 ）

+ 执行次数：循环执行



### 清除定时器 clearTimeout ( )

```javascript
var timer1 = setTimeout(function () {console.log('你好');} ,1000);
var timer2 = setInterval(function () {console.log('不凡');} ,1000);

clearTimeout(timer1);
clearTimeout(timer2);
```

+ 作用：顾名思义，可以用于清除以上两种定时器
+ 需要给要清除的定时器起名字，才能使用





## 立即执行函数

现有匿名函数如下：

```javascript
function(a, b) {
    console.log("a = " + a);
    console.log("b = " + b);
};
```

立即执行函数如下：

```javascript
(function(a, b) {
    console.log("a = " + a);
    console.log("b = " + b);
})(123, 456);
```

立即执行函数：函数定义完，立即被调用，这种函数叫做立即执行函数。

立即执行函数往往只会执行一次。为什么呢？因为没有变量保存它，执行完了之后，就找不到它了。



+ 立即执行函数的应用
    + **循环绑定事件时，解决如何获取当前点击元素的下标。**

```javascript
for (var i = 0; i < lis.length; i++) {
	lis[i].onclick = (function(n) {
		return function() {
			console.log(n);
		};
	})(i);
}
```







---

---







# jQuery 补充



## mouseover 和 mouseout 区别

![image-20191222215305504](https://i.loli.net/2020/10/26/EjJIvB9iwmX5Ny1.png)

```html
<!-- 红盒子 -->
<div class="box"> 
    <!-- 绿盒子 -->
    <div class="inner-box">

    </div>
</div>

<script src="js/jquery-3.4.1.min.js"></script>
<script>
    $('.box').mouseenter(function () {
        console.log('mouseenter')
    })

    $('.box').mouseover(function () {
        console.log('mouseover')
    })

    // 两者 都是 鼠标进入 
    // mouseover 会触发多次, 进入子类元素时，会再次触发，从子类元素回到父类时，又会触发；来回在子类和父类间移动，就会来回触发，不停在控制台打印
    // mouseenter 进入父类盒子触发打印，顺势进入子类盒子，不会打印；也就是将整个父类盒子的范围作为主体，绑定的事件与子类无关
</script>
```



## jQuery 动画

```html
<div class="box"></div>

<button class="btn">显示</button>
<button class="btn">隐藏</button>

<button class="btn">slideDown滑入</button>
<button class="btn">slideUp滑出</button>

<button class="btn">fadeIn淡入</button>
<button class="btn">fadeOut淡出</button>
<button class="btn">fadeTo</button>

<script src="js/jquery-3.4.1.min.js"></script>
<script>
    //...
</script>
```



### 隐藏(hide)/显示动画(show)

+ $ ( selector ) . show  ( speed , callback ) ;    								   						
    + speed：显示动画的时长（毫秒单位）
        + 参数1 也可以填关键字 slow ( 600 )  /  normal ( 400 )  /  fast ( 200 )
    + callback：回调函数：显示动画结束后，执行的函数 function ( ) { … } ;
+ ​	隐藏动画 hide ( ) ， 同 show ( )

> - $ ( selector ) . show ( 2000 ) ;
> - $ ( selector ) . show ( slow ) ;     slow ( 600 )  /  normal ( 400 )  /  fast ( 200 )
> - $ ( selector ) . show ( 2000 , function ( ) { … } ) ;
> - $ ( selector ) . show ( ) ;

```javascript
$('.btn').eq(1).click(function () {
    $('.box').hide(2000) //用2秒时间来完成隐藏动作
});
$('.btn').eq(0).click(function () {
    $('.box').show(2000, function () { //2秒显示出来后，瞬间修改css样式宽变为100，颜色绿
        $(this).css({
            width: '100px',
            backgroundColor: 'green'
        })
    })
});
```



### 滑入滑出动画

+ 修改的是 . box 的 height 值
+ $ ( selector ) . slideDown/Up/Toggle ( speed , callback ) ; 
    + speed：显示动画的时长（毫秒单位）
        + 参数1 也可以填关键字 slow ( 600 )  /  normal ( 400 )  /  fast ( 200 )
    + callback：回调函数：显示动画结束后，执行的函数 function ( ) { … } ;

> - $ ( selector ) . slideDown ( speed , callback ) ;
> - $ ( selector ) . slideUp ( speed , callback ) ;
> - $ ( selector ) . slideToggle ( speed , callback ) ;

```javascript
$('.btn').eq(2).click(function () {
    $('.box').slideDown(1000)
});
$('.btn').eq(3).click(function () {
    $('.box').slideUp(1000)
});
```



### 淡入淡出动画

$ ( selector ) . slideDown/Up/Toggle ( speed , callback ) ; 

+ speed：显示动画的时长（毫秒单位）
    + 参数1 也可以填关键字 slow ( 600 )  /  normal ( 400 )  /  fast ( 200 )
+ callback：回调函数：显示动画结束后，执行的函数 function ( ) { … } ;

> - $ ( selector ) . fadeIn ( speed , callback ) ;
> - $ ( selector ) . fadeOut ( speed , callback ) ; 
> - $ ( selector ) . fadeToggle ( speed , callback ) ;   
> - $ ( selector ) . fadeTo ( speed , opacity ) ;   调节透明度

```javascript
$('.btn').eq(4).click(function () {
    $('.box').fadeIn(1000)  // 淡入
});

$('.btn').eq(5).click(function () {
    $('.box').fadeOut(1000)  // 淡出 
});

$('.btn').eq(6).click(function () {
    $('.box').fadeTo(1000, 0.3);  // 将被选元素.box的不透明度逐渐地改变为指定的值0.3
});
```



> 注意：省略参数或者传入不合法的字符串，那么则使用默认值：400 毫秒







## 自定义动画  和 停止动画

### 自定义动画

$ ( selector ) . animate ( styles , speed , ease , callback )

- 第一个参数表示：要执行动画的 CSS 属性（必选）
- 第二个参数表示：执行动画时长（可选）
- 第三个参数表示: 运动函数 ' swing ' 和 ' linear '
- 第四个参数表示：动画执行完后立即执行的回调函数（可选）

​                                       ↓↓↓↓

```html
<button class="start">开始</button>
<button class="stop">停止</button>
<script src="js/jquery-3.4.1.min.js"></script>
<script>
    // animate()
    $('.start').click(function () {
        $('.box').animate({
            width: '400px',
            left: '600px',
            top: '100px'
            // 颜色不能参与动画,但是可以借助一些 插件 实现
            // backgroundColor: 'green'
            // marginLeft: '600px'
        }, 1000, 'linear', function () {
            $('.box').animate({
                left: 200
            }, 1000, 'linear')
        })

        // 可以为他指定多个动画,但是动画会进入排队状态,上一个执行完,下一个才能开始
        // 回调函数异步 ,必须等同步执行完,他才能执行
        $('.box').animate({
            left: '1000px'
        }, 1000, 'linear')
    })


    // 如何停止动画 
    $('.stop').click(function () {
        // stop(stopAll,goToEnd)
        $('.box').stop(false, false)
    })
</script>
```

### 停止动画        ↑↑↑↑

- stop ( ) ;  停止当前动画

为什么要停止动画？如果多个动画同时作用于一个单位上面，那么多个动画会进入排队。后一个动画的执行必须等前面的执行完毕。

- stop ( stopAll , goToEnd )

stopAll：是否全部停止动画（停止队列中所有动画），默认 false goToEnd： 是否将停止的动画,停在当前动画的最后一个状态







## BOM 相关

### 宽高设置

（ 这两个方法( ) 并**不常用**，更多是通过css样式来设置或修改宽高）

- height ( )
- height ( 200 )
- width ( )
- width ( 100 )       取值类型为 `num` 可以直接参与运算

```javascript
// width() / height() 

console.log($('.box').width()) // 获取元素 宽和高  num 数值 可以参与运算
$('.box').width(300);  // 也可以用来设置宽高
```



### 坐标值操作：获取元素位置

- offset()
    - 作用：获取或设置元素相对于文档的位置
    - $(selector).offset();
    - $(selector).offset({left:100, top: 150});

```javascript
console.log($('.box').offset()); // 用于获取  {top: 100, left: 100}
console.log($('.box').offset().left); // 用于获取  100

$('.box').offset({   // 用于设置
    top: 200,   
    left: 150
})
```



- position()
    - 作用：获取相对于其最近的具有定位的父元素的位置。
    - $(selector).position();
    - 注意：只能获取，不能设置。

```javascript
console.log($('.inner').position()); // {top: 0, left: 80}
```



#### 回到顶部

- scrollTop()
    - 作用：获取或者设置元素垂直方向滚动的位置
    - $(selector).scrollTop();
    - $(selector).scrollTop(100);

```javascript
// 原生 js 获取 scrollTop 
var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
console.log(scrollTop)

// jQuery 获取 scrollTop 
console.log($(document.documentElement).scrollTop());

// 实例应用：回到顶部
$('.top').click(function () {
    // $(document.documentElement).scrollTop(0);  回到顶部效果
    
    $(document.documentElement).animate({  
        //回到顶部效果，上面和这个的最终呈现一样，但是animate实现了动画过渡，不会特别生硬
        scrollTop: 0
    }, 2000)
})
```



## on方式绑定事件

+ html

```html
<div class="box">
    <p>不凡</p>
    <a href="javascript:;">nihao</a>
</div>
```



### jQuery 正常绑定事件的方式

```javascript
$('.box').click(function () {  // 这里是 jq 正常绑定事件的方式
    console.log('click')
})
```



### on ( ) 方法 绑定事件的方式

```javascript
$('.box').on('mouseenter', function () { 
    console.log('你好') //鼠标移入 <p>不凡</p> 和 <a>nihao</a> 都会在控制台打印'你好'
})
```

+ on ( ) 方法：一共有三个参数
    + 第1个参数：是要绑定的事件名
    + **第2个参数：用来过滤后代元素（!）**
    + 第3个参数：是执行程序

```javascript
$('.box').on('click', 'p', function () { 
    // (第2个参数写了p,就只绑定p标签,点击a标签不再触发box 的点击事件)
    // 其实就相当于，获取了.box的jq对象，对其内部的p标签，进行绑定点击事件 和 执行程序
    console.log('点击')    // 点击.box里的p标签才会打印;点击a标签不再触发
})
```



### 如何解绑事件：off ( )

```javascript
$('.box').off( ); // 不写参数: 全部解绑
$('.box').off('mouseenter'); // 解绑括号里的'鼠标进入mouseenter'事件
```



---



## 事件委托

+ 使用 on 方式，将事件委托给父级元素代理。

```html
<ul>
    <li>li0</li>
    <li>li1</li>
    <li>li2</li>
</ul>
<button>新增</button>
```

+ 想要实现：动态生成多个 li ，并给 li 绑定事件和程序

```html
<script>
    var count = 2;
    $('button').click(function () { // 每次点击按钮，都生成一个 <li> li? </li>
        count++;
        $('ul').append('<li>li' + count + '</li>')
    })


    // 给每个 li 绑定单机事件：打印 this.li 里的文本内容
    $('li').click(function () {
        console.log($(this).text());  // 仅能打印出 li0 li1 li2 
    })
    // 因为对于动态生成的元素，会出现无法绑定事件的情况

    
    // 解决办法：
    // 利用on()方法绑定，可以解决这个问题
    // 给父级盒子绑定on('click')，筛选出里面的li标签，添加处理程序function，此时的this就可以获取到当前点击的li身上，这种方法就叫——事件委托
    
    // 事件委托
    // 把li的点击事件 委托给了 ul
    $('ul').on('click', 'li', function () {
        // 不加过滤元素('li'), this 指向 ul
        // 加上'li' ,之后 this  指向 点击的 那个 li

        console.log(this); // 此时点击哪个li就打印<li>li?</li> 出整个li内容

        console.log($(this).text()); //此时点击哪个li就打印哪个li里的文本内容 => li? 
    })


</script>
```

​	

---

​	

## 事件对象 event

### **打印 事件对象event **

```javascript
$('.box').click(function (event) {
    console.log(event); // 打印结果（见下图：信息非常之多）
    // console.log('你好');
    console.log('current====>', event.currentTarget);
    console.log('target===>', event.target);
    console.log('this====>', this)
})
```

![image-20191224200638274](https://i.loli.net/2020/10/26/tNEbKreq8ApjdLO.png)

### 常用事件属性、方法

- event.data     传递给事件处理程序的额外数据
- event.currentTarget     事件绑定的对象(事件源),和 this 相同
- event.pageX     鼠标相对于文档左部边缘的位置
- event.target     实际触发事件的对象，不一定===this
- event.stopPropagation()；   阻止事件冒泡
- event.preventDefault();     阻止默认行为
- event.type     事件类型：click，mouseover…
- event.which     鼠标的按键类型：左 1 中 2 右 3
- event.keyCode     键盘按键代码

+ ……



**举例演示：↓↓**

### 阻止事件冒泡、阻止默认事件

```javascript
$('ul').click(function () {
  alert('ul')
})
$('li').click(function (e) {
  e.stopPropagation(); // 阻止冒泡  //调用event方法,书写形式
  //如果不写 ↑↑ 这个方法 , 当我点击li时,同时触发ul的事件  // 会先弹出li,再弹出ul的弹窗
  //写了上面这个方法,就会阻止事件冒泡;  点击li,不再触发父级事件ul
  alert('li')
})
$('a').click(function (abc) {
  abc.stopPropagation(); // 阻止冒泡    // 调用event方法,书写形式
  abc.preventDefault(); // 阻止 a标签 默认的跳转行为  // 调用event方法,书写形式
  console.log('a');
})
// 很多情况下,我们想要实现阻止事件往外冒泡,只触发自己的；就可以调用事件对象里的stopPropagation()
```

-

```html
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: brown;
    overflow: hidden;
  }

  ul {
    margin: 50px auto;
    width: 100px;
    height: 100px;
    background-color: pink;
    list-style: none;
    overflow: hidden;
  }

  li {
    margin: 25px auto;
    width: 50px;
    height: 50px;
    background-color: orange;
    text-align: center;
    line-height: 50px;
  }
</style>

<!-- 阻止事件冒泡 -->
<div id="div">
  <ul id="ul">
    <li id="li">
      test
    </li>
  </ul>
</div>


<!-- 阻止默认事件 -->
<form action="/account/login" id="login-form" method="post">
  <p>账号: <input type="text" name="username" required /></p>
  <p>密码: <input type="password" name="pwd" required /></p>
  <p><input id="submit" type="submit" value="登录" /></p>
</form>
<a id="a" href="https://www.baidu.com">跳转百度</a>


<script>
  //阻止事件冒泡
  li.onclick = () => {
    alert('li');
  }
  ul.onclick = () => {
    alert('ul');
    event.stopPropagation ? event.stopPropagation() : event.cancelBubble = true;
  }
  div.onclick = () => {
    alert('div');
  }


  //阻止默认事件
  submit.onclick = () => {
    event.preventDefault ? event.preventDefault() : event.returnValue = false;
  }
  a.onclick = () => {
    event.preventDefault ? event.preventDefault() : event.returnValue = false;
  }
</script>
```

​	

​	

### **键盘按键代码 . keyCode     **

+ 补充 keyup ( ) 方法：效果是 监听到每次键盘按键升起时，就执行该方法下的函数操作 
+ 本例想要实现的效果是： 按下enter键(13)的时，将当前input输入框里输入的内容，在p标签里显示

```javascript
$('input').keyup(function (e) { 
    var txt = $(this).val(); // 先设置，将当前input输入框内的内容，赋值给变量txt
    console.log(e.keyCode) //调用keyCode 可实现打印键盘按下的按键的数字代码，测出enter是13
    $('p').text(txt); // 监听到每次键盘按键升起时,直接将输入框的text内容,添加到p标签里显示
    //完成

	// 也可以用if语句判断 当前按下的按键的keycode是否是13，是则将txt内容添加到p标签中
    if (e.keyCode === 13) { 
        //当按下回车时(回车键代码为13),将输入框的txt内容,添加到p标签里显示
        $('p').text(txt);
    }
})
```

​	

​	

---

​	

## jQuery 链式编程

- 链式编程原理：return this；调用“任何”一个方法都是返回了对象本身
- 通常情况下，只有设置操作才能把链式编程延续下去。因为获取操作的时候，会返回获取到的相应的值，无法返回 this。
- end ( ) ：结束当前 this 并回到匹配元素之前的状态，让this重新指向了 最初的调用者

```javascript
$(".inner-box").click(function() {
    $(this) 
        .css("width", "80px")  // 设置当前点击的innerbox的宽为80，return $(".inner-box")
        .css("font-size", "40px")// 因为返回的是对象本身，所以这里就相当于对象本身在调用css()
        .parent()               // 这里相当于$(this).parent()，拿到this的父级对象
        .css("width", "200px")  // this的父级对象调用css()，修改其宽度
        .end()          // 结束了this指向父级的操作,让this重新指向了$(.inner-box).$(this)
        .css("background-color", "black");     // $(.inner-box).$(this) 调用css()方法

    // 结束当前链最近的一次过滤操作，并且返回匹配元素之前的状态。
});
```

​	

​	

## 隐式迭代

- 隐式迭代的意思是：在方法的内部会为匹配到的所有元素进行循环遍历，执行相应的方法；而不用我们再进行循环，简化我们的操作，方便我们调用。
- **如果获取的是多元素的值，大部分情况下返回的是第一个元素的值**。
    - 文本比较特殊，如果要返回元素的文本内容，则会将全部元素的文本内容都返回，不会只返回第一个元素的文本内容

```javascript
$(".btn").click(function() {
	console.log($(".box").text());
});
```

+ 实例 ↓↓

```html
<ul>
  <li>li-1</li>
  <li>li-2</li>
  <li>li-3</li>
</ul>
<div class="btn">点击</div>
<script src="js/jquery-3.4.1.min.js"></script>
<script>
  $('li').click(function () {
    console.log($(this).text());
  })

  $('.btn').click(function () {
    console.log($('li').text()); // li-1 li-2 li-3
    //文本比较特殊, 点击btn后【会返回所有的li里的文本内容】, 不会只返回第一个li的文本内容

    console.log($('li').css('color')); // green
    // 但是,如果要打印文本以外的值，例如color值,此时只会返回 第一个li的颜色 绿色【不会返回所有li的color值】
  })
</script>
```





## each 循环

- 大部分情况下是不需要使用 each 方法的，因为 jQuery 的隐式迭代特性 ↑↑ ：会为自动为匹配到的所有元素进行循环遍历

-  each  类似于 数组的 forEach 方法

    - ```javascript
        var arr = ['小明', '张三', '李四'];
        arr.forEach(function (item, index) {
          console.log('item====> ', item);
          console.log('index====>', index);
        })
        // 打印结果如下：
        //          item====>  小明
        //          index====> 0
        //          item====>  张三
        //          index====> 1
        //          item====>  李四
        //          index====> 2
        ```

- 如果要对每个元素做不同的处理，这时候就用到了 each 方法

- 作用：遍历 jQuery 对象集合，为每个匹配的元素执行一个函数

- $(selector).each(function(index,element){});

- Element 是一个 js 对象，需要转换成 jquery 对象

```javascript
var lis = $('li');
lis.each(function (index, item) {
    //index - 选择器的 下标 位置
    // item  正在遍历的  dom元素
    console.log(index, item); //  0  <li>li-1</li>
    						  //  1  <li>li-2</li>
    						  //  2  <li>li-3</li>
    if (index % 2 === 0) { // 意为 下标为偶数
        $(item).click(function () {
            $(this).css('color', 'red'); //点击 下标为偶数的li,就会让当前li变成 红色
        })
    } else { //反之,也就是下标为奇数的,执行下面的函数
        $(item).click(function () {
            $(this).css('color', 'green'); //点击 下标为奇数的li,就会让当前li变成 绿色
        })
    }
})
```

+ 在 **“ 01-DOM操作 ” **中 有一个案例：**for循环 实现 “ 隔行变色 ↓↓ ” **，那在 **jQuery **中就可以使用 **each ( ) 方法 ↑↑ **来实现

```html
<script>
  // 实现 奇数行 显示red ; 偶数行 显示green 
  // 鼠标移入 this行 显示orange

  var liArr = document.getElementsByTagName('li');
  console.log(liArr);

  var colorBefore;
  for (var i = 0; i < liArr.length; i++) {   // for循环遍历li
    if (i % 2 === 0) { // 奇数行 红色
      liArr[i].style.backgroundColor = 'red';
    }
    if (i % 2 !== 0) { // 偶数行 绿色
      liArr[i].style.backgroundColor = 'green';
    }

    // 鼠标移入：变橙色，
    // 鼠标离开：变回原来颜色
    liArr[i].onmouseenter = function () {
      colorBefore = this.style.backgroundColor;
      this.style.backgroundColor = 'orange';
    }
    liArr[i].onmouseleave = function () {
      this.style.backgroundColor = colorBefore;
    }
  }
</script>
```



##  多库共存

- 此处多库共存指的是：jQuery 占用了\$ 和 jQuery 这两个变量。当在同一个页面中引用了 jQuery 这个 js 库，并且引用的其他库（或者其他版本的 jQuery 库）中也用到了$或者 jQuery 这两个变量，那么，要保证每个库都能正常使用，这时候就有了多库共存的问题。
- \$.noConflict();  让 jQuery 释放对\$的控制权，让其他库能够使用$符号，达到多库共存的目的。此后，只能使用 jQuery 来调用 jQuery 提供的方法



## 插件机制

- jQuery 这个 js 库，虽然功能强大，但也不是面面俱到包含所有的功能。
- jQuery 是通过插件的方式，来扩展它的功能：
- 当你需要某个插件的时候，你可以“安装”到 jQuery 上面，然后，使用。
- 当你不再需要这个插件，那你就可以从 jQuery 上“卸载”它。


