# JS 对象分类——原型 & 类




「构造函数」「原型」「new 操作符」「类 class」<!--more-->

​	



## 对象需要分类吗？

>   这是一个值得思考的问题

我们来做一个小程序

-   输出各种形状的面积和周长



​	

## 一个正方形  Square

代码


```js
let square = { 
  width: 5, 
  getArea(){ 
    return this.width * this.width   // 先简单的把this理解成当前对象，在「函数篇」会重新学习this 
  }， 
  getLength(){ 
    return this.width * 4
  }
}
```

分析 

+   声明一个「正方形」对象
+   「正方形」拥有三个属性：边长、面积、周长



​	

## 一打正方形 💡

```js
let square1 = { 
  width: 5, 
  getArea(){ 
    return this.width * this.width
  }， 
  getLength(){ 
    return this.width * 4
  }
}
let square2 = { 
  width: 5, 
  getArea(){ 
    return this.width * this.width
  }， 
  getLength(){ 
    return this.width * 4
  }
}
let square3 = { ... }
...
let square12 = { ... }
```

>   写12遍。这样写代码的，要么是新人，要么是傻子。
>
>   +   这么写非常累，如果修改，需要逐个修改，非常非常麻烦

​	

### 用 for 循环实现（浪费内存）

```js
let squareList = []
for(let i=0; i<12; i++){  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
  squareList[i] = {       // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
    width: 5,
    getArea(){
      return this.width * this.width
    },
    getLength(){ 
      return this.width * 4
    }
  }
}
```

如果 width 不全是 5，怎么实现

```js
let squareList = []
let widthList = [5,6,5,6,5,6,5,6,5,6,5,6]   // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
for(let i=0; i<12; i++){
  squareList[i] = {
    width: widthList[i],  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
    getArea(){
      return this.width * this.width
    },
    getLength(){ 
      return this.width * 4
    }
  }
}
```

#### 分析

+   虽然实现了需求，但上面写法仍属于「垃圾代码」，浪费了太多内存，自己画 **内存图** 就知道了

```js
squareList[0].getArea === squareList[1].getArea   // false
```

<img src="https://i.loli.net/2020/09/15/CtYzvbgywRE1pxZ.png" alt="image-20200903161514254" style="zoom:67%;" />

<img src="https://i.loli.net/2020/09/03/CIS2vNQlDfA5KMi.png" alt="内存图-循环结束后" style="zoom:80%;" />

```js
<img src="https://i.loli.net/2020/09/03/kQiTnhXL92HNabF.jpg" alt="内存图-循环过程" />
```

>   会画内存图的人， 比其他人理解的更好
>
>   +   内存中，一共创建了 24 个函数，其中 22 个都是多余重复的

<img src="https://i.loli.net/2020/09/03/FdCh5kqPOUNRcTp.png" alt="image-20200903154452782" style="zoom:80%;" />

​	

### 借助原型 √

>   将12个正方形对象的共有属性放到原型里

代码

```js
let squareList = [] 
let widthList = [5,6,5,6,5,6,5,6,5,6,5,6] 
let squarePrototype = { 
  getArea(){ return this.width * this.width }, 
  getLength(){ return this.width * 4 }
}
for(let i=0; i<12; i++){ 
  squareList[i] = Object.create(squarePrototype) // 每一个正方形都以squarePrototype为原型，共享方法
  squareList[i].width = widthList[i]
} 
```

```js
squareList[0].getArea === squareList[1].getArea   // true
```

<img src="https://i.loli.net/2020/09/15/TJbgdDQK56LBSRG.png" alt="image-20200903161451314" style="zoom: 67%;" />

<img src="https://i.loli.net/2020/09/15/XW2KBfPS4bkH3r6.png" alt="image-20200903161609247" style="zoom:67%;" />

#### 分析

>   有人指出创建一个正方形的代码分散在两个地方很不优雅，于是我们用一个函数把这两部分联系起来

+   还是垃圾代码！创建square的代码太分散了！
+   在上面基础上，应该再把代码抽离到一个函数里，实现调用函数 就可以创建正方形 —— 封装函数



​	

### 抽离到函数（封装）⭐️

>   +   将创建正方形的代码，整合到一个 function 中
>   +   直接调用 function 即可创建出对应个数的正方形对象

这种过程就叫做「封装」

+   把细节写到一个函数里，调用函数、传参，就搞定了

```js
let squareList = []
let widthList = [5,6,5,6,5,6,5,6,5,6,5,6] 

function createSquare(width){ // 此函数叫做「构造函数」详见一下版块
  let obj = Object.create(squarePrototype)   // 以 squarePrototype 为原型创建空对象 
  obj.width = width 
  return obj 
}

let squarePrototype = { 
  getArea(){ return this.width * this.width } 
  getLength(){ return this.width * 4 } 
}
for(let i=0; i<12; i++){ 
  squareList[i] = createSquare(widthList[i])  // 这下创建 square 很简单了吧！
}
```

​	

## 构造函数 ⭐️

+   就是可以构造出对象的函数



###  函数和原型结合（进一步封装）⭐️

>   进一步封装
>
>   -   squarePrototype 原型 和 creatSquare 函数，还是分散的
>   -   能不能组合在一起？

```js
let squareList = [] 
let widthList = [5,6,5,6,5,6,5,6,5,6,5,6] 
function createSquare(width){  // 构造函数：用于创建 square 对象
  let obj = Object.create(createSquare.squarePrototype) 
  // 先使用后定义？NO，这里并未执行，执行时已经定义完了
  obj.width = width 
  return obj 
}
createSquare.squarePrototype = { // 把原型放到构造函数上，结合够紧密了吗？
  getArea(){ return this.width * this.width },
  getLength(){ return this.width * 4 }, 
  constructor: createSquare // 再把构造函数放到原型上，方便通过原型找到构造函数 
	// 原型和构造函数互相引用，非常紧密
  // 可以通过createSquare函数，找到原型squarePrototype。也可以拿到原型，方便的找到createSquare函数
} 
for(let i=0; i<12; i++){ 
  squareList[i] = createSquare(widthList[i]) 
  console.log(squareList[i].constructor) // ƒ createSquare(width){...}
  // 打印 constructor 可以知道谁构造了 squareList[0→11] 对象：你妈是谁？
}
```

函数上面也可以用「点 . 」？

+   因为函数属于对象

>   此时，代码已经没有进一步优化的空间了
>
>   +   这段代码几乎完美
>   +   为什么不固定下来，让每个JS开发者直接用呢?   
>   +   这时 JS 就有了 new 操作符 来帮我们实现

​	

### new 操作符 ⭐️

>   让我们感受JS之父的爱
>
>   +   JS 之父创建了 new 关键字，可以让我们可以再少写几行代码
>   +   [JS 的 new 到底是干什么的？](https://zhuanlan.zhihu.com/p/23987456)⚡️⚡️⚡️⚡️（必读！！！）

#### 函数和原型结合（重写）⭐️⭐️

```js
let squareList = [] 
let widthList = [5,6,5,6,5,6,5,6,5,6,5,6] 

function Square(width){  // 构造函数
  this.width = width 
}

Square.prototype.getArea = function(){ return this.width * this.width } 
Square.prototype.getLength = function(){ return this.width * 4 }
for(let i=0; i<12; i++){ 
  squareList[i] = new Square(widthList[i]) 
  console.log(squareList[i].constructor) 
}
// 多美，几乎没有一句多余的废话 
```

-   每个函数创建时，都自带有prototype属性，这是JS之父故意的 

-   每个prototype都自带有constructor属性，也是故意的

    ```js
    function f1(){}
    console.dir(f1)
    f1.prototype.constructor === f1   // true   // 函数原型上的constructor等于函数本身
    ```

    <img src="https://i.loli.net/2020/09/03/1F48wDzCrifjnUl.png" alt="image-20200903171850625" style="zoom:67%;" />




​	

#### 对比

 <img src="https://i.loli.net/2020/09/03/Iq36M8DLyQ1ZumY.png" alt="image-20200903174813922" style="zoom: 67%;" />

-   上面的代码被简化为下面的代码
-   唯一的区别是要用 new 来调用

 <img src="https://i.loli.net/2020/09/03/AS6mGux7NXocCk3.png" alt="image-20200903174825949" style="zoom:67%;" />

#### 细节

+   creatSquare  =>  Square 函数名变了
+   之前需要创建对象，让对象的原型指向拥有 getArea 和 getLength 的那个对象 。
    现在这句话不用写了，new 会帮我们实现
+   用 this 代表新的对象（this 会指向临时对象）
+   return obj 也不用写了，new 会帮我们实现（函数原本三行，压缩成一行，其他 new 会帮我们实现）

+   现在，把 getArea 和 getLength 通过「点方法」挨个添加到 prototype 上，不能直接给 prototype 赋新值，会导致丢失原本的 constructor（可以用 Object.assign 批量添加）
+   最后，声明新对象时，用 new Square(width)



​	

## 总结 ⚡️⚡️⚡️

>   [JS 的 new 到底是干什么的？](https://zhuanlan.zhihu.com/p/23987456)⚡️⚡️⚡️⚡️（必读！！！）

### new X() 自动做了四件事情

1.  自动创建空对象

2.  自动为空对象关联原型，原型地址指定为 `X.prototype`

3.  自动将空对象作为 this 关键字运行构造函数

    -   this 就是我们new构造函数创建的对象

4.  自动 return this

——这就是 JS 之父的爱

### 构造函数 X

-   X 函数本身负责给对象本身添加属性
-   `X.prototype` 对象负责保存对象的共用属性



### 原型与共有属性的关系

>   因为 JS 引擎按照「堆栈」来分配内存、存储数据
>   根据「堆栈」的规则，简单类型在「栈区 Stack」存储，复杂类型在「堆区 Heap」存储
>
>   +   X.prototype 的值是，原型的地址
>       +   因为原型是一个对象，对象是以「堆」的形式存储，所以严格来说，X.prototype的值是：原型的地址
>
>   -   这个地址，对应到计算机中的那一坨内存，才是原型本身
>   -   而原型中，有很多属性/方法：toString、valueOf … 它们就是「共有属性」（原创的词）
>   -   共有属性的集合就是原型
>
>   >   如果会画内存图，会理解的更清楚   ↓↓↓

![内存图0](https://i.loli.net/2020/09/04/zWLG8D1ytOFpdxZ.jpg)

​	

​	

## 示例

```js
function Dog(name){ 
	this.name = name  
	this.color = 'white'
	this.kind = '萨摩耶'    // this 就是我们new构造函数创建的对象
}
Dog.prototype.say = function(){ console.log('汪汪') }    // 共用函数
Dog.prototype.run = function(){ console.log('狗在跑') }

let dog1 = new Dog('小白')
```

<img src="https://i.loli.net/2020/09/03/gNSZAViqfdD4Bz3.png" alt="image-20200903180630000" style="zoom:67%;" />

```js
Dog.prototype.x = '狗'   // 共用的不一定都是函数, 也可以共用属性。

let dog2 = new Dog('小黑')
dog1.x // '狗'
dog2.x // '狗'
```

​	

​	

## 题外话：代码规范

### 大小写

-   所有构造函数（专门用于创建对象的函数）首字母大写
-   所有被构造出来的对象，首字母小写

### 词性

-   new 后面的函数（构造函数），使用名词形式。 如 `new Person()`、`new Object()`
-   普通函数，一般使用动词开头。如 `createSquare(5)`、`createElement('div')`
-   其他规则以后再说



​	

​	

## 总结一个非常重要的公式 💋

>   也是 JS 里唯一的一个公式   

很多前端对于原型的理解是通过画图，实际上是可以通过公式来表示的
只有方方的课才能看到，若愚的课也没有

### 如何确定一个对象的原型

为什么 

-   `let obj = new Object()`的原型是 `Object.prototype `
-   `let arr = new Array()`的原型是 `Array.prototype `
-   `let square = new Square()`的原型是 `Square.prototype `
-   `let fn = new Function()`的原型是 `Function.prototype`

>   可以总结出，一个对象通过 new XXX 创建出来，那么 XXX.prototype 就是这个对象的原型

因为 new 操作故意这么做的

 <img src="https://i.loli.net/2020/09/04/jH5EKDo2gtBzArS.png" alt="image-20200904181358158" style="zoom: 67%;" />

​	

### 结论

>   你是谁构造的
>   你的原型就是谁的 prototype 属性
>   对应的对象

-   很多前端会说 prototype 就是原型
-   实际上、严格来说，prototype 只是存了个地址，不是对象。
-   prototype 地址对应的那块内存、内存中所有共有属性的集合，才是原型对象本身

>   ⚡️⚡️⚡️⚡️ 原型公式 ⚡️⚡️⚡️
>
>   ```js
>   对象.__proto__ === 其构造函数.prototype
>   ```

​	

### 例 💋

```js
function X(width){
  this.width = width
}
X.prototype.getArea = function(){ return this.width * this.width }
X.prototype.getLength = function(){ return this.width * 4 }
let a = new X(5)
let b = new X(6)
```

-   构造函数的原型：`X.prototype ` 是 #309
-   构造出的对象 a 和 b 的原型 ：` a.__proto__` 和 `b.__proto__` 也是 #309

![内存图0](https://i.loli.net/2020/09/04/zWLG8D1ytOFpdxZ.jpg)

补充：#109 结构

<img src="https://i.loli.net/2020/09/04/FQnCfpEzc1XADko.png" alt="image-20200904182733916" style="zoom:75%;" />

​	

### 参考资料

[JS 中 `__proto__` 和 `prototype` 存在的意义是什么？](https://www.zhihu.com/question/56770432/answer/315342130)



​	

###  做几个题

>   来理解公式：**`对象.__proto__ === 其构造函数.prototype`**

#### 难度1

```js
let x = {}
```

请问：

1.  x的原型是什么？  Object.prototype

2.  `x.__proto__`的值是什么？  Object.prototype
3.  上面两个问题是等价的吗？ 
4.  请用内存图画出x的所有属性

答：

```js
Object.prototype  // x的原型
Object.prototype  // x.__proto__
x.__proto__ === Object.prototype   // true  二者是等价的
x.__proto__ === window.Object.prototype  // true  「window.」可省略
```

![image-20200905114825912](https://i.loli.net/2020/09/05/l1hJ6PrBRuNCeSb.png)

​	

#### 难度2

```js
function Square(width){
  this.width = width
}
let square = new Square(5)  ⚡️⚡️⚡️
```

请问：

1.  square的原型是什么？
2.  `square.__proto__`的值是什么？
3.  请用内存图画出 square 的所有属性

答：

```js
Square.prototype  // square的原型
Square.prototype  // square.__proto__   
// 1/2两个问题是等价的（带入公式理解）
```

![image-20200905200024903](https://i.loli.net/2020/09/05/vtF8cuO1C9mr2Ps.png)

​	

#### 难度3

请问：

1.  Object.prototype 是哪个函数构造出来的？
2.  Object.prototype 的原型是什么？
3.  `Object.prototype.__proto__` 值是什么?
4.  请用内存图画出上述内容

答：

1.  未知，Object.prototype 是默认就存在的，没有谁把它构造出来

2.  没有原型

3.  ```js
    Object.prototype.__proto__ === null   // true
    ```



​	

​	

## 构造函数、prototype、new

>   通过 Square 的例子，已经可以基本理解了

**构造函数**

+   用来创建对象的函数，就是构造函数（特点：首字母大写）



​	

**prototype**

+   不论构造函数、还是普通函数，每一个函数（对象）都有一个 prototype，用来存放共有属性

+    每个对象都有原型，但除了「根对象 Object.prototype」比较特殊，Object.prototype 这个对象的原型为空 null

     ```js
     function add(x,y){return x+y}
     add.prototype  // 不仅是构造函数，普通函数也有 prototype
     delete add.prototype  // false  而且删不掉，仍然存在
     ```




​	

**new**：会帮我们做四件事情（省略了很多代码）

1.  创建一个临时对象
2.  把这个对象指向一个原型
3.  把这个对象作为 this 来运行这个构造函数
4.  return this



​	

## Square 最终版（存疑）

```js
function Square(width){
  this.width = width
}
Square.prototype.getArea = function(){
  return this.width * this.width
}
Square.prototype.getLength = function(){
  return this.width * 4
}
let square = new Square(5)
square.width         // 5
square.getArea()     // 25
square.getLength()   // 20
```

为什么说存疑：因为还有一个更简化的版本，后面再讲

​	

​	

## 圆形 Circle

```js
function Circle(radius){
  this.radius = radius
}
Circle.prototype.getLength = function(){
  return this.radius * Math.PI
}
Circle.prototype.getArea = function(){
  return Math.pow(this.radius, 2) * Math.PI
}
let c1 = new Circle(10)
c1.radius        // 10
c1.getLength()   // 31.41592653589793
c1.getArea()     // 314.1592653589793
```

​	

## 长方形 Rectangle

两个参数：宽、高

```js
function Rect(width, height){
  this.width = width
  this.height = height
}
Rect.prototype.getArea = function(){
  return this.width * this.height
}
Rect.prototype.getLength = function(){
  return (this.width + this.height) * 2
}
let r1 = new Rect(4,5)
r1.width
r1.height
r1.getLength()
r1.getArea()
```

​	

## 对象需要分类吗？🧐

>   回到最初的问题

>   ### 答案是 需要分类

因为不同的对象有不同的功能，某些对象具有相同功能，某些对象具有不同功能

### 理由一

-   有很多对象拥有一样的属性和行为
-   需要把它们分为同一类
-   如 square1 和 square2
    如 圆1、圆2、圆3，都是圆
    如 长方形1、长方形2，都是长方形
    …
-   这样创建类似对象的时候就很方便
    -   直接 new 一个 Square、new Circle、new Rect … 然后传参，就能创建出相应图形的对象
    -   就不需要【 `let square1 = {…}; let square2 = {…} ` 然后把所有属性写一遍】，这样会很麻烦




​	

### 理由二

-   但是还有很多对象拥有其他的属性和行为

-   所以就需要不同的分类

-   比如 Square / Circle / Rect 就是不同的分类

-   Array / Function 也是不同的分类

-   而 **Object 创建出来的对象，是最没有特点的对象**（没有什么额外更多的功能，相对比较普通）

    ```js
    let x = {}   // 等价于  let x = new Object()
    ```

    



​	

​	

## 类型 vs. 类

>   「 类型  &  类 」有什么区别 ？

### 类型

-   类型是 JS 数据的分类，有 7 种
-   四基两空一对象
    1.  string
    2.  number
    3.  boolean
    4.  symbol
    5.  null
    6.  undefined
    7.  object

### 类

-   **类是针对于对象的分类，有无数种**
    -   Object 创建出来的对象，是最没有特点的对象
    -   只要觉得需要再创建一个分类，就再写一个 构造函数，new 出来新的分类对象
-   **常见的有 Array、Function、Date(日期)、RegExp(正则) 等**



​	

## 有特色的类 ⭐️

>   上面提到 Object 创建的的对象，是最没有特色的类
>
>   那什么是有特色的的类？举两个例子：数组对象、函数对象
>
>   （在其他语言中，数组、函数可能都不是对象，但在 JS 中，数组/函数 都属于对象）

### 数组对象

#### 定义一个数组

```js
let arr = [1,2,3] // 简写
let arr = new Array(1,2,3) // 元素为 1,2,3  // arr [1,2,3]
let arr = new Array(3)  // 长度为 3   // arr [empty×3]
```

​		

#### 数组对象的自身属性

```js
let arr = [1,2,3]
// arr的自身属性有 4 个： '0'/'1'/'2'/'length'
```

>   注意，属性名没有数字，只有字符串

+   属性名：'0'/'1'/'2'  ，都是字符串
+   灰色属性，是不能被遍历到的：如 `length`、`__proto__`

<img src="https://i.loli.net/2020/09/07/iQWrTYHl896nbUZ.png" alt="image-20200907161533992" style="zoom:67%;" />

<img src="https://i.loli.net/2020/09/07/G6iYF2bDPqjRzcd.png" alt="image-20200907171913057" style="zoom:67%;" />

#### 数组对象的共用属性

>   1.  共有属性非常多，都存储在数组对象的 `__proto__` 中

```js
'push'/'pop'/'shift'/'unshift'/'join'  ......
```

​	

>   2.  数组对象 比 普通对象，多一层 原型 

```js
let obj = {}
let arr = [1,2,3]
obj.__proto__ === Object.prototype            // true
obj.__proto__ === arr.__proto__.__proto__     // true
          
arr.__proto__.__proto__ === Object.prototype  // true
```

​	

>   3.  调用共有属性时，采用**就近原则**
>       +   在 arr 自身的原型上找到这个属性时，就不会再去 对象原型 上找

```js
arr.toString == arr.__proto__.__proto__.toString  // false
arr.toString == arr.__proto__.toString  // true
```

​	

>   4.  各个【共有属性】，用法都在 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array#)，基本与其英文原意相关
>       +   后面会有单独课程 教这些 [API](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)。所谓的 API  就是【数组有哪些函数、对象有哪些函数】

+   推  push() 方法，将一个或多个元素添加到数的末尾，并返回该数组的新长度。
+   弹 pop()方法，从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。
+   提档 shift() 方法，从数组中删除第一个元素，并返回该元素的值。
+   降档 unshift() 方法，将一个或多个元素添加到数组的开头，并返回该数组的新长度。（修改原数组）
+   联结 join() 方法，将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。
+   联结 concat() 方法，用于**合并两个或多个数组**。此方法不会更改现有数组，而是**返回一个新数组**。
+   …

```js
let arr = [1,2,3] 
arr.push(0)  // 4  返回数组的新长度
arr // [1,2,3,0]

arr.pop()  // 0   返回被删除的元素的值
arr // [1,2,3]

arr.shift()  // 1   返回被删除的元素的值
arr // [2,3]

arr.unshift(0)  //  3   返回数组的新长度
arr // [0,2,3]

arr.join('哈') //  "0哈2哈3"  直接返回字符串

let arr1 = [1,1], arr2 = [2,2], arr3 = [3,3]
arr1.concat(arr2) // [1, 1, 2, 2]
arr1.concat(arr2, arr3)  // [1, 1, 2, 2, 3, 3]

...
```

​		

​		

### 函数对象

#### 定义一个函数 

```js
function fn(x,y){return x+y}   // 声明函数 fn
let fn2 = function fn(x,y){return x+y}  // 声明函数 fn，并赋给变量fn2
let fn3 = (x,y) => x+y 
let fn4 = new Function('x','y', 'return x+y')  // 声明的是匿名函数，并将它赋给 fn4
```

```js
function fn1(x,y){return 'fn1'}   // 声明函数 fn
let fn2 = function fn(x,y){return 'fn2'}  // 声明函数 fn，并赋给变量fn2
let fn3 = (x,y) => 'fn3'  // 声明函数 fn3
let fn4 = new Function('x','y', 'return `fn4`') // 声明的是匿名函数，并将它赋给 fn4
```

<img src="https://i.loli.net/2020/09/07/7lpryjo26af3v5W.png" alt="image-20200907182639223" style="zoom:80%;" />

#### 函数对象自身属性 

```js
'name' / 'length'
```

<img src="https://i.loli.net/2020/09/07/MrbeXBgkTFwxJ6L.png" alt="image-20200907181853735" style="zoom: 80%;" /><img src="https://i.loli.net/2020/09/07/OSeBrcm9CK4LNpW.png" alt="image-20200907181918834" style="zoom:80%;" /><img src="https://i.loli.net/2020/09/07/6tfFIYshXEoMj4w.png" alt="image-20200907182002270" style="zoom:80%;" />

<img src="https://i.loli.net/2020/09/07/8N2tdsQiMKYoZjB.png" alt="image-20200907182036807" style="zoom:80%;" />

​	

#### 函数对象共用属性

>   共有属性非常多，都存储在函数对象的 `__proto__` 中

```js
'call' / 'apply' / 'bind'    这三个属性是重点
```

 后面会有单独课程介绍函数

​	

​	

## JS 终极一问：谁构造了ta

### window 是谁构造的

-   Window 

-   可以通过 constructor 属性看出构造者

    ```js
    window.constructor === Window           // true
    window.__proto__ === Window.prototype   // true
    ```

    <img src="https://i.loli.net/2020/09/07/ZxtcOHTVJXUB2GL.png" alt="image-20200907194258233" style="zoom:;" />



### window.Object 是谁构造的

-   window.Function

-   ==**因为所有函数都是 window.Function 构造的**==

    ```js
    window.Object.constructor === window.Function         // true
    window.Object.__proto__ === window.Function.prototype // true
    ```

    <img src="https://i.loli.net/2020/09/07/NYPzeA5UZQmCsRD.png" alt="image-20200907194910520"  />



### window.Function 是谁构造的

-   window.Function 

-   因为所有函数都是 window.Function 构造的

-   ```js
    window.Function.constructor === window.Function  // true
    ```

-   自己构造的自己？并不是这样，这是「上帝(浏览器)」的安排

-   浏览器构造了 Function，然后指定它的构造者是自己



## ES6 ：class 语法 💋

>   JS 构造对象目前有两种方式，一种是用【构造函数+prototype】，一种是用【class】

### prototype 是过时的 ？

>   非常遗憾，下面代码（构造函数）被某些前端认为是过时的

```js
function Square(width){
  this.width = width
}
Square.prototype.getArea = function(){
  return this.width * this.width
}
Square.prototype.getLength = function(){
  return this.width * 4
}
```

>   学习资料：[你可以不会 class，但是一定要学会 prototype](https://zhuanlan.zhihu.com/p/35279244)



### ES6 ：class 语法

>   class 是用来声明一个类，类是用来创建对象的，不讲究什么内存共用

```js
class Square{
  constructor(width){   // constructor中写对象里的属性
    this.width = width
  }
  getArea(){    // 对象里的函数
    return this.width * this.width
  }
  getLength(){  // 对象里的函数
    return this.width * 4
  }
}
```

注意：方法不能写成 `getLength: function(){ ... }`  这种形式



### class 语法引入了更多概念

```js
class Square{
  static x = 1   // static表示x属于Square，调用需采用 Square.x 的写法 <<<<<<<<<<<<<<<<<<<<<<<<<<
  width = 0      // 初始化 width 的值 // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
  constructor(width){
    this.width = width
  }
  getArea(){
    return this.width * this.width
  }
  getLength(){
    return this.width * 4
  }
  get area2(){  // 只读属性：调用时直接Square.area2就会执行，无需括号 <<<<<<<<<<<<<<<<<<<<<<<<<<<
    return this.width * this.width
  }
}
```

class 引入更多的语法，这些语法多来自 Java世界 或 c#世界（跟 JS 以前的世界是格格不入的）

​	

### 用 class 重写 Circle

```js
class Circle{
  constructor(radius){
    this.radius = radius
  }
  getArea(){
    return Math.pow(this.radius, 2) * Math.PI
  }
  getLength(){
    return this.radius * 2 * Math.PI
  }
}
let circle = new Circle(10)
circle.radius
circle.getArea()
circle.getLength()
```



### 用 class 重写 Rectangle

```js
class Rectangle{
  constructor(width, height){
    this.width = width
    this.height = height
  }
  getArea(){
    return this.width * this.height
  }
  getLength(){
    return (this.width + this.height) * 2
  }
}
let rect = neww Rectangle(4,5)
rect.width
rect.height
rect.getArea()
rect.getLenght()
```

​	

### 易混淆语法

**语法1：**

```js
class Person{
  sayHi(name){}
}
// 等价于
function Person(){}
Person.prototype.sayHi = function(name){}
```

**语法2：**

注意冒号变成了等于号

```js
class Person{
  sayHi = (name)=>{} // 注意，一般我们不在这个语法里使用普通函数，多用箭头函数
}
// 等价于
function Person(){
  this.sayHi = (name)=>{}
}
```



### 不要强求完全转换成 ES5

大部分 class 语法都可以转为 ES5 语法，但并不是 100% 能转，有些 class 语法你意思理解就行，不需要强行转换为 ES5。



## 原型好，还是类好？

>   都是用来给对象分类的

目前，先推荐用 class

+   但是 class 的语法知识比较复杂，还需要再多花点时间学习
    （关于类和对象的新语法有 [页面1](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，[页面2](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Object_initializer#ECMAScript_6%E6%96%B0%E6%A0%87%E8%AE%B0) 和 [页面3](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)）

+   原型的知识，上面👆已经全部讲过了



