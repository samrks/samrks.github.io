# DOM 编程 


<!--more-->

​	

## 前言

>   本节是 DOM 最基础的部分

​	

## 前置知识

>   需要什么知识

-   理解简单的 JS 语法，如 变量、if else、循环
-   会背 JS 的七种数据类型（四基两空一对象、bigInt）
-   会背 JS 的五个 falsy 值 （0，NaN，null，undefined，空字符串）
-   知道函数是对象，数组也是对象
-   会用 div 和 span 标签
-   会简单的 CSS 布局（flex）

​	

## 网页其实是一棵树

>   第一个知识点

![image-20201021011318799](https://i.loli.net/2020/10/21/p9d1Psvb4B7RSDy.png)

画成「树」

![image-20201021011354271](https://i.loli.net/2020/10/21/JnhEHrpvxoYOTFB.png)

### JS 如何操作这棵树

-   JS 只能操作 JS，是操作不了网页的
-   **浏览器提供了功能，往 window 上添加了一个 document **
-   只要有 document 这个对象， JS 就可以操作这棵树了



示例

```js
用chrome打开任意网站
在控制台键入： window.document  得到一个 #document 
鼠标放在 `#document` 上会发现整个网页被选中了，说明document包含整个网页
```

>   通过 window.document 得到网页的根节点
>
>   +   根节点下有 head 、 body …

![image-20201021012457724](https://i.loli.net/2020/10/21/1WdSk8jqsaLwgM6.png)

​	

### JS 用 document 操作网页

>   这就是 Document Object Model 文档对象模型

+   「用一个 document 对象来操作整个网页」这种思想(模型)，全称叫做 「Document Object Model」
+   简称 DOM

	

### DOM 很难用

>   请记住这个事实

+   之前讲过「JS 的原创之处并不优秀，优秀之处并非原创」
+   DOM 可能比 JS 还要难用
+   难用到「都没人愿意使用 DOM」

>   下面会想办法，解决这个难题

​	

### 如果你觉得 DOM 很智障

>   不要怀疑自己，你觉得的是对的

+   DOM 的接口设计的非常反人类
+   导致前端人员，不得不使用 jQuery 来操作 DOM
+   后来 jQuery 又被 Vue 代替了，于是大多数人就用 Vue 来操作 DOM
+   后来又有 React 了，就用 React 来操作 DOM
+   从来不会用 DOM 自带的功能来操作 DOM，自带的功能实在是非常反人类

​	



 <img src="http://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201027005340133.png" alt="image-123340133" style="zoom:67%;" /> [图片来自](https://javascript.info/dom-navigation)

​	

​	

## 获取元素的 API

>   获取元素，也叫标签

>   什么是 API —— 没有准确定义，听多了你自然就知道什么是 API 了

### 有很多 API

#### 通过 id 

```js
window.id名
id名
document.getElementById('id名')
```

举例 👇

```html
<input id="kw">
```

```js
window.kw                     // <input id="kw">   （直接获取到这个标签）
kw                            // <input id="kw">
document.getElementById('kw') // <input id="kw">   （已经有上面两个特别简单的写法，谁还用这个）
```

特例 👇

>   当 id 名为 JS 关键字/属性时，就不能通过前面两个简单的写法来获取到元素

```js
// 如下图所示
window.parent                      // parent 在这里是【获取 window 的上一层窗口】的意思
document.getElementById('parent')  // 此时，只能通过此写法
```

<img src="https://i.loli.net/2020/10/21/eHXtaxwAOvundbM.png" alt="image-20201021022844479" style="zoom: 67%;" />

>   所以，只要 id 不与全局属性冲突，就可以最简单的直接用这个 id
>   如果不小心冲突了，就只能退而求其次，用这个很长的 API

​		

#### 通过 标签名

```js
/*
 *  找到所有标签名为 div 的元素。
 *  拿到的是一个数组（伪数组）
 *  要获取到具体某一个 div，需要用下标（也可以遍历）
*/
document.getElementsByTagName('div')[0]   
```

​		

#### 通过 class 获取元素

```js
/*
 *  找到所有 class 为 red 的元素。
 *  拿到的是一个数组（伪数组）
 *  要获取到具体某一个 red 元素，需要用下标（也可以遍历）
*/
document.getElementsByClassName('red')[0]
```

​	

#### 最新的 API ：query

>   虽然是 query 开头，但并不是 jQuery 提供的 API，而是 JS 原生的
>   [querySelector 和 getElement(s)ByXxx 方法的区别](https://www.imooc.com/article/13027)

>   querySelector()，接收一个CSS选择符，返回与该模式匹配的第一个元素
>   querySelectorAll()，用于选择匹配到的所有元素

```js
document.querySelector('#id名')    // 借用了css语法，css怎么找到这个标签，括号中就怎么写
document.querySelectorAll('.red')[0]
```

​		

​		

### 用哪一个 ⭕️

-   工作中用最新的， querySelector 和 querySelectorAll  
-   做 demo 直接用 idxxx，千万别让人发现 
-   要兼容 IE 的可怜虫才用 getElement(s)ByXxx

​	

​	

### 获取特定元素的 API

#### 获取 html 元素

```js
document.documentElement
```

#### 获取 head 元素

```js
document.head
```

#### 获取 body 元素

```js
document.body
```

#### 获取窗口（窗口不是元素）

```js
window
```

#### 获取所有元素

```js
document.all
```

-   这个 document.all 是个奇葩，第 6 个falsy值

​	

​	

## 获取到的元素是个啥

>   显然是一个对象，我们需要搞清它的**原型**

### 抓一只 div 对象来看看

（图示见[下一P](# div 完整原型链)）

```JS
let div = document.getElementsByTagName('div')[0]   // www.baidu.com
console.dir(div)  
// 用 dir 可以打印出结构。 （ 如下图，会有很多属性，都是构造函数添加的 ）
// 重点关注【原型链】 
```

![image-20201021030553549](https://i.loli.net/2020/10/21/gtmyX7VYC4IxicZ.png)

>   注意：这里写的 HTMLDivElment 不是真正的原型

```js
div.__proto__ === HTMLDivElment            // false
div.__proto__ === HTMLDivElment.prototype  // true   
```

```js
// JS 经典公式
对象.__proto__ === 其构造函数.prototype
```

>   console.dir(div)  开始查看原型链

1.   点开最外层 `div#wrapper.wrapper_new`，最先看到的是这个 **div 自身的属性**

2.   第一层原型 **HTMLDivElement**.prototype

     -   点开，这里面也是有很多属性，是**所有 div 共有的属性**，不用细看

3.   第二层原型 **HTMLElement**.prototype

     -   这里面是**所有 HTML 标签共有的属性**，不用细看

4.   第三层原型 **Element**.prototype

     -   这里面是**所有 XML、HTML 标签的共有属性**，你不会以为浏览器只能展示 HTML 吧
     -   AJAX 的 X 指的就是 XML。在没有发明 json 之前，全部处理的都是 XML（XML 里也有标签）
     -   具体这里包含 XML、HTML、SVG、… 各种不同标签都共有的属性，所以叫 Element
     -   在 Element 各种元素之上，我们还有👇节点node的属性

5.   第四层原型 **Node**.prototype

     -   这里面是**所有节点共有的属性**，节点包括 XML 标签文本注释、HTML 标签文本注释等等

6.   第五层原型 **EventTarget**.prototype

     +   只有 3 个共有属性：addEventListener、dispatchEvent、removeEventListener

     -   最重要的函数属性是 **addEventListener**

7.   最后一层原型就是 **Object**.prototype（根对象）了 

     +   再往上就是 null 了

>   综上，div 是个非常复杂的对象 

​		

​		

### div 完整原型链

>   自身属性和共有属性，[点击查看](https://i.loli.net/2020/10/21/bwt5rm24PC8IXa6.png)
>
>   [本地查看](D:\Jirengu\第1阶段\7-JS编程接口\1-DOM编程\div完整原型链.png)

![2019-10-17-20-36-26](https://i.loli.net/2020/10/21/bwt5rm24PC8IXa6.png)

```js
let div = document.querySelectorAll('.red')[0]
```

+   由于 div 是由 HTMLDivElement 构造的
+   HTMLDivElement 构造函数往 this 上添加了一些属性 （this 指代 div）
+   div 也继承了 Element，所以 Element 也往 this 上添加了一些属性
+   还继承了 node 构造函数，添加了一些属性
+   综上，每一层构造函数，都会往 div 身上添加了属性

​	

>   例 👇

```js
requestFullScreen()   // 请求全屏（是所有Element的共有属性）
```

<img src="https://i.loli.net/2020/10/21/185o9xEOrBRnSjD.png" alt="image-20201021033538316" style="zoom:67%;" />

```js
head.requestFullScreen() // head元素全屏显示  // 这个 API 兼容性一般不是很好，通常不会使用
// 每一个元素都可以要求自己跟屏幕一样大
// head 能调用到这个 API , 就是因为顺着【原型链】继承而来
```

​	

### 总结

-   这样我们就可以清楚的知道，获取到的 div  是个啥了
-   就是个对象，且有 6 层原型

​	

​	

​	

## 节点类型 nodeType

节点？元素？傻傻分不清

>   一个网页里面，节点包括很多种。最常见的就是元素、也叫标签

>   节点 Node 包括以下几种

+    1   表示元素 Element，也叫标签 Tag  （最常见）
+    3   表示文本 Text
+    8   表示注释 Comment
+    9   表示文档 Document 
+    11  表示文档片段 DocumentFragment

（记住 1 和 3 即可）

>   [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType) 有完整描述，通过 **x.nodeType ** 得到一个数字

```js
// 在任何一个元素上通过 .nodeType 就可以获取到这个元素的类型
div.nodeType  // 1  表示标签
```

示例

```js
// www.baidu.com
let div = document.getElementsByTagName('div')[0]
div.nodeType  // 1
div.childNodes  // 查看div的所有子节点  // 得到 Nodelist(5) 伪数组。 0: text 第一个子节点就是文本
div.firstChild  // #text  获取到文本节点
div.firstChild.textContext  // 获取文本里面的内容  "  "  是个空格
div.firstChild.nodeType  // 3  获取到div的第一个子节点的节点类型就是 3 文本
```

![image-20201021135544247](https://i.loli.net/2020/10/21/vIEMQC3JioRr41f.png)

​	

​	

​	

## 节点的增删改查

>   程序员的宿命就是增删改查
>
>   +   后端，对【数据库】进行增删改查
>   +   前端，对【页面元素】进行增删改查

## 增

### 创建一个标签节点

```js
let div1 = document.createElement('div')  // “DOM反人类”再次得到验证：创建一个元素居然写这么长的单词
document.createElement('style') 
document.createElement('script') 
document.createElement('li')
// <div></div> 、 <style></style> 、 <script></script> 、 <li></li>
```

### 创建一个文本节点

```js
text1 = document.createTextNode('你好'） 
// 为什么不能直接写成 text1='你好'
// 因为 '你好' 是一个字符串；而文本节点是一个【对象】（包含很多原型、函数什么的）
```

### 标签里面插入文本

>   两种形式、3 种写法

```js
div1.appendChild(text1) 
div1.innerText = '你好'   或者   div1.textContent = '你好'
// 但是不能用 div1.appendChild('你好'）
```

+   [appendChild](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/appendChild) 是 Node 构造函数 添加的
+   [textContent](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent)  也是 Node 构造函数 添加的
+   [innerText](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/innerText) 是 HTMLElement 构造函数 添加的

>   +   喜欢        Node        就用         Node        提供的接口
>       喜欢 HTMLElement 就用 HTMLElement 提供的接口
>
>   +   **但是不能混着用**
>       示例如下图，会报错：appendChild 只能接收一个 node 节点

<img src="https://i.loli.net/2020/10/21/iIrlXLUys3z8baF.png" alt="image-20201021141218590" style="zoom:67%;" />

### 综上

```html
<div>你好</div>
```

```js
// 通过 DOM 创建上面这个html标签 👆
// 需要下面 3 行代码，才能实现
let div1 = document.createElement('div')
let text1 = document.createTextNode('你好'）
div1.appendChild(text1)
```

>   此时的 div1 并不会显示在页面中，只是在 JS 内存中存活
>   只有插入页面中，这个 div1 才能生效（显示）

​	

### 插入页面中

-   创建的标签，默认处于 JS 线程中
    -   不会显示在页面中，只是在 JS 内存中存活
-   你必须把它插到 head 或者 body 里面，它才会生效、显示在页面中
    -   创建的是 style 或 link 元素 …  就需要传入到 head 里才能生效
-   appendChild 会把元素插入到目标容器的**末尾**

```js
document.body.appendChild(div) 
document.head.appendChild(div)
// 或者
已在页面中的元素.appendChild(div)
```

例

```js
let div1 = document.createElement('div')  // 创建div元素
let text1 = document.createTextNode('你好'）// 创建文本节点
div1.appendChild(text1)  // 通过appendChild把文本节点添加到div元素中。此时div仍在内存中，不在页面显示
document.body.appendChild(div1) // <body><div>你好</div></body>  此时 div 显示在页面中
```

​		

​		

### appendChild 疑问

#### 一个元素，只能插入一处

>   页面中有 div#test1 和 div#test2 

```js
let div = document.createElement('div') 
test1.appendChild(div) 
test2.appendChild(div)  
// 创建一个 div 元素，先后插入到另外两个div中
// 请问新创建的这一个 div 元素，最终会出现在哪里？
```

+   请问最终 div 出现在哪里？

    +   选项1、test1 里面 

    +   选项2、test2 里面 

    +   选项3、test1 里面 和 test2 里面

+   答案：（鼠标选中显示答案）👉 <font color="white">最终 div 出现在 test2 里面</font>

    >    因为**一个元素不能出现在两个地方，除非复制一份**

    同理：送子观音，把一个孩子送到第一户人家，又把他送到第二户人家，那最后在哪降生？ 答：第二户人家
    因为一个孩子只会有一个亲生家庭

​	

示例

>   尝试用 appendChild，把创建好的元素，先后添加到两个地方（无法实现一个元素插入多处）

```js
let div1 = document.createElement('div')  
let text1 = document.createTextNode('你好')
div1.appendChild(text1)  
div1.style.backgroundColor = 'white'
div1.style.fontSize = '100px'
// 此时内存中有一个 <div>你好</div> 元素。 背景白色，字体100像素
// 把这个 div1 元素，先后插入到 head 和 body 中
document.head.appendChild(div1)
document.body.appendChild(div1)  // 最终div1元素只会出现在body中 
```

​		

​	

#### 复制元素，使插入多处

>   用「克隆节点」 [cloneNode MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/cloneNode)

```js
let div2 = div1.cloneNode(true)  
// true 深拷贝：该节点的所有后代节点也都会被克隆 //  false 浅拷贝：只克隆该节点本身
document.head.appendChild(div1)
document.body.appendChild(div2)
```

​	

​	

## 删

### 两种方法

```js
// 旧方法 👇
parentNode.removeChild(childNode)  // 必须找到父节点，来删除子节点 （反人类）

// 新方法 👇
childNode.remove()  // IE不支持（兼容性有问题）
```

#### 临时探讨：为什么 IE 总搞独立？

-   实际上在最初，IE 确实是最厉害的浏览器。其他各家浏览器公司为了反制 IE 才搞出「标准」
-   所以不是 IE 搞独立，而是标准出的太晚了
-   IE 一家独大的时候，标准还没有出台。IE 也不会提前知道标准的内容。
    而且标准中某些内容还会故意跟 IE 反着写
-   比如 IE 发明了 innerText，标准出台表示不用 innerText 而用 textContent（就导致开发者两个写法都要记😭）
-   所以并不一定是 IE 故意搞独立，会不会是「标准」在故意使坏呢 ?!  （🤔盲生你发现了华点）

​	

#### 旧方法

```js
// 先创建出 div1，再克隆出 div2
let div1 = document.createElement('div')  
let text1 = document.createTextNode('你好')
div1.appendChild(text1)
let div2 = div1.cloneNode(true)

// 把 div1/div2 元素，分别插入到 head/body 中
document.head.appendChild(div1)
document.body.appendChild(div2) 

// 再删除节点
div1.parentNode.removeChild(div1)
div2.parentNode.removeChild(div2)

// 删除后还能再添加回来吗 ？
// 可以的。因为删除节点后，节点还在内存里面，所以还可以添加回来
document.body.appendChild(div2)  
```

#### 新方法

```js
// 先创建出 div1，再克隆出 div2
// 把 div1/div2 元素，分别插入到 head/body 中

// 再删除节点
div1.remove()
div2.remove()

// 删除后还能再添加回来吗 ？
// 可以的。因为删除节点后，节点还在内存里面，所以还可以添加回来  // 跟旧方法的效果一模一样
document.body.appendChild(div2)
```

>   由于【 `ele.remove()` 】是后发明的，所以不兼容 IE

​	

​	

### 思考

-   如果一个 node 被移出页面（DOM 树）
-   那么它还可以再次回到页面中吗？
    -   答案：可以。（示例参考上面）
    -   只是被移出来，并没有被彻底干掉，所以还可以存在在 JS 内存中


​	

### 如何彻底干掉元素

>   即元素被删除后，就彻底消失、不存在在内存中、也无法重新添加回页面

```js
// 彻底干掉元素，先把元素移出页面
div1.remove()
div2.remove()  
// 这时 div1/div2 都在内存中
div1 = null
div2 = null  
// 等于空，div1/div2 就与内存断开联系了，就会被【垃圾回收】掉
```

​	

​	

​	

## 改 💡

### 改属性

#### 写标准属性

##### 改 class

>   科普：早期 JS 对象是不能拥有一个「保留字」作为 key 的

```js
div1.if  
// JS引擎的解析器看到 if 会认为是不是 if 语句，而实际上 if 是 对象 div1 的 一个key
// 这就会导致歧义
// 所以 JS 不接受「保留字(JS关键字...)」作为 key

div1.class = "red"  // 修改失败
// 因为 class 也是JS关键字，所以不能使用
// 于是就起了新的名字，用 className 表示标签の类名 class
```

正确写法 👇

>   注意：每次用 className 修改类名，都会把之前的类名 直接覆盖掉

```js
// 如果只是想【追加】类名
div.className = 'red blue'  // 用 className 就把所有类名都写上
div.classList.add('red')    // 或者通过👈方式追加类名
```

```js
div.classList  // 可以查看div元素当前已有的class类名组成的数组
```

​	

##### 改 style

```js
div.style = 'width:100px;color:blue;'  // 全覆盖  【不推荐】
div.style.width = '200px'              // 改一部分【推荐】
div.style.backgroundColor = 'white'    // 注意「驼峰命名」的大小写
```

>   JS 不支持有「中划线 - 」的 key 值

```js
div.style.background-color    // ❌ 中划线会被理解成：减号
div.style['background-color'] // ⭕ 就是某些情况支持 key 包含中划线，也只能用[]中括号的形式
div.style.backgroundColor     // ✅
```

​	

##### 改 data-* 属性

>   以前有段时间，需要往元素上添加自定义属性。现在基本没人用了（库开发者可能会用到）

```js
// 添加自定义属性
div.setAttribute('data-x', 'test')  // <div data-x="test"></div>
div.dataset.xx = 'sam'              // <div data-xx="sam"></div>

// 获取自定义属性的属性值
div.getAttribute('data-x')   // test  
div.dataset.xx               // sam

// 修改自定义属性的值
div.dataset.xx = 'jack'      
```

​	

​	

#### 读标准属性

```js
div.id
div.classList
a.href 
// 👆 大多是属性都是一一对应，直接读就可以
```

##### 获取原本的属性值

>   不想被浏览器加工

```js
div.getAttribute('class')
div.classList

a.getAttribute('href')
a.href
```

>   两种方法都可以，但值可能稍微有些不同（大多情况两种方法获取的结果是一样的）

+   一种是简单的书写方式，「xxx.属性名」，但值可能被加工
+   一种是较长的书写方式，「xxx.getAttribute('属性名')」，虽然长，但可确保结果更准确，更保险一点

	

##### 举例：a 标签的[特殊情况](https://jsbin.com/suqesar/edit?html,js,console) 👇

```html
<a id=test href='/xxx'></a>
```

```js
console.log(test.href)  // https://null.jsbin.com/xxx  浏览器把域名给补全了
```

![image-20201021162116652](https://i.loli.net/2020/10/21/Li3TgSDJ1tCdIY9.png)

>   如果直接用 js 的属性，读出值。得出的结果，有可能会被浏览器加工

​	

​	

### 改事件处理函数

#### div.onclick 默认为 null

>   [代码在线编译](https://jsbin.com/qujosiw/edit?html,js,console,output)

```html
<div id="test">test</div>
```

```js
console.log(test.onclick)  // null 
```

```js
test.onclick = () => {
  console.log('hi')
}
```

```js
test.onclick = function(x){
  console.log(this)  // this: test
  console.log(x)     //    x: event
}
// 调用原理：test.onclick.call(test, event)
// 所以 this 和 event ，实际上是浏览器在用户点击时，用 call 传进来的
```

-   每一个元素都有 onclick 属性，该属性的默认值为 null
-   默认点击 div 不会有任何事情发生
-   但是如果你把 div.onclick 改为一个函数 fn 
    -   那么点击 div 的时候，浏览器就会调用这个函数
    -   并且是这样调用的 fn.call(div, event)
        -   div 会被当做 this   （如果要用到 this 就不能用箭头函数、必须用 function）
        -   event 则包含了点击事件的所有信息，如坐标


​	

#### div.addEventListener

>   是 div.onclick 的升级版，之后的课程单独讲 DOM Event

+   div.onclick 只能写一个函数（点击时，执行的所有操作，都必须写在一个函数中）
+   div.addEventListener 可以写无数个函数（点击时，执行的所有操作，可以写作不同的函数，对于复杂的操作情况非常友好）

​	

​		

### 改内容

#### 改文本内容 

```js
div.innerText = 'xxx'    // 早期 IE 发明的
div.textContent = 'xxx'  // 出台「标准」中规定的
```

+   两者几乎没有区别 
+   现在所有浏览器基本都是同时支持这两种写法的

​	

#### 改HTML内容

[代码在线编译](https://jsbin.com/vokuqoj/edit?html,js,output)

```js
div.innerHTML = '<strong>重要内容</strong>' 
```

+   innerText ：所有内容都会被识别为 文本
+   innerHTML ： 会解析内容、识别标签…
    （内容需要注意长度，超过一定限度，如20000个字符，可能会导致浏览器卡爆）

​	

#### 改标签

```js
div.innerHTML = ''      // 先清空 
div.appendChild(div2)   // 再加内容
```

​	

​	

### 改爸爸

>   想要找一个新爸爸

```js
newParent.appendChild(div)  // 直接把div添加到新的父节点内，以前父节点中的div就自动消失了
```

+   直接这样就可以了，直接从原来的地方消失

	

	

	

## 查

### 查爸爸 

```js
node.parentNode 
// 或者 
node.parentElement
```

​	

### 查爷爷

```js
node.parentNode.parentNode
```

​	

### 查子代

```js
node.childNodes  // 包含文本节点
// 或者 
node.children    // 【优先使用】不包含文本节点
```

>   [查看在线代码](https://jsbin.com/miqahoj/edit?html,js,console,output)

#### childNodes

例1

```html
<ul id=test>
	<li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```

```js
console.log(test.childNodes.length)   // 7
```

+   第 1 个子节点是：回车 + 空格，最终缩成一个**空格 **（最开始讲 html 时就讲过，不论几个回车空格，都会缩成一个空格）
+   第 2 个子节点是：li
+   第 3 个子节点是：回车 + 空格，最终缩成一个**空格 **
+   第 4 个子节点是：li
+   第 5 个子节点是：回车 + 空格，最终缩成一个**空格 **
+   第 6 个子节点是：li
+   第 7 个子节点是：回车 + 空格，最终缩成一个**空格 **

例2

```html
<ul id=test2><li>1</li><li>2</li><li>3</li></ul>
```

```js
console.log(test2.childNodes.length)   // 3   // 因为没有回车和空格
```

​	

#### children

```html
<ul id=test>
	<li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```

```js
console.log(test.children.length)   // 3     // 即使有回车空格，子元素也是 3 个
```

>   所以「查子代」优先使用 children

​	

#### 思考：元素节点的变化

>   当子代变化时，childNodes 和 children 也会实时变化吗？

[在线代码：children / childNodes](https://jsbin.com/rulicaz/edit?html,js,console,output)

答：如果子元素变化，children / childNodes 的长度**也会变化 **

[在线代码：querySelectorAll](https://jsbin.com/duqexed/edit?html,js,console,output)

答：通过 document.querySelectorAll 获取子元素集合。如果子元素变化，集合的长度**不会变化 **
querySelectorAll 不会实时根据元素变化，去改变自己。获取过一次之后，就不再变化

​	

​	

### 查兄弟姐妹

>   没有 API 可以直接获取到「任一元素的兄弟元素」，只能通过「先获取到父元素，再获取父元素的子元素」的方式

```js
node.parentNode.childNodes   // parentNode 可以和 parentElement 互换
node.parentNode.children     // 同上

// 发现问题：上面获取到的数组，都包含了自己，而需求是只要兄弟元素，过滤掉自己  // 只能遍历
// children 遍历排除自己
// childNodes 遍历排除自己和所有文本节点（更麻烦）
```

#### 例

```js
let div2 = document.getElementsByTagName('div')[1]
// div2 有多少个兄弟姐妹
let siblings = []
let c = div2.parentElement.children // 先获取到父亲的所有子代，再遍历从中排除自己
for(let i = 0; i > c.length; i++){
  if(c[i] !== div2){
    silblins.push(c[i])
  }
}
```

​	

​	

### 查看第一个子代

```js
node.firstChild    // node.parentNode.children[0]
```

​	

### 查看最后一个子代

```js
node.lastChild
```

​	

### 查看上一个兄弟

距离自己最近的上一个兄弟节点（哥哥）

```js
node.previousSibling          // 如果上一个节点是文本节点(空格、回车...)，就会获取到文本节点
node.proviousElementSibling   // 会避开文本节点，只找元素节点
```

补充：[节点类型](# 节点类型 nodeType)

​	

### 查看下一个兄弟

距离自己最近的下一个兄弟节点（弟弟）

```js
node.nextSibling         // 下一个兄弟节点 （包含文本节点）
node.nextElementSibling  // 下一个元素兄弟节点 （只包含元素节点）
```

​	

### 遍历一个 div 里面的所有元素

>   与前面的「数据结构——遍历树」逻辑相同

>   DOM 就是一棵树。数据结构中「树」的所有算法都可以用到 DOM 中

```js
travel = (node, fn) => {   // 与遍历树节点的代码逻辑一致
  fn(node)
  if(node.children){ 
    for(1et i = 0；i < node.children.length; i++){ 
      travel(node.children[i], fn) 
    } 
  }
}
travel(div1, (node) => console.log(node) ) 
```

+   看，数据结构多么有用

	

	

	

## DOM 操作是跨线程的（详解）📌

>   面接常问：为什么 DOM 操作比较慢？
>   因为 DOM 操作和 JS 是两个不同的东西。

>   下面就详细解释：什么叫「DOM 操作是跨线程的」

### 浏览器功能划分

>   在《JS世界》那一节里，讲过浏览器功能划分

-   浏览器有两个重要功能：「渲染引擎」和「JS 引擎」
    -   渲染引擎：专门用来渲染 html 和 css
    -   JS 引擎：专门用来执行 JS 
-   这两个引擎在不同的线程中，互不相干

​	

### 跨线程操作

#### 这两个线程-各司其职

-   JS 引擎，不能操作页面，只能操作 JS （window对象、object 对象、document对象…之类的）

    -   虽然存在 document 对象，但不能通过它操作任何事情，它只能操作document对象本身 —— 只能操作 JS 

-   渲染引擎，不能操作 JS，只能操作页面

-   这就是【各司其职】

```js
document.body.appendChild(div1)  
// 但是执行这句JS的调用语句，却使页面发生了改变。这不就违背了【各司其职】的原则 ❓❓
```

>   既然是各司其职：JS 引擎 只能操作 JS，渲染引擎 只能操作 页面
>
>   +   怎么让 div 出现在屏幕中的 ？
>   +   理论上，它只能出现在 body 的内存里面
>   +   这句 JS 的调用语句到底是如何操作、改变页面的  ❓❓   —— [跨线程通信](# 跨线程通信)

​	

#### 跨线程通信

-   当浏览器发现 JS 要在 body 里添加一个 div1 对象
-   浏览器就会通知渲染引擎：
    -   在页面里也新增一个 div 元素
    -   新增的 div **元素**的所有属性，都照抄 div1 **对象**

>   所以不是 JS 去渲染、改变了页面，而是浏览器去渲染、改变了页面

​	

#### 图示跨线程操作

<img src="https://i.loli.net/2020/10/22/l9vU7YNTzidV2aA.png" alt="image-20201022002103175" style="zoom: 67%;" />

+   左【JS 执行线程】、中【浏览器】、右【渲染线程】  各自独立的

+   ```js
    let div = doc.createElement('div')  // 不会影响页面，只改变了JS 执行线程的内存
    div.textContent = 'hi'              // 也不会影响页面（第1次改变文本内容）
    document.body.appendChild(div)      
    // 浏览器发现JS往body里添加了div节点，浏览器就通知了渲染引擎【慢】。
    // 渲染引擎接到通知，就往body里添加了div元素（div元素的属性照搬div节点的属性）
    ...
    div.textContent = 'sam'   
    // 浏览器发现div节点中的文本内容改变了，于是通知渲染引擎【慢】，照搬操作（第2次改变文本内容）
    ```

+   大量的时间花费在【中间过程】，也就是「浏览器通知渲染引擎」的过程中

    +   这就使得： div 的操作，会比其他几行单线程操作，都慢很多
    +   「第 2 次改变文本内容」需通知渲染引擎。所以「第 2 次改变文本内容」的操作，肯定比「第 1 次」慢
    +   [思考](# DOM 操作慢？)：执行速度变慢，这是模块化的缺点吗 ？


​	

​	

### DOM 操作慢 ❓❗️

>   网上都说 DOM 操作慢，实际上只是比 JS 操作慢，DOM 操作比网络请求还是快很多的。
>   关于这一部分内容，大家可以延伸阅读一些文章：
>
>   -   [为什么说DOM操作很慢](https://segmentfault.com/a/1190000004114594)
>   -   [为什么经过10年的努力DOM操作还是这么慢](https://stackoverflow.com/questions/6817093/but-whys-the-browser-dom-still-so-slow-after-10-years-of-effort)
>
>   注意，网上的文章说的不一定都是对的，作为参考了解一下即可。

>   「跨线程操作，使得执行速度变慢」，这是模块化的缺点吗？  答 👇

+   虽然变慢了，但是可以实现各线程内部单独的优化。

+   比如，在渲染引擎中可以单独优化渲染，不需要理会 JS 的各种变量的问题 … 因为根本就看不见它们，所以也就无需考虑
+   总结
    +   「模块化」可以让划分的每一块，都比较简单、容易优化、容易代替
    +   虽然损失了时间，但「模块化的优点」却是更显著的

	

	

	

### 插入新标签的完整过程（生命周期）

>   这个 div 经历了 3 个过程（vue 也有生命周期：之前、之时、之后）

#### 在 div1 放入页面之前

-   你对 div1 所有的操作都属于 JS 线程内的操作

#### 把 div1 放入页面之时

1.  浏览器会发现 JS 的意图
2.  就会通知渲染线程在页面中渲染 div1 对应的元素

#### 把 div1 放入页面之后

>   为什么要说「可能会 ~，也可能不会」这种看似无意义的话
>
>   +   因为不同的浏览器，有不同的逻辑
>   +   以下 4 点均以 Chrome 为例

1.   你对 div1 的操作都**有可能**会触发重新渲染

2.   `div1.id='newId'` 可能会重新渲染，也可能不会

     -   比如，改的这个 id 有 css 样式，那就会触发重新渲染

3.   `div1.title = 'new'` ，即使改 title**[也可能会重新渲染](https://css-tricks.com/css-content/#article-header-id-4)**，也可能不会

     -   貌似看起来改 title 不应该影响页面。实际上 title 有时也会渲染在页面里

     -   举例 👇 

         ```html
         <div title='titleHi'></div>
         <style>
           div::after{ content: attr(title); }   /* 页面中显示出了 `titleHi` */
         </style>
         <!-- 
         	div 的伪元素内容，就是获取了 div 的 title 属性。
         	这种情况如果改了 div 的 title，页面一定会重新渲染
         -->
         ```

4.   如果你连续对 div1 多次操作，浏览器可能会**合并成一次操作**，也可能不会（**之前在动画里提到过**）

     -   需求动画效果：让 test 元素的宽度从 100 px 渐变成 200 px

     -   [代码见链接](http://js.jirengu.com/yefac/1/edit?html,css,js,output)。这样写为什么不会发生动画 ？

         -   在短时间内，对这个元素的 classList 进行了两次操作（两次添加类名）
         -   JS 认为 执行两次、渲染两次是浪费时间，何不合并、渲染一次，更节约渲染时间
         -   由于合并 ，导致动画效果出不来。

     -   怎么才能不合并、展示出动效

         -   ```js
             test.classList.add('start')
             // 中间多执行一步
             test.clientWidth  
             // 获取test的客户端宽度。看似这句代码人畜无害的，但事实并非如此
             // 因为这里要获取宽度，使得上面添加class的操作，必须立即渲染
             // 所以就不会合并操作（从而展示出动效）
             test.classList.add('end')
             ```
     
     +   在中间读取宽度，导致 JS 必须先渲染出 start，然后告诉你宽度，最后渲染 end（强行拆分）
     +   又因为中间存在 css 的过渡效果 transition，所以就会展示动画啦

>   这其实是非常高深的一点  ，很少有人能这么清晰的分析出原因

​	

​	

### 提问

```js
let div = doc.createElement('div')  
div.textContent = 'hi'          // 不触发重新渲染
document.body.appendChild(div)  // div插入页面之时
...
div.textContent = 'sam'   // 在div插入页面之后，修改 div 的文本内容，一定会触发重新渲染
```

>   在 div 插入页面之后
>
>   +   修改 div 的文本内容，一定会触发重新渲染
>   +   **那是否「修改 div 的所有属性，都会触发重新渲染」呢？**

[示例代码](http://js.jirengu.com/meviw/2/edit?html,js,output)

+   html 中，div 元素有三个不同的属性：`id`、`x`、`data-x`，属性值都是 `test`
+   JS 中，获取到这个 div 元素为 div1，再分别修改这三个属性的值为 `frank`
+   那最终页面中的这个 div 元素，是否会三个属性都修改成功了呢 ？
    +   `id` 修改成功、`x` 修改失败、`data-x` 修改成功

>   可修改的属性，存在什么规律吗？

+   如果这个属性是在「标准属性」中、或在「data 属性」中
+   那么浏览器会自动同步这个修改结果到渲染的页面中
+   id 属于标准属性、data-x 属于 data 属性，
    而 x 属于非标准属性，修改它就不会影响（渲染）到页面

>   总结「属性同步」的标准，[见下](# 属性同步)

​	

​	

### 属性同步

#### 标准属性

-   对 div1 的标准属性的修改，会被浏览器自动同步到页面中
-   比如 id、className、title 等（你改了就会直接变）

#### data-* 属性

-   同上

#### 非标准属性

-   对非标准属性的修改，则只会停留在 JS 线程中
-   不会同步到页面里
-   比如 x 属性，[示例代码](http://js.jirengu.com/meviw/2/edit?html,js,output)

#### 启示

+   如果你想自定义属性，又想被同步到页面中，请使用 data-  作为前缀（data 属性）
+   不要使用类似 `x` 这种属性

	

#### 图示

![image-20201022020126565](https://i.loli.net/2020/10/22/jld7aQhNqkOUfsG.png)

+   div 的【标准属性】，自动同步
+   div 的【data 属性】，自动同步
    +   【data 属性】中的【x 属性】，自动同步
+   div 的 x 属性，什么东西，滚

>   补充：
>
>   +   JS 线程中的属性叫「properties」，渲染线程中的属性叫「attributes」 （[对比](# Property  vs  Attribute)）
>   +   所以 [Element.getAttribute()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getAttribute) 这个 API 获取的是页面中的属性

​	

​	

### Property  vs  Attribute

#### property 属性

+   JS 线程中 div1 的所有属性，叫做 div1 的 property

    ```js
    div1.style
    div1.id
    div1.className
    ```

#### attribute 也是属性

+   渲染引擎中 div1 对应标签的属性，叫做 attribute

    ```html
    <div id="test" class="test" data-x="test"></div>
    ```


​	

#### 区别

-   大部分时候，同名的 property 和 attribute ，值相等
-   但如果**不是标准属性**，那么它俩只会在一开始时相等
    +   非标准属性 x，一开始左右相等。后来 JS 线程中修改了 x 的值，但是渲染线程并不知道，导致不等
-   但注意 **attribute 只支持字符串**
    -   页面中的标签属性的值，只能是字符串，`<div id=1> </div>` 中的 id=1 也只是省略了引号的字符串 1
-   而 property 支持字符串、布尔等类型


