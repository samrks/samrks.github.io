# JS 的基本语法⚙️




内容包括「什么是表达式和语句」「标识符的规则」「 if else 语句」「 while for 语句」「 label 」<!--more-->



## 推荐书籍

1.  适合入门《[网道 JavaScript 教程](https://wangdoc.com/javascript/)》
2.  适合进阶《[你不知道的 JavaScript（上卷）](https://book.douban.com/subject/26351021/)》





## JS 语法 ⭐️

>   开始学习

### 表达式与语句

表达式

-   1+2 表达式的值为 3
-   add(1,2) 表达式的值为函数的**返回值**
-   console.log 表达式的值为函数本身（因为没加括号）
-   console.log(3) 表达式的值为多少？（**面试**)
    -   表达式的值就是函数的返回值 ，log函数的返回值为 undefined
    -   所以 console.log(3) 表达式的值为 undefined

语句

-   var a=1 是一个语句

二者的区别

-   表达式一般都有值，语句可能有也可能没有
-   语句一般会改变环境（声明、赋值）
-   上面两句话并不是绝对的



### 大小写敏感

不要写错

-   var a 和 var A 是不同的
-   object 和 Object 是不同的
-   function 和 Function 是不同的
-   具体含义后面说



### 空格

大部分空格没有实际意义

-   `var    a    =    1` 和 `var a=1` 没有区别

-   加回车，大部分时候也不影响

-   只有一个地方不能加回车，那就是 return 后面

    -   return 后面不加东西，js 会自动补充成  return undefined

        ```js
        function fn(){
          return 3
        }
        function fn(){
          return [undefined]    // return 和 3 之间加了一个回车，js会自动补充为 return undefined
          3
        }
        ```

        ![image-20200821163326585](https://i.loli.net/2020/08/21/ZUnch4s5GofivtQ.png)  ![image-20200821163621293](https://i.loli.net/2020/08/21/6TVHjPuytI5nDJp.png)

    +   唯独 return 后面不能加回车，其他多离谱的回车都没问题 





### 标识符

#### 规则

+   第一个字符，可以是 `Unicode 字母` /  `$` / `_`  /  `中文`  

+   后面的字符，除了上面所说，还可以有数字

+   注意，首位不能是数字

    ![image-20200821164512411](https://i.loli.net/2020/08/21/2J5IFxVmzkYOHer.png)

    ```js
    var 9$
    Uncaught SyntaxError: Invalid or unexpected token // SyntaxError意为语法错误，token理解成字符串
    ```

    

#### 变量名是标识符

以下都是合法的标识符用法

-   `var _ = 1 `
-   `var $ = 2 `
-   `var ______= 6 `
-   `var 你好 = 'hi'`
-   其他标识符用到再说



### 注释

>    谣言：“ 写代码要多写注释。”
>
>    注释，分为：好的注释、不好的注释

#### 不好的注释

>   不好的注释：告诉别人我写了什么

1.  把代码翻译成中文

    可能埋没了重要的注释。有用的信息和噪音的比例（信噪比）要低

     ![image-20200821165356032](https://i.loli.net/2020/08/21/QblzWd2YtuvgG8c.png)

2.  过时的注释

3.  发泄不满的注释



#### 好的注释

>   好的注释：告诉别人为什么我要这么写

1.  踩坑注解

2.  为什么代码会写得这么奇怪，遇到什么 bug
    +   遇到某个 bug，代码非得这么奇怪的写，才能避开这个 bug（这种也需要注释出来）



### 区块 block

+   把代码包在一起 

    ```js
    {
      let a=1
      let b=2
    }
    ```

+   常常与 if / for / while 合用





## if 语句

>   如果 … 那么 …

### 语法

```js
if(表达式){语句1}else{语句2}
```

+   { } 在语句只有一句的时候可以省略，不建议这样做

### 变态情况

-   (表达式) 里可以非常变态，如 a=1

    ```js
    const a=2
    if(a=1){
      console.log('a是1')   // 最终打印这句，因为 = 是赋值，=== 才是判断是否相等
    }else{
      console.1og('a不是1')
    }
    ```

-   语句1 里可以非常变态，如嵌套的 if else

-   语句2 里可以非常变态，如嵌套的 if else

-   缩进也可以很变态，如面试题常常下套

    ```js
    const a=1
    if(a===2)
      console.log('a')
    	console.log('a等于2')
    ```

    上述代码执行结果为：<img src="https://i.loli.net/2020/08/21/Gn7tHCrXjNx6PvD.png" alt="image-20200821174405618" style="zoom:80%;" />

    因为不写 { } 时，只默认**第一个语句**是跟随 if 条件的，有一个**无形的 { } **括住了第一句，相当于下面效果

    ```js
    const a=1
    if(a===2) {
      console.log('a') }
    console.log('a等于2')
    ```

    注：**js 中是没有【行】的概念**，即使两句写在一行，结果不变，仍是只括住**第一个语句**

    ```js
    const a=1
    if(a===2) {
      console.log('a'); } console.log('a等于2')
    ```

    如果用 逗号分隔呢？

    ```js
    const a=1
    if(a===2) {
      console.log('a'), console.log('a等于2') }
      console.log('a???')
    ```

    <img src="https://i.loli.net/2020/08/21/ENOkbro9eC2lsdP.png" alt="image-20200821175441188" style="zoom: 67%;" />

    +   逗号，表示这句话没结束。所以默认两个 console 就变成一个语句了（效果如上）
    +   分号；表示这句话结束了。





### 使用最没有歧义的写法

#### 最推荐使用的写法

>   永远不要省略花括号 { }，即使只有一个语句。这是最不会产生歧义的写法

```js
if(表达式){
	语句
}else if(表达式){
  语句
}else{
  语句
}
```



#### 次推荐使用的写法

>   该写法，只在函数里有用
>
>   基于函数 return 的特点：执行 return ，函数就结束了，不再往下执行

```js
function fn(){
  if(表达式){
    return 表达式
  }
  if(表达式){
    return 表达式
  }
  return 表达式
}
```



## switch 语句

>   if…else…升级版
>
>   JS 的 switch 语句设计的不算精致，但确实在某些情况时，比 if…else… 好用

### 语法 

```js
switch(fruit){ 
  case "banana"：
    //...
    break； 
  case "apple":
    //...
    break； 
  default:
    //...
}
```

### break 

1.   **大部分时候，省略 break 你就完蛋了 **

     -   当条件符合 banana 时，执行某些语句 …  ，遇到 break 跳出 switch
     -   如果没有 break，就会向下跳过`case 'apple'`，直接执行 apple 的语句 …
     -   看起来很鸡肋

2.   **少部分时候，可以利用 break**

     -   最早为什么这么设计 break 呢？

     -   是为了方便「 满足多个条件（case）时 」的判断

         -   如下，case 1 可以默认贯穿到 case 3  （初衷是好的）

         ​       <img src="https://i.loli.net/2020/08/21/IgaF7rzPAYlwXi3.png" alt="image-20200821182541640" style="zoom: 80%;" />



3.   [Swift](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html) 的 switch case 语句设计，完爆 JS 的 switch case  （apple 出的语言）

     -   执行完一个 case，默认跳出，无需 break
     -   多条件判断：可以直接在一个 case 下写多个条件句 ，无需去掉 break 来表示贯穿
     -   综上，JS 出现的太早了，没有抄到一个好的 switch case

     ```js
     switch some value to consider {
     case value 1:
         respond to value 1
     case value 2,
          value 3:
         respond to value 2 or 3
     default:
         otherwise, do something else
     }
     ```

     <img src="https://i.loli.net/2020/08/21/xYPbjI2RZyqh4ct.png" alt="image-20200821183018680" style="zoom:50%;" />



## 问号冒号（三元）表达式

>   **`表达式1 ? 表达式2 : 表达式3 `**    表达式
>
>   最简单的 if…else… 的写法，能用问号冒号就不用 if…else…

```js
// 求两个数的最大值
function max(a,b){
  if(a>b) return a;
  else return b;
}

function max(a,b){
  return a>b ? a: b
}

// 求绝对值
function abs(n){
  return n>0 ? n: -n
}
```



## && 短路逻辑

与运算

>   -   A && B && C && D 
>       -   取第一个假值，后面就不看了
>       -   如果ABC都为真，就取 D。
>   -   一般整句表达式的值，不会出现取 true / false，而是取 ABCD 中的一个
>   -   只要整个表达式中，有一个是假，整个式子就是假


```js
a&&b
if(a){  // 理解：a&&b 等价于，如果a是真，就执行b，否则就什么都不执行
  b
}else{ 
  
}
```

例

```js
3>2 && 2>1 && 4>5 && 5
false
3>2 && 2>1 && 4>2 && 5
5
3>2 && 2>1 && 4>2 && console.log('前面全对了')
前面全对了
3>2 && 2>1 && 4>2 && 5<0
false
```

例

```js
if(window.f1){
  console.log('f1存在')
}
// 上下效果等价：如果 window.f1为 true ，就会执行 conosole.log('f1存在')
window.f1 && conosole.log('f1存在')
```

>   前端中，如果能写成 && 的语句，就不写 if…else… 语句

### 最常见的用法

```js
fn && fn()  // 如果 fn 存在就执行 fn
```

例

```js
console && console.log && console.log('hi')  
// 如果console存在，就判断console.log是否存在，也存在，就执行打印 hi
/* 这么写有什么意义？
   因为 IE 没有 console，如果用 IE 就不执行 console，不然 IE 会报错 */
```





## || 短路逻辑

或运算

>   -   A || B || C || D 
>
>       -   取第一个真值，后面就都不看了。
>
>       -   如果ABC都为假，就取 D
>
>   -   一般整句表达式的值，不会出现取 true / false，而是取 ABCD 中的一个
>
>   -   只要整个表达式中，有一个是真，整个式子就是 真

```js
a||b
if(!a){  // 理解：a||b 等价于，如果a不是真，就执行b，否则就什么都不执行
  b
}else{
  
}
```

>   如果能写成 || 的语句，就不写 if…else… 

### 最常见的用法

例：前端中经常有一个种写法

```js
a = a || 100 

// 理解：如果a存在就什么都不做，否则 a=100
if(a){
  a = a  // 自己赋给自己，相当于什么都不做
}else{
  a = 100  // 保底值
}
```



## 总结：条件语句

-   if … else…  
    -   if…else…的逻辑是最常用的，但是很多情况并不使用它。代码简短时，会用更简便的代码替代这种写法
-   switch case
    -   不能少了 break
-   A ? B : C
    -   相当常用（A若为真，执行B，A若为假，执行C）
-   A && B
    -   相当常用，举例：fn && fn()    （ fn 存在就执行 fn ）
-   A || B
    -   相当常用，举例： A = A || B    （A 存在时就什么都不执行，A 不存在时 B 就是 A 的保底值）







## while 循环

>   用的不多

### 语法

```js
while (表达式) { 语句 }
```

-   判断表达式的真假
-   当表达式为真，执行语句，执行完再判断表达式的真假
-   直到遇到表达式为假，跳出循环，执行后面的语句

```js
var a = 1  // 初始化
while(a !== 10){   // 判断条件
  console.log(a)   // 循环体
  a = a+1  // 增长(为最终能跳出循环)
}
```

+   共需要四部分： 1. 初始化  2. 判断条件  3. 循环体  4. 增长



### 其他

+   do ... while 用得不多，自行了解



### while 变态情况：死循环

```js
while(a!==1){
  console.log(a)
  a = a + 0.1
}
```

原因：浮点数不精确，导致死循环

![image-20200824155208349](https://i.loli.net/2020/08/24/h7xMZLjD18vWcVS.png)





## for 循环

>   for循环 是 while循环 的语法糖

### 语法糖

-   for 是 while 循环的方便写法
    -   写 while 时需要四部分： 1. 初始化  2. 判断条件  3. 循环体  4. 增长
    -   for 将 while 的写法升级、整合，如下
        1.  语句1 是用来初始化的
        2.  表达式2  是判断条件
        3.  语句3 是自增
        4.  循环体

### 语法

```js
for(语句1;表达式2;语句3){
  循环体
}
```

1.  先执行语句1
2.  然后判断表达式2
    -   如果为真，执行循环体，然后再执行语句3
    -   如果为假，直接退出循环，执行后面的语句

例

```js
for(var i=0;i<5;i++){
  console.log(i)   // 0 1 2 3 4
}
// i 5
```

### 变态

```js
for(var i=0;i<5;i++){
  setTimeout(()=>{
    console.log(i) // 5 5 5 5 5
    // console.log(i + '随机数' + Math.random())  
  },0)
}
// i 5
```

<img src="https://i.loli.net/2020/08/24/CDvzIu6UhsjXpek.png" alt="image-20200824191159605" style="zoom:67%;" />

9 是 Chrome 的 bug。

5个5是正解。

每轮循环触发 setTimeout 相当于设置一个闹钟命令：过一会再执行 setTimeout 中的语句。而过一会之后，for循环已经走完，i 变为 5。然后闹钟到点，依次执行每轮触发的 setTimeout 中的语句，也就是执行 5轮 `console.log( i )` ，就会打印 5 个 5

#### 解决

把 var 替换成 let

```js
for(let i=0;i<5;i++){
  setTimeout(()=>{
    console.log(i) // 0 1 2 3 4 
  },0)
}
```

<img src="https://i.loli.net/2020/08/24/iwFOXAm1cqtERrL.png" alt="image-20200824192321638" style="zoom:67%;" />



### 语法变形

```js
for(语句1;表达式2;语句3){
  循环体
}
```

+   省略 语句 1 ，可在 for 循环外面定义变量

    ```js
    let i = 0
    for(; i<5 ; i++){
      console.log(i)    // 0 1 2 3 4
    }
    ```

+   省略 表达式 2 ，会进入死循环

    ```js
    let i = 0
    for(; ; i++){
      console.log(i)    // 0 1 2 3 4 5 6...
    }
    ```

+   省略 语句 2 & 3 ，会进入死循环

    ```js
    let i = 0
    for(; ; ){
      console.log(i)    // 0 0 0 0 0 0 0...
    }
    ```

    

### break 和 continue

>   break 退出所有循环 
>
>   continue 退出当前一次循环

```js
for(var i=0;i<10;i++){
  if(i%2===1){ // 遇到奇数轮次，退出整个for循环
    break
  }
}
console.log(i)  // 1
```



```js
for(var i=0;i<10;i++){
  if(i%2===1){ // 遇到奇数轮次，跳过当次循环
    continue
  }else{
    console.log(i)  // 0 2 4 6 8
  }
}
```





## label 语法

>   用的很少，面试会考（概率5%）
>
>   这个知识点，除了用来做面试题之外，毫无用处

### 语法

label 语法：在任何情况下，写一个[标识符](# 标识符)，后面写一个代码块 { }（通常包含多句代码），或一句代码



#### 代码块

```js
foo: {
  console.log(1);
  break foo;   // 跳出foo这个代码块
  console.log('本行不会输出')
}
console.log(2);
```

<img src="https://i.loli.net/2020/08/24/PYvcu15AV679GTx.png" alt="image-20200824215354524" style="zoom:80%;" />

#### 一句代码

```js
foo: console.log(2)
```

<img src="https://i.loli.net/2020/08/24/LXBuJ6hgOoP2Rab.png" alt="image-20200824220315534" style="zoom: 80%;" />



### 面试

1

```js
{
  foo: 1
}
```

2

```js
foo: 1   // 表示代码块只有一行，叫做 1
console.log(2);
```

3

```js
{
  foo: 1  // 一个代码块，第一行是一个标签，标签内容是 1，没有什么实际意义
  console.log(2);
}
```

问上面是什么

答： 是一个代码块（属于 label 语法），里面有一个标签 foo，语句就是一个 1



为什么不是一个对象？	

答：如果写成下面形式，a 就是对象了

```js
var a = {
  foo: 1
}
```

如果单纯写一个代码块（如下），那就是一个 label，语句为 1

```js
{
  foo: 1
}
```


