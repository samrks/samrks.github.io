# JS 函数的基本介绍


定义函数、call / apply / bind、大师调用法、调用时机、作用域、闭包、形参、调用栈、函数提升、arguments、this、箭头函数、立即执行函数。<!--more-->

​	

## 函数是对象

+   函数怎么会是对象……看起来不一样啊

+   解释起来非常抽象，请直接接受这个结论

>   函数是一种特殊的对象



## 定义一个函数

### 1️⃣ 具名函数

```js
function 函数名(形式参数1, 形式参数2){
  语句
  return 返回值
}
```

### 2️⃣ 匿名函数

>   上面的具名函数，去掉函数名就是匿名函数
>
>   匿名函数，通常要声明一个变量进行赋值，不然函数就消失了

```js
let a = function(x, y){ return x+y }
```

也叫函数表达式

ps：等号左边是声明变量 a 并赋值。等号右边部分，才是函数表达式

### 例

```js
function fn(x,y){
  return x+y
}
------------------------------- 上面是具名函数，函数名为fn；
------------------------------- 下面是匿名函数，函数没有名字，但是声明一个变量a来容纳这个函数的地址
let a = function(x,y){
  return x+y
}
```

#### 变态

```js
let a = function fn(x,y){return x+y}
// fn(1,2)  // 报错 fn 不存在：fn is not defined    // 为什么？
a(1,2)  // 3
```

![image-20200912151053139](https://i.loli.net/2020/09/16/TuP3W7gAVeJ6oF8.png)

解：

-   如果 fn 函数式的声明是在等号右边，那么这个 fn 的作用域就只能在等号右边、这块<font color="purple">**高亮**</font>的范围
-   出了这块高亮范围，fn 就不存在
-   其他地方要用这个函数，只能用 a 来调用

​	

```js
function fn(x,y){return x+y}
fn(1,2)  // 3
```

+   如果没有左边赋值的操作，那么 fn 在哪里都可以用

​	

### 3️⃣ 箭头函数

写法

```js
let f1 = x => x*x 
let f2 = (x,y) => x+y // 多个形参，则圆括号不能省
let f3 = (x,y) => { return x+y }  // 写了return，则花括号不能省
let f4 = (x,y) => {console.log('x+y='); return x+y } // 多语句时，花括号不能省，return不能省
```

变态：函数返回值是一个对象

```js
// let f5 = (x,y) => {name:x, age: y} // JS 中 {} 被优先当做【代码块(label语法)】的起始，而不是对象
let f5 = (x,y) => ({name:x, age: y})  
```

+   函数返回值是一个对象，需要加个圆括号，表示里面是一个整体，不是一个代码块
+   可以看出 JS 这门语言每次添加新的语法时，可能都多少会有点小bug，大概是因为它要兼容以前得版本，所以有些错误它也解决不了（不是门很严谨的语言）

​		

### 4️⃣ 用构造函数

```js
// 单语句
let f = new Function('x', 'y', 'return x+y')   
// 多语句
let f = new Function('x', 'y', 'console.log(\'hi\'); return x+y')   
f(1,2) 
// hi 
// 3
```

-   基本没人用，但是能让你知道函数是谁构造的
-   所有函数都是 Function 构造出来的
-   包括 Object、Array、Function 也是 （这里都省略了 window. ）
    -   Function 本身也是由自己构造出来的（在对象原型的笔记中解释过）

​	

​	

## 函数自身 vs. 函数调用（区别）

>   `fn`    V.S.    `fn()`

### 函数自身

```js
let fn = () => console.log('hi')
fn
```

结果

-   不会有任何结果
-   因为 fn 没有执行（执行就是调用，调用就是执行）

### 函数调用

```js
let fn = () => console.log('hi')
fn()
```

结果

-   打印出 hi
-   有圆括号才是调用

### 再进一步

```js
let fn = () => console.log('hi')  // 很多人认为fn就是函数，实际上这里的fn只是保存了函数的地址
let fn2 = fn // 把fn保存的地址复制给fn2
fn2()
```

结果

-   **fn 保存了匿名函数的地址**
-   这个地址被复制给了 fn2
-   fn2() 调用了匿名函数
-   实际上 fn 和 fn2 都是对匿名函数的**引用**而已
-   真正的函数既不是 fn 也不是 fn2

​	

​	

## 函数的要素（9 个）🤩

>   9个概念需要知道，其他函数的教程，都没这里讲的细

### 每个函数都有这些东西

1.  调用时机

2.  作用域

3.  闭包

4.  形式参数

5.  返回值

6.  调用栈

7.  函数提升

8.  **arguments（除了箭头函数）**

9.  **this（除了箭头函数）**

    >   JS 的三座大山
    >
    >   1.  原型、原型链
    >   2.  **this**
    >   3.  AJAX

    >   搞懂这三座大山，vue、react、angular … 就都可以去学了。
    >
    >   但如果三个有任何一个没搞清楚，那就还是 JS 没入门的水平。你会感觉一直在 JS 的屏障/瓶颈中

​	

​	

### 1️⃣ 调用时机 🧐

>   JS 函数的执行时机 —— 时机不同，结果不同 	

#### 例1

```js
let a = 1
function fn(){
  console.log(a)
}
```

问打印出多少？答：不知，因为没有调用（函数只有被调用才会执行）

#### 例2

```js
let a = 1
function fn(){
  console.log(a)
}
fn()
```

问打印出多少？答：1

#### 例3

```js
let a = 1
function fn(){
  console.log(a)   
  // 很多人看到这，会认为是打印 1。
  // 但此时函数根本没执行过，可以完全忽略整个函数，不要提前就把 a=1 带入到函数中做记号
}
a = 2 // 到这里，a变成2了
fn()
```

问打印出多少？答：2

#### 例4

```js
let a = 1
function fn(){
  console.log(a)
}
fn()   // 看时机：函数被调用，此时a=1
a = 2
```

问打印出多少？答：1

#### 例5

```js
let a = 1
function fn(){
  setTimeout(()=>{  // setTimeout 相当于过一会、尽快（意思是当前手头事忙完，就立马执行里面的语句）
    console.log(a)
  },0)
}
fn()
a = 2
```

举个栗子：你在打游戏（运行这段js），你妈叫你去吃饭（fn()被调用） ，你说马上去（执行setTimeout）。说了马上去，意思是要先把这局游戏打完才去（继续向下执行a=2）。现在打完了(js全部走完)，可以吃饭 console.log(a)

问打印出多少？答：2

#### 例6 💡

```js
let i = 0  // 注意：let i 写在for外面，相当于 i 只声明了一个。let i（声明i）的过程不会参加到循环中
for(i = 0; i<6; i++){  
  setTimeout(()=>{  // 忙完手头事（for循环）立马执行里面的语句 console.log   // 【异步】
    console.log(i)  // 6 6 6 6 6 6 
  },0)
}
```

上面代码块打印出多少？答：不是 0、1、2、3、4、5，而是 6 个 6

变形：

```js
for(i = 0; i<6; i++){  // 相当于只有一个 i
  setTimeout(()=>{
    console.log(i)  // 6 6 6 6 6 6
  },0)
}
-------------------------------------------------
for(let i = 0; i<6; i++){   // 这样相当于在每一次循环体的代码块中各声明了一个i，一共声明了6个i
  setTimeout(()=>{
    console.log(i)  // 0 1 2 3 4 5 
  },0)
}
```

#### 例7 💡

```js
for(let i = 0; i<6; i++){ // 这样相当于在每一次循环体的代码块中各声明了一个i，一共声明了6个i
  // 每次循环都会创建一个i 进行赋值，然后留在这个空间中。6次循环创建6个新的i在各自的{}中，互不干涉
  // 细品：类似于刻舟求剑，每次刻一下，剑的位置竟随着舟的移动也发生了变化
  setTimeout(()=>{ 
    console.log(i)
  },0)
}
```

问打印出多少？答：是 0、1、2、3、4、5。

+   因为 JS 在 for 和 let 一起用的时候会加东西，每次循环会多创建一个 i（我服了 JS）

​	

#### 总结

-   setTimeout 就是尽快、等一会，但是不是现在。相当于先干完手头的，然后去做 setTimeout 里面的
-   JS 函数的「调用时机」，由于变量的值可能会发生改变，所以每次求值的时候都要想一想所有代码执行的顺序是怎样的。如果不能确定代码执行的顺序，那么最终结果可能就是不对的。

​	

​	

###  2️⃣ 作用域

>   每个函数都会默认创建一个作用域

>   作用域特别简单，就是画圈圈

#### 例1

```js
function fn(){
  let a = 1           // let声明的变量的作用域仅在当前这个 {...} 中
}
console.log(a) // 报错 a is not defined  // a不存在
```

>   问：是不是因为 fn 没执行，导致 a 不存在 ？？

#### 例2

```js
function fn(){
  let a = 1
}
fn()
console.log(a) // 即使 fn 执行了，a 还是不存在，仍旧报错
```

答：就算 fn 执行了，也访问不到作用域里面的 a

+   跟执不执行没有关系。
+   let 声明的变量的作用域非常好找。
+   在 let 声明语句的前面找到一个 `{`  ，在 let 语句后面找到一个 `}`  ，这俩半个花括号组成的作用域`{ } `，就是 let 变量的作用域

​	

#### 全局变量 vs. 局部变量

-   在**顶级作用域**声明的变量是【全局变量】
-   **挂载到 window 上的属性**是【全局变量】
-   其他都是【局部变量】

```js
function fn(){
  let a = 1     // let 声明的变量 a，仅在fn函数的{}内生效，所以是局部变量
}
let b = 2   // let声明的变量b，是在顶级作用域声明的，所以是全局变量，全局可访问
```

```js
window.c = 3  // 在window上声明变量c，c就是全局变量，全局可访问
function f1(){
  window.d = 4 
  console.log(c)
}
function f2(){
  console.log(d)
}
f1() // 3   // 函数里也可以访问到 window 上的 c
f2() // 4   // f2函数中能访问到f1函数中，声明在window 上的变量d
```

只要是挂载在window上的变量，不论在哪个作用域声明/挂载的，都是全局变量，全局可访问

>   为什么有些方法可以直接使用，因为是挂在 window 上的
>
>   例如： Object / window.Object 、parseInt / window.parseInt  …



#### 函数可嵌套

>   作用域也可嵌套 
>
>   就近原则

```js
function f1(){
  let a = 1

  function f2(){   // 在f1函数中声明了一个f2函数
    let a = 2
    console.log(a)  
  }

  console.log(a) 
  a = 3
  f2() 
}
f1()  
// 1
// 2
```

<img src="https://i.loli.net/2020/09/12/zcNhGuS7vwyOTHI.png" alt="image-20200912222917120" style="zoom:67%;" />

#### 作用域规则

如果多个作用域有同名变量 a  （如上）

-   那么查找 a 的声明时，就向上取最近的作用域
-   简称「**就近原则**」
-   查找 a（分清作用域）的过程与函数执行无关
    -   函数的作用域与函数执行无关 —— 静态作用域（又叫 词法作用域，属于编译原理的知识）
    -   函数的作用域与函数执行有关 —— 动态作用域，但 JS 里没有动态作用域，只有静态
-   但 a 的值与函数执行有关

#### 例4 ⭐️

>   看懂这个例子，作用域就没什么问题了

```js
function f1(){
  let a = 1
  function f2(){
    let a = 2
    function f3(){
      console.log(a)
    }
    a = 22
    f3()
  }
  console.log(a)
  a = 100
  f2()
}
f1()
// 1
// 22
```

>   作用域总结：==「就近原则」==

​	

​	

### 3️⃣ 闭包

>   闭包上面讲过了  ——  讲过了吗？！

#### 重看例4

```js
function f1(){
  let a = 1
  function f2(){
    ---------------------------
    let a = 2
    function f3(){
      console.log(a)  // f3里面用到了外层函数f2的变量 a ，那么 a 和 f3 就是闭包
    }
    ---------------------------
    a = 22
    f3()
  }
  console.log(a)
  a = 100
  f2()
}
f1()
```

**闭包**

-   **如果一个函数用到了外部的变量**
-   **那么这个函数加这个变量**
-   **就叫做闭包**
    -   左边的 a 和 f3 组成了闭包
    -   闭包的用途以后讲，这里先把【闭包】的形式记下来即可
    -   你也可以先搜一下

>   “ 闭包这么简单吗？怎么看到网上讲的各种花里胡哨…  ”
>
>   frank：在 JS 基础知识这方面，我很有自信，我比其他所有在网上教你的人都懂。网上教的乱七八糟的

​	

​	

### 4️⃣ 形式参数

#### 形式参数的意思就是非实际参数

```js
function add(x, y){
  return x+y
}
// 其中 x 和 y 就是【形参】，因为并不是实际的参数，x/y并【不代表任何实际的值】，仅代表参数的【顺序】

add(1,2)
// 调用 add 时，1 和 2 是实际参数【实参】，会被赋值给 x 和 y
```

>   其他 JS 教程可能会说： JS 传参时分为「值传递」和「地址传递」。
>
>   上面记法太复杂，如果你**搞懂了内存图**，就知道没那么麻烦。
>
>   +   实际上，**传参就是把 stack 里记的内容拷贝给形参**（不要区分什么值和地址，太麻烦了）

```js
function add(x, y){
  return x+y
}
add(1,2)  // 3
// x接收到的 1，和 add(1,2) 这里的1，不是同一个1，只是复制了一份给 x
```

```js
function addObject(x,y){
  return x.value + y.value
}
addObject({value:1},{value:2})  // 3
// 怎么知道调用时，赋给x的{value:1}和 addObject(x,y)中x接收到的{value:1}是不是同一个
```

```js
// 测试一下
let a = {value:1}
let b = {value:2}
function addObject(x,y){ // 执行函数不仅把value加起来，还偷偷把x的内容改掉，看看是否影响到外面定义的x
  x.name = 'xxx'
  return x.value + y.value
}
addObject(a,b)   // 3
a  // {value: 1, name: "xxx"}    // 诶! a被改了
```

>   如果搞懂内存图，就会知道，当我们在赋值时，只是把 a 存的 stack 内容，拷贝给形参
>
>   +   实际上，**传参就是把 stack 里记的内容拷贝给形参**（不要区分什么值和地址，太麻烦了）
>   +   而形参 x / y ，应该会存储在「不知道什么区」（代指任何应该出现的区）

​	

#### 形参可认为是变量声明

>   其实，形参的本质就是变量声明
>
>   「形参」并不特殊，就是个语法糖

```js
// 上面代码近似等价于下面代码
function add(){
  var x = arguments[0]   // 为什么用 var ?  答：历史原因，当时发明形参时 就只有var声明
  var y = arguments[1]
  return x+y
}
```

​	

#### 形参可多可少

>   形参只是给参数取名字而已

```js
function add(x){   // 只声明一个形参，如果传了两个参数，怎么拿到第2个形参呢？
  return x + arguments[1]
}
add(1,2)   // 3
```

>   arguments 就是所有形参组成的数组
>
>   所以我们没有必要，把形参全部声明出来。通过 arguments 就可以全部获取到

```js
function add(x,y,z){  
  return x+y   // z 怎么办？无所谓，声明着玩
}
add(3,4)   // 7
```

>   JS 代码就是这么随意，形参爱多就多，爱少就少，没有规则约束
>
>   +   后来这种特性造成一些问题，比如 TypeScript 兴起，TS 要求形参必须按照严格的类型和顺序

​	

​	

### 5️⃣ 返回值

#### 每个函数都有返回值

>   不存在没有返回值的函数

```js
function hi(){ console.log('hi') }
hi()
```

+   没写 return，会默认返回值是 undefined

```js
function hi(){ return console.log('hi') }
hi()  
// hi           // 会照常执行语句的打印效果，并不表示这是语句的返回值
// undefined    // 返回值
```

+   `return console.log('hi')` 也就是说返回值为 console.log('hi') 的值，也就是 log 函数 的返回值，因为 log 函数没有 return，所以最终的值就是 undefined

    >   return 的结果，还是比较严谨的

​	

#### 函数执行完了后才会返回

>   如果不执行，就不会有返回值
>
>   执行了，才会有返回值

​	

#### 只有函数有返回值

-   1+2 的返回值为 3  ❌ （这是我们常见的一种口误，没有 return 哪来的返回值）
-   1+2 的值为 3   ✅

​	

​	

### 6️⃣ 调用栈 ⭐️

>   [MDN：Call stack](https://developer.mozilla.org/zh-CN/docs/Glossary/Call_stack) 

>   很抽象。是函数非常重要的要素。

什么是调用栈

-   JS 引擎在调用一个函数前
-   需要把函数所在的环境 push 到一个数组里
-   这个数组叫做调用栈
-   等函数执行完了，就会把环境弹(pop)出来
-   然后 return 到之前的环境，继续执行后续代码

#### 举例

```js
console.log(1)
console.log('1+2的结果为' + add(1,2))
console.log(2)
```

![调用栈](https://i.loli.net/2020/09/16/8KGRIamcz4VHs35.jpg)

#### 调用栈的作用（抽象）

-   **计算机是健忘的**，每次进到一个函数，都要记下来等会要回到哪。把记录写到「调用栈」中
-   所以要把这个回到的地址，写到这个调用栈里面 ——**「压栈」**
    -   在进入一个函数后，还要再进入另一个函数（嵌套的），那也要把这个子函数的地址放到栈里
-   等当前子函数执行完毕，就**「弹栈」**—— 告诉计算机函数执行完要回到哪了。当前父函数执行完，再弹栈…
    +   弹栈，会立刻删除调用栈列表中压栈时存下的对应信息。

>   [MDN 调用栈](https://developer.mozilla.org/zh-CN/docs/Glossary/Call_stack)
>
>   这个模型还是比较重要的。
>
>   -   JS 每次进入一个函数之前，先存档，执行完毕，没什么问题就读档，消掉前面的档
>   -   类似玩游戏打boss之前要存档，dead了后，就能自动读档到打boss之前的游戏进度 —— 与 JS 的流程不是非常一致，就是大概意思.
>   -   主要理解上面的流程图

>   「栈」会不会满呢？
>
>   +   如果使用**递归函数**，就有可能把栈压满。
>   +   因为递归函数，可能会一直不停的在压栈。

#### 递归函数

##### 阶乘

-   当 n 不等于 1，就执行 n × f(n-1)
-   当 n 等于 1，就返回 1

<img src="https://i.loli.net/2020/09/13/oy5datf3LXH9BQj.png" alt="image-20200913170028265" style="zoom:50%;" />

```js
function f(n){
  return n !== 1 ? n* f(n-1) : 1
}
```

​	

##### 理解递归

>   层层递进↘，再层层回归↙  —— 递归

```js
f(4)
= 4 * f(3)
= 4 * (3 * f(2))
= 4 * (3 * (2 * f(1)))
= 4 * (3 * (2 * (1)))
= 4 * (3 * (2))
= 4 * (6)
24
```

先递进↘，再回归↙

+   很多教程中，说递归就是不停的调用自己，实际上是不正确的理解
+   调用自己 !== 递归。调用自己有时候会死循环的，死循环就不是递归。递归——先递进，再回归、
+   递归的尽头，就在 **`f(1) === 1`**  这个关键点。

​	

#### 递归函数的调用栈

##### 递归函数的调用栈很长

+   请画出阶乘 (4) 的调用栈

    ![递归函数的调用栈](https://i.loli.net/2020/09/16/ObmNoZXLcHCV597.jpg)

+   阶乘 4 ，会压 4 次栈

+   阶乘 10000 ，会压 10000 次栈  （数值太大，Chrome 计算不了）

    <img src="https://i.loli.net/2020/09/13/1ZSaXi7y3TMJCHj.png" alt="image-20200913174120616" style="zoom:67%;" />

+   试试「阶加」10000，压 10000 次栈

    ```js
    function sum(n){
      return n !== 1 ? n+ sum(n-1) : 1
    }
    sum(10000) // 50005000 
    sum(20000) // Maximum call stack size exceeded 【爆栈】
    ```

    <img src="https://i.loli.net/2020/09/13/Y3dtPuT49ExkfaF.png" alt="image-20200913174351155" style="zoom:67%;" />

#### 爆栈

>   如果调用栈中压入的帧过多，程序就会崩溃 —— 爆栈

```js
function sum(n){
  return n !== 1 ? n+ sum(n-1) : 1
}
sum(10000)   // 要压10000次栈 // 50005000
sum(20000)   // 要压20000次栈 // 已经报错。Maximum call stack size exceeded
```

可以用**二分法**，试试 Chrome 最多能压多少次栈

```js
sum(15000)  // 爆栈
sum(12500)  // 爆栈
sum(11431)  // 爆栈
sum(11430)  // 65328165
```

>   Chrome 的调用栈的长度，大概 11000 ~ 12000 左右，不是固定值（因为里面可能已经放了一些别的东西）

​	

#### 调用栈最长有多少

>   使用下面代码，可以测试一个浏览器的调用栈的长度

```js
function computeMaxCallStackSize() {
  try {
    return 1 + computeMaxCallStackSize();
  } catch (e) {
    // 报错说明 stack overflow 了
    return 1;
  }
}
```

-   Chrome 12578
    Firefox 26773
    Node 12536

-   Chrome 和 Node 的用的是同一个 JS 引擎，所以测出来差不多。
    Firefox 用的是自己的 JS 引擎，所以可能大一些

​	

#### 总结—————————————

>   什么是调用栈
>
>   +   就是我们进入一个函数时，要先把这个环境存下来，然后再进去，不然函数执行完就不知道怎么回去了。
>       要存的东西很多，就需要一个数组来保存。这个保存函数所在环境的数组，就叫[「调用栈」](https://developer.mozilla.org/zh-CN/docs/Glossary/Call_stack)
>
>   +   调用栈的长度大概是在一万到两万左右，超过这个值程序就会崩溃 —— 爆栈

​	

​	

​	

### 7️⃣ 函数提升

#### 什么是函数提升

>   不管你把具名函数声明在哪里，它都会跑到第一行

```js
function fn(){}
```

示例

```js
add(1,2)
function add(x,y){
  return x+y
}
// 3
```

>   有一种代码规范就是，把所有声明的函数，都集中放到最后面，这样代码阅读起来就更简洁

拓展

+   如果同时声明 变量 add 和 函数 add，那 add 到底是谁呢？

    ```js
    let add = 1
    function add(){}
    ```

    <img src="https://i.loli.net/2020/09/13/PeYVQwNxahILg1c.png" alt="image-20200913181910418" style="zoom:67%;" />

    报错：add 已经被声明了。

    输出 add，结果为 函数 add。（函数会提升，自动变成 ↓↓ 这样）

    ```js
    function add(){}
    let add = 1 
    ```

+   let 特性：如果这个变量已经存在，就不允许再次重复声明，会直接报错

+   但是用 var 声明，就不会报错

    ```js
    var add = 1
    function add(){}
    ```

    <img src="https://i.loli.net/2020/09/13/cJjEQmTByzAI2wo.png" alt="image-20200913182633832" style="zoom:67%;" />

    ```js
    var add
    function add(){}
    ```

    <img src="https://i.loli.net/2020/09/13/MzW2K5qsrlFDECx.png" alt="image-20200913184305554" style="zoom:67%;" />

+   由上，用 var 可能有很多问题，搞不清到底表示函数还是什么。（押题再讲 var）

+   如果只用 let ，那世界就清净了。因为只要 let 重复声明，就会报错，避免上述搞不清变量到底是谁的 bug

    >   学习方法：难得东西着重去学；简单的东西，可以放到面试准备阶段背
    >
    >   就比如：var 很复杂，但又没什么用，只是面试会考到。所以我们只在面试准备阶段，讲一下

​	

#### 什么不是函数提升

```js
let fn = function(){}
```

>   这是赋值，右边的匿名函数声明不会提升，你在什么时候写，它就什么时候声明

例

```js
add(1,2)   // 报错
let add = function(x,y){return x+y}   // 这个函数的声明并没有提升，导致声明前先调用，所以会报错
```

<img src="https://i.loli.net/2020/09/13/FH9Njmozp7s3eQt.png" alt="image-20200913211540977" style="zoom:67%;" />

​	

​	

### 8️⃣ arguments 

### 9️⃣ this ⚡️

>   arguments 和 this，是**除了箭头函数**，每个函数都有的。

-   箭头函数，是新出的语法，故意摒弃了这两个特性。
-   可见，新的语法并不认为这俩是好东西
-   正如 JS 之父说的：JS 的原创之处并不优秀、优秀之处并非原创
-   arguments 和 this 就是 JS 原创的，使得 JS 语法特别独特，也特别不好用

​	

## 理解 arguments

>   译为：参数

>   注意
>
>   -   arguments 是所有参数组成的伪数组
>   -   每次**调用函数**时，都会对应产生一个 arguments
>   -   我们应该尽量不对 arguments 内的元素进行修改，修改 arguments 会让代码变得令人疑惑

```js
function fn(){
  console.log(arguments)
}
fn()
fn(1)
fn(1,'a')
```

<img src="https://i.loli.net/2020/09/14/vLQEN8MD63Vmraz.png" alt="image-20200914192202937" style="zoom:67%;" />

>   发现：打印 arguments，结果类似**数组**
>
>   +   实际上， **arguments 是包含所有参数的伪数组**。
>       +   arguments 数组的原型是「根对象」——包含对象的共有属性。没有 push、shift、join … 这些数组共有方法
>       +   没有数组共有方法的数组，就是「伪数组」

>   「伪数组」怎么变真数组 ？
>
>   +   通过 `Array.from(array)` 可以把任何不是数组的东西，转换为真数组（具有数组的共有属性）

​	

​	

## 理解 this ⚡️

>   this 可以说是 JS 的 “ 千古奇案 ” —— 各种取值，眼花缭乱

### 情况一

>   如果不给任何条件，那么 this 默认指向 window（包含所有全局变量）
>
>   +   这种情况，通常用不上。因为如果要获取 window 上的某变量，直接写就行 `window.xxx` ，不需要用 this 来指代

```js
function fn(){
  console.log(this)
}
```

<img src="https://i.loli.net/2020/09/14/4w16HChlcqx2ryP.png" alt="image-20200914193603652" style="zoom:67%;" />

​	

​	

### 如何指定 this

>   目前只能用 fn.call(…)

#### 情况一

>   如果传的 this 不是对象，JS 会尽量**封装成对象**

```js
function fn(){
  console.log(this)
}
fn.call(1)          // → 对象1
fn.call(undefined)  // → window
```

<img src="https://i.loli.net/2020/09/14/ZKBRAVTGw9zSxyN.png" alt="image-20200914201002349" style="zoom:67%;" />

<img src="https://i.loli.net/2020/09/14/6RSBem3sIvJWYKo.png" alt="image-20200914202807474" style="zoom:67%;" />



#### 什么叫「封装成对象」？

```js
就是 new Number(1)   // 具有 number 的共有属性 —— 但基本没人用
```

<img src="https://i.loli.net/2020/09/14/baWCiSjqVP4xFtI.png" alt="image-20200914200721378" style="zoom:67%;" />

#### 怎么禁用这个自动封装的特性 ？

>   例：传数字 1，最终 this 就是指向【数字 1】，而不被自动封装成【对象 1】

```js
// 很简单，在声明函数的时候，【使用严格模式】，相当于告诉 JS 不要随便添加东西
function fn(){
  'use strict'  
  console.log(this)
}
fn.call(1)
fn.call(undefined)
```

<img src="https://i.loli.net/2020/09/14/lwNF9J1CheavqKM.png" alt="image-20200914202233840" style="zoom:80%;" />

<img src="https://i.loli.net/2020/09/14/zXoYCN8yHKEL1TZ.png" alt="image-20200914203201885" style="zoom:80%;" />

​	

​	

### 同时指定 this 和 arguments

-   目前可以用 fn.call(xxx, 1,2,3) 传 this 和 arguments
-   **第1个参数是 this，后面所有参数是 arguments**
-   xxx 作为 this，会被自动转化成对象（JS 的糟粕）


```js
function fn(){
  console.log(this)
  console.log(arguments)
}
fn.call(1,2,3,4) 
```

<img src="https://i.loli.net/2020/09/14/okUVGdxmJTCyKzg.png" alt="image-20200914205359679" style="zoom:67%;" />

​	

​	

###  “ this 是隐藏参数、arguments 是普通参数 ”

>   this 是参数（此结论是 frank 个人的）

>   ↑↑ 是什么意思呢 ？ —— 我们需要花很多例子，来理解这句话

>   要理解 this，先从 JS 中把 this 排除出去。就是看看不用 this，能不能达到跟 this 一样的功能 ！

### 假设没有 this

```js
let person = {
  name: 'frank',
  sayHi(){
    console.log(`你好，我叫` + person.name)  // 以前是使用 this.name，改成 person.name
  }
}
```

为什么这里可以用 person.name，应该还没完成 person 的声明吧 ？

+   因为这是一个函数，函数等会儿才会执行，等到执行时，person 不是已经完成了声明吗 ！！所以这是可以的
+   我们在 person 对象中声明一个函数的时候，在函数中可以用变量 person 得到这个对象的引用
+   在不准用 this 的前提下，这段代码是合法的，也符合我们的预期

>   我们可以用直接保存了对象地址的**变量**获取 'name'
>   我们把这种办法简称为**引用** （ 一个变量**保存了**一个对象的**地址**，就叫引用）

 

#### 问题一

>   如果先声明了这个函数，后声明对象

```js
let sayHi = function(){
  console.log(`你好，我叫` + person.name) 
  // 函数声明时怎么知道会有 person 变量呢，person 还没声明 ？
  // 虽然执行上没问题，但先后逻辑上确实略有不通之处
}
let person = {
  name: 'frank',
  'sayHi': sayHi  // 引用sayHi函数
}
person
sayHi()
```

分析

-   person 如果改名，sayHi 中引用 person 的地方也必须跟着修改，否则 sayHi 函数就挂了
-   甚至有可能有 2 个单独 JS 文件，一个放着 person，而 sayHi 函数在另一个文件里面。
    这样就显得更加奇怪：一个文件居然需要知道另一个文件中有什么变量
-   所以我们可能不是很希望 sayHi 函数里出现 person 引用
    （感觉上这个代码有点不好，但也不是那么不好，较尴尬令人不爽 🙃 ）

​	

#### 问题二 ⚡️⚡️

>   对象还好，如果用 类 class 的话，问题就更大了。

```js
class Person{
  constructor(name){
    this.name = name 
    // this 指临时的新对象，这里的 this 是 new 强制指定的。我们就不讨论了 😅
  }
  sayHi(){
    console.log(???) // 问题在这，怎么打印出name呢？声明类时，还没有new出任何实例对象，没法引用
  }
}
```

>   我们想在 sayHi 中获取到当前对象的 name，但此时根本就没有当前对象，那怎么获取 ？ 
>
>   +   我们就需要一种机制，来获取到未来的对象的 name 的引用

分析

-   这里只有类，还没创建对象，故不可能获取对象的引用
-   那么如何拿到对象的 name 属性？ 🤔

​	

#### 需要一种办法拿到未来的对象

>   复述问题：我们需要在函数中，获取一个对象的引用，但这个对象还未创建，那要怎么获取 ？

>   怎样才能获取的未来对象的引用，以便拿到对象的 name 属性？  →  怎样在不知道对象名字的情况下，拿到对象的引用 ？

#### 一种土办法，用参数（传参）

对象

```js
let person = {
  name: 'frank',
  sayHi(p){
    console.log(`你好，我叫` + p.name)
  }
}
person.sayHi(person)  
// 用参数的形式，把你要得到的对象，传给了你
// 这种方法，看起来很冗余、很挫
```

类

```js
class Person{
  constructor(name){ this.name = name }
  sayHi(p){
    console.log(`你好，我叫` + p.name)
  }
}
```

​	

#### 谁会用这种办法 —— Python

>   Python 在每一个函数中加了一个参数，并且约定这个参数就是后面创建的新对象

```python
class Person:
  def __init__(self, name): # 构造函数
    self.name = name

    def sayHi(self):
      print('Hi, I am ' + self.name)

person = Person('frank')
person.sayHi()   # Python 默认会把 sayHi 前面的 person 作为参数传到 sayHi 中，所以 self 就是这个参数
```

特点

-   每个函数都接受一个额外的 self
-   **这个 self 就是后面会创建并传进来的对象**
-   只不过 Python 会偷偷帮你传对象  
    -   person.sayHi() 等价于 person.sayHi(person)
-   **person 就被传给 self 了**（得到了一个未来的对象的引用）

>   这其实是任何语言都要解决的问题 —— 在写代码的时候，不知道后面要创建的对象叫什么

​	

#### JS 没有模仿 Python 的思路

>   JS 走了另一条路 —— 更难理解的路 —— 这就是 JS 的第二座大山 this

​	

### JS 在每个函数里加了 this ⚡️⚡️

>   JS 没有像 Python 那样加一个参数，而是发明了一个关键字 —— this
>
>   在任何一个函数里，可**用 this 获取到那个**你现在还不知道名字的**对象**

```js
let person = {
  name: 'frank',
  sayHi(隐藏的this){
    console.log(`你好，我叫` + this.name)
  }
}
person.sayHi()
----------------------------------------------------
class Person{
  constructor(name){ this.name = name }
  sayHi(p){
    console.log(`你好，我叫` + p.name)
  }
}
let person = new Person('frank')
person.sayHi()  // 隐式的写了 this = p （JS引擎擅自执行的操作）
// ↑↑ 相当于 ↓↓ 
// person.sayHi(person)  
// JS 和 Python 做了一样的处理：会自动把 person 传给 sayHi
// 然后 person 被传给 this 了（person 是个地址）
```

>   -   JS 做的第 1 件事：把 this关键字 赋予 sayHi。
>   -   JS 做的第 2 件事：把 person(地址) 传给 this。
>   -   综上，就是把 person 给了 sayHi
>
>   这样，每个函数都能用 this 获取一个未知对象（person）的引用了

>   ### **person.sayHi() 会隐式地把 person 作为 this 传给 sayHi**
>
>   （ 而不是像 Python 一样作为第 1 个参数 self ，传给 sayHi ）
>
>   **方便 sayHi 获取 person 对应的对象**

​	

### 总结

>   总结一下目前的知识

-   我们想让函数获取对象的引用
-   但是并不想通过变量名做到
-   Python 通过额外的 self 参数做到
-   JS 通过额外的 this 做到：
    +   person.sayHi() 会把 person 自动传给 sayHi，sayHi 可以通过 this 引用 person
-   其他
    -   注意 person.sayHi 和 person.sayHi() 的区别
    -   注意 person.sayHi() 的断句 (person.sayHi) ( )

​	

### 这就引出另一个问题 💡

#### 到底哪个对

```js
let person = {
  name: 'frank',
  sayHi(){  // 隐藏的this参数
    console.log(`你好，我叫` + this.name)
  }
}
// 自动隐式的把 person 传给 sayHi
person.sayHi()        // 省略传参 ？
person.sayHi(person)  // 完整传参 ？  哪种写法是对的
```

>   省略形式反而对了，完整形式反而是错的

#### JS 怎么解决这种不和谐

>   [Python](#谁会用这种办法 —— Python) 至少有明确的约定：这种 person.sayHi() 写法就会把 person 传给 sayHi 的第 1 个显式参数 self 。
>
>   那 JS 要怎么解释 this 的存在呢 ？（函数并没有显式的形参，this 完全是一个不成文的、隐性的约定） 
>
>   +   JS 提供两种调用形式

​	

### 两种调用🧐

#### 小白调用法 🚫

-   person.sayHi()
-   会自动把 person 传到函数里，作为 this



#### 大师调用法 ✅

>   使用 JS 新出的调用方法：call

-   person.sayHi.call(person)

-   需要自己手动把 person 传到函数里，作为 this （更为清晰）

    ```js
    let person = {
      name: 'frank',
      sayHi(){
        console.log(`你好，我叫` + this.name)
      }
    }
    person.sayHi.call({name:1})   // call 里传什么 this 就是什么，非常清晰
    ```

    

#### 应该学习哪种？

-   学习大师调用法，因为小白调用法你早就会了
-   从这段笔记开始，默认用大师调用法

​	

​	

## 指定 this 😈

### call 指定 this

>   call 是 JS 新出的调用方法。call 会使得所有东西变得明朗起来

```js
let person = {
  name: 'frank',
  sayHi(){
    console.log(`你好，我叫` + this.name)  // call传什么this就是什么
  }
}
person.sayHi.call({name:1})   // call 里传什么 this 就是什么，非常清晰
person.sayHi.call({name:'jack'})
-------------------------------------------------------------------------------
// 大多数情况，我们需要this就是当前对象
person.sayHi.call(person)  
person.sayHi()  // 为什么不用这种写法？因为隐藏了太多细节，只适合小白
```

<img src="https://i.loli.net/2020/09/15/FwDMEWhaqU9k5yt.png" alt="image-20200915224945178" style="zoom:67%;" />

>   ### 所有函数调用，必须强迫自己使用「大师调用法」—— call / apply

​	

#### 例1

>   有一个 add 函数，不需要用到 this，那如何使用 call 调用 ？

```js
function add(x,y){
  return x+y
}
```

##### 没有用到 this

```js
add.call(undefined, 1,2) // 3   
// call的第1个参数是指定this的，后面所有参数作为实参传递给函数对应形参
```

##### 为什么要多写一个 undefined

-   因为第一个参数要作为 this
-   但是代码里没有用 this
-   所以只能用 undefined 占位
-   其实用 null 也可以

​	

#### 例2

>   Array.prototype.forEach 这个函数就用到了 this 

```js
Array.prototype.forEach2 = function(){
  console.log(this)
}
let array = [1,2,3]
array.forEach2()   // 小白写法：脑子一懵，就不知道了this是什么了
array.forEach2.call(array)  // [1,2,3]  // 大师写法：规定了call里面的就是this，所以清晰明了的知道此时打印的this就是传进去的array数组本身
```

##### 尝试写出完整的 forEach 函数

>   forEach 功能是遍历当前数组。当前数组在哪呢？ 就是 this，this 就可以作为未来数组的引用

```js
Array.prototype.forEach2 = function(fn){  // 传一个方法fn
  for(let i=0; i<this.length; i++){
    fn(this[i], i, this)  // 对每一个元素，执行fn方法
  }
}
```

>   Tips
>
>   我们在看一个函数的代码时，不要想 this 的值是什么 ， 因为 this 的值是不确定的，没人知道
>
>   +   只有在函数被调用时（用大师法传进 this），才清晰的知道 this 是什么

##### 如何调用

```js
Array.prototype.forEach2 = function(fn){
  for(let i=0; i<this.length; i++){
    fn(this[i], i, this)
  }
}
let array = [1,2,3] 
array.forEach2.call(array, item => console.log(item) ) // 大师：显式的指定了this
array.forEach2(item => console.log(item) ) // 小白：隐式的把array作为this
```

this 是什么

-   由于大家使用 forEach2 的时候总是习惯于用 arr.forEach2
-   所以 arr 就被自动传给 forEach2 了

​	

#### this 一定是数组吗

-   不一定，比如

    ```js
    Array.prototype.forEach2 = function(fn){
      for(let i=0; i<this.length; i++){
        fn(this[i], i, this)
      }
    }
    // 我们可以指定 this 为一个对象转化的伪数组
    Array.prototype.forEach2.call({0:'a',1:'b',length:2}, item=>console.log(item) )
    ```

    <img src="https://i.loli.net/2020/09/15/EeRHQBhSFnMtDzw.png" alt="image-20200915233452747" style="zoom:67%;" />

>   所以 this 就是我们可以任意指定的参数而已。
>
>   +   使用小白写法，JS 就会猜你想要的 this 是什么，绝大部分情况都能猜对。

​	

### 总结 this 的两种使用方法

>   不论什么方式调用函数，实际上都在传递 this 。 区别在于：你【知道】传的 this 什么 或【不知道】

#### 隐式传递

```js
fn(1,2) // 等价于 fn.call(undefined, 1, 2)
obj.child.fn(1) // 等价于 obj.child.fn.call(obj.child, 1)   // 一个对象的属性上的fn函数
```

#### 显式传递

```js
fn.call(undefined, 1,2)
fn.apply(undefined, [1,2])
```

#### apply 区别

-   apply 要在后面其他参数的部分，加上中括号 [  ] 
-   apply 后面的参数要用**数组**的形式来表示 
-   只是写法形式不同，其他都和 call 是一样的

​	

​	

### 绑定 this

>   如果不确定 this 是什么，可以使用 bind **强制绑定**

#### 使用 .bind 可以让 this 不被改变

```js
function f1(p1, p2){
  console.log(this, p1, p2)
}
let f2 = f1.bind({name:'frank'}) // 那么 f2 就是 f1 绑定了 this 之后的新函数
f2() // 等价于 f1.call({name:'frank'})
// 打印结果：{name: "frank"} undefined undefined  // this是传进来的对象，p1/p2没传所以是undefined
```

<img src="https://i.loli.net/2020/09/15/84BVSHIqy5FnJt9.png" alt="image-20200915235302436" style="zoom:67%;" />

-   f2 是 f1 的 this 绑定之后的版本
-   调 f2 相当于调 f1，唯一的区别就是，f2 的 this 被绑定了，绑定成通过 bind 传递的参数

>   有什么用呢？ —— 后面学 vue / react 可能就天天遇到了

​	

#### .bind 还可以绑定其他参数

>   bind 除了可以绑定 this，其实可以**用来绑定所有参数**

```js
function f1(p1, p2){
  console.log(this, p1, p2)
}
let f3 = f1.bind({name:'frank'}, 'hi')
f3() // 等价于 f1.call({name:'frank'}, hi)  // 已经绑死了：this是这个对象，p1是'hi'
f3(3)  // 因为 this 和 p1 已经绑死了，所以这里传的 3 会作为 p2
```

<img src="https://i.loli.net/2020/09/15/cXhOtpwrH8gIJBD.png" alt="image-20200915235700244" style="zoom:67%;" />

​	

​	

## 箭头函数

>   「箭头函数」没有 arguments 和 this 。

>   上面讲 this 用了大量篇幅，因为 this 功能太复杂且隐晦。所以新版 JS 就放弃了 this

### 函数里面的 this 就是外面的 this

>   默认的 this 是 window

```js
console.log(this) // window
console.log(this === window) // true

let fn = () => console.log(this) // 这里的this无需确认，外面的this是什么，箭头函数里面的this就是什么
fn() // window
```

例

>   对于箭头函数来说，变量就是普通变量

```js
let a = 1
let fn = () => console.log(a)
fn()   // 1
```

>   函数打印变量a 。就近原则，先找函数里有没有变量a，没有。就用函数外的变量a

>   this 同理，箭头函数里面没有 this 变量，就找外层的 this 变量。所以箭头函数中的 this 就是外面的 this

​		

### 就算用 call 也无法指定 this

>   怎么证明箭头函数里面没有 this 呢？

>   可以用 call 来尝试指定箭头函数的 this 。 结果，无法指定，this 仍然指向 window

```js
let fn = () => console.log(this)
fn.call({name:'frank'}) // 仍是 window
```

>   不论是用 call 、 bind … 都无法改变箭头函数中 this 的指向，永远是和 函数外的this 保持一致。
>   除非外面的 this 改变了，否则 箭头函数的 this 不会变化



### 箭头函数没有 arguments

```js
let fn = ()=> console.log(arguments)  // arguments是所有参数组成的伪数组
fn(1,2,3)  // 报错：arguments is not defined
```



>   没有 this ，没有 arguments 的函数就是「箭头函数」





## 总结

每个函数都有这些东西

1.  **调用时机**：决定了变量的值
2.  **作用域**：同时多个作用域，遵循「就近原则」
3.  **闭包**：如果一个函数用到了外部的变量，那么这个函数加这个变量，就叫做闭包

-   **形式参数**：给参数取名字，相当于声明一个变量

5.  **返回值**：return ，默认 return undefined
6.  **调用栈**：进去每个函数前都要先压栈，出来函数前要弹栈
7.  **函数提升**：函数跑到最前面
8.  **arguments（除了箭头函数）**：包含所有参数的伪数组
9.  **this（除了箭头函数）**：引用一个当前不存在的对象，是 call() 方法的第一个参数



​	

​	

## 立即执行函数

>   只有 JS 有的变态玩意，现在用得少

### 声明局部变量（ES6 之前）

>   在 ES6 之前，怎么获得一个局部变量

例

```js
// 以前只有 var声明时
var a = 1  // a 是一个全局变量

// 如果想声明一个局部变量，必须写一个函数
function fn(){
  var a = 2   // 在函数里，声明的 a 才是局部变量
  console.log(a)  
}

fn()           // 2
console.log(a) // 1
```

>   用函数确能得到局部变量，但也同时增加了一个全局的函数（不也是全局变量），这与初衷相悖

#### 思考 / 办法

>   思考：如果函数没有名字，那就不会生成一个全局函数，然后直接调用这个没名的函数不就行了

>   办法：声明一个匿名函数，然后直接调用执行 —— 没有暴露任何一个全局变量 或 全局函数

```js
function fn(){
  var a = 2   // 获得局部变量a，但副作用是又带来一个全局函数
  console.log(a)
}
fn() // 2
----------------------------------------- ↑↑ 改写成 ↓↓ ---------------
// 去掉函数名
function(){
  var a = 2   // 获得局部变量a，但副作用是又带来一个全局函数
  console.log(a)
} () // 把圆括号加到匿名函数的后面
// 上面就是声明一个匿名函数，然后直接调用执行
```

#### 执行 / 报错

```js
function(){
  var a = 2
  console.log(a)
} () 
// Uncaught SyntaxError: Function statements require a function name
```

>   执行，报错（JS认为语法不对）

#### 解决

>   JS 的程序员绞尽脑汁，找到一些解决办法    ↓↓ 

+   匿名函数前加一个操作符

    ```js
    + function(){
      var a = 2
      console.log(a)
    } ()
    // 2
    // NaN（返回值并不影响需求，所以放着就好）
    ```

    <img src="https://i.loli.net/2020/09/16/QzT8kpw4EJ2C6bu.png" alt="image-20200916153324433" style="zoom:67%;" />

+   只要做一个运算，上面函数的写法都可以直接执行，不会再报错（与 undefined 运算，返回都是 NaN）

    ```js
    + function(){...}()   // NaN
    - function(){...}()   // NaN
    1* function(){...}()  // NaN   // 乘号必须左右都有值
    ```

+   取反也可以 **`!`** 

    返回值为 undefined  =>  ! undefined  =>  true

    ```js
    ! function(){...}()  // true
    ```

+   … 

>   这样我们终于就得到 JS 中，只要一个局部变量的方法

>   总结：为得到一个局部变量，不得不去造一个函数，并执行这个函数 —— 这也是 JS（旧） 的问题

### 总结原理

-   ES 5 时代，为了得到局部变量，必须引入一个函数
-   但是这个函数如果有名字，就得不偿失
-   于是这个函数必须是匿名函数
-   声明匿名函数，然后立即加个 () 执行它
-   但是 JS 标准认为这种语法不合法
-   所以 JS 程序员寻求各种办法
-   最终发现，只要在匿名函数前面加个运算符即可
-   !、~、()、+、- 都可以
-   但是这里面有些运算符会往上走
-   所以方方推荐永远用 ! 来解决

​	

### 声明局部变量（ES6 之后）

```js
{
  let a = 2
  console.log(a)
}
2
undefined

console.log(a)  // 报错 a is not defined
```

<img src="https://i.loli.net/2020/09/16/vqsik8e2anXctmo.png" alt="image-20200916155138292" style="zoom:67%;" />



### 注意事项

>   推荐永远用 ! 来解决

用 +、括号 … 可能有 bug

```js
console.log('hi') // 没有这句时，代码执行一切正常，返回值也是 undefined。一旦有这句话，代码执行就不同了
(function(){
  var a = 2
  console.log(a)
} ())
```

<img src="https://i.loli.net/2020/09/16/oCEeiWspb4Z18RU.png" alt="image-20200916155617412" style="zoom:67%;" />

>   JS 有个特点：它的回车是没有意义的

```js
// 上面代码执行过程，如下
console.log('hi')  =>  log函数返回值 undefined  => undefined(function(){}())
```

>   因为回车是没有意义的，等同于 undefined 后面跟着一对 **`( )`**，所以把 undefined 当成函数执行，必然报错

#### 总结

>   永远**不要**用 【圆括号】来写立即执行函数（圆括号会往上面代码凑，甚至可能连起来执行）
>
>   +   虽然可以用【分号】来强制结束/分隔两个语句 。但显然也没感叹号方便
>   +   **这是 JS 中唯一需要加分号 `;` 的地方**，其他任何代码不需要分号

```js
console.log('hi');   // 可以用 ; 分隔两个语句   // 如果别人用了圆括号，一定要在前面加 ; 分号
(function(){
  var a = 2
  console.log(a)
} ())
```


>   用【感叹号】最合适，因为感叹号不会往上面代码看，只会往后看








