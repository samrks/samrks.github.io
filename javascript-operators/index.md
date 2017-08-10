# JS 运算符 


<!--more-->

​	

## 前言

>   [JavaScript 网道：运算符](https://wangdoc.com/javascript/operators/index.html)

​	

## 算数运算符

>   数学运算

### number 运算

#### 加减乘除

```js
1+1    // 2
9-4    // 5
5*3    // 15
9/4    // 2.25
9/0    // Infinity  正无穷大
-9/-0  // Infinity  正无穷大
9/-0   // -Infinity  负无穷大
-9/0   // -Infinity  负无穷大
```

​	

#### 余数 x % 7

```js
10 % 7    // 3
 7 % 7    // 0   表示没有余数
-1 % 7    // -1  把负号提出来，用1%7=>1，然后合起两部分 => -1
 1 % -7   // 1
```

>   数学逻辑原理上：
>
>   +   两个数相隔 x ，再分别和 x 取余，其结果应该是相等的

```js
// 10 和 17 相隔 7，分别和 7 取余，结果应该是等价的
10 % 7 === 17 % 7    // true
-1 % 7 ===  6 % 7    // false  // JS 中负数取余有BUG
```

##### 变态面试题：负数取余

>   JS 犯错了，在 JS 中
>
>   +   如果是**负数取余** -x % y ，执行的逻辑是，先把负号提出来，两个正数取余，其结果再加上负号

```js
-x % y    =>  -(x % y)
-x % -y   =>  -(x % y)
 x % -y   =>  x % y
```

```js
-1 % 7    // -1
-7 % 8    // -7
-4 % 3    // -1
-4 % 2    // -0
-4 % 8    // -4
----------------------------------
-1 % -7   // -1
-7 % -8   // -7
-4 % -3   // -1
-4 % -2   // -0
-4 % -8   // -4
----------------------------------
 1 % -7   // 1
 7 % -8   // 7
 4 % -3   // 1
 4 % -2   // 0
 4 % -8   // 4
```

​	

#### 指数 x ** 3

>   x  的 3次方

>   这是最新的语法，但是 IE6 不支持（谁管它~🙄）

```js
7 ** 2    // 49
8 ** 2    // 64
2 ** 3    // 8
```

​	

#### 自增自减 👿 x++ / ++x / x-- / --x 

>   面试常考

```js
var a = 1
a++  // 1 --- 表达式的值
a    // 2
a--  // 2 --- 表达式的值
a    // 1
```

```js
var a = 1
++a  // 2 --- 表达式的值
a    // 2
--a  // 1 --- 表达式的值
a    // 1
```

>   规则：
>
>   +   a 在前，表达式的值就是 ++ 前的值
>   +   a 在后，表达式的值就是 ++ 后的值
>   +   a 在前，表达式的值就是 -- 前的值
>   +   a 在后，表达式的值就是 -- 后的值

```js
var a = 10
var b = a++   // 先赋值，后自增
b  // 10
a  // 11
```

​	

#### 求值运算符 +x

>   就是单纯的获取一下值，不对值做任何改变

```js
var a = 8
+a   // 8
----------------------
var a = -8
+a   // -8
```

​	

#### 负数运算符 -x

>   求负运算：正数变负数、负数变正数

```js
var a = 8
-a   // -8
----------------------
var a = -8
-a   // 8
```

​	

​	

### string 运算

>   字符串仅支持一种符号运算 ： +    （效果是连接两个字符串）

#### 连接运算  +

```js
'123' + '456'    // "123456"
```

>   字符串的 相减、相乘、相除，都会转换成 number 运算。但本质上是错误的计算

```js
'2' - '1'   // 1
```

​	

#### 变态

```js
 1  + '2'    // "12"
"1" + "2"    // "12"
```

>   本质上：一个数字为什么要和一个字符串相加  ？！
>
>   +   这从类型上讲就不对。其他语言都不允许不同类型进行相加，会报错

>   JS 中
>
>   +   **数字与字符串相加**：JS 默认先把「数字」转为「字符串」，然后拼接字符串
>
>   +   **数字与字符串相减**：因为字符串不支持减号，JS 默认先把「字符串」转为「数字」，然后进行减法运算

```js
 1  + '2'    // "12"
"5" -  5     // 0
```

​	

​	

### 尽量少用自增自减

>   因为容易你和别人都记错
>
>   +   总有人会分不清  a++ / ++a ，所以你要兼容处理这种可能的情况，想办法避免
>
>   +   解决办法，就是不用自增自减

```js
a++  =>   a += 1
```

#### 什么情况用 a++ 呢？

>   只在 for 循环用：因为这是一种「约定俗成」的写法

```js
for(let i=0; i<10; i++){}   // 所有程序员都这么写
for(let i=0; i<10; i+=1){}  // 这么写没问题，但是看着就有点野路子、不正规
for(let i=0; i<10; ++i){}   // 还有处女座程序员喜欢这么写，不管他怎么解释，你听听就好，不必当真
```

>   可能 20 年前， ++i 更快。但是现在，无脑写 i++ 就可以了，完全没问题 🤪

​	

### 不同类型不要加起来

>   把 1 和 '2' 加起来是几个意思
>
>   苹果 + 橘子 是什么？ 苹橘？

​	

​	

​	

## 比较运算符

```js
>
<
=
<=
==   // 模糊相等
!=   // 不模糊相等
===  // 全等
!==  // 不全等
```

>   == 是 JS 的 BUG
>
>   -   [为什么推荐使用 === 不推荐 ==](https://zhuanlan.zhihu.com/p/22745278)
>   -   [假设要计算 x == y， 计算的过程很冗长，但是每个步骤都很简单](https://www.zhihu.com/question/20348948/answer/19601270)

### JS 的三位一体

<img src="https://i.loli.net/2020/09/19/ik4aWOKCDhU1TQR.png" alt="image-20200919195127102" style="zoom:67%;" />

```js
0 == []     // true
0 == '0'    // true
0 == '\t'   // true   (\t: tab)
// 上面等式，还可以理解，但是等式右边的三个却互不相等
[]  == '0'   // false
[]  == '\t'  // false
'0' == '\t'  // false
```

>   ### 忠告：
>
>   +   永远不要使用 `==`，而是使用 `===` 
>   +   `==` 的问题在于，它总是自作聪明（自动类型转换）

​	

### x == y

```js
// 令人难以理解
[] == false但不是 falsy
[] == false但{}却不是 
[[]] == false
```

#### 真值表

>   黄色块，表示两个值是真的相等

![js_equal2](https://i.loli.net/2020/09/22/YBDf7tlXqRMJPjn.png)

#### JS 的自相矛盾

>   逻辑 BUG

问：下述代码 if 判断为真吗 ？

```js
var a = []
if(a) {}   // a是true，可以通过 if ，真
```

+   只有 5 个 falsy 值是假，其他都是真

    ```js
    0  NaN  null  undefined  ""
    ```

但是

```js
var a = []
if(a) { console.log("判断为真") }else{ console.log("判断为假") }
[] == true   // false
[] == false  // true
```

打印结果：【判断为真】，但 `[ ] == true` 却为假。

```js
if(a==true) { console.log("判断为真") }else{ console.log("判断为假") }
```

打印结果：【判断为假】。

>   总结：
>
>   +   if(a)为真，if(a==true)为假。
>
>   -   a 是真的 ，但是却不等于 true
>
>   -   那 a 到底是真的假的  ？！
>
>       ——  JS 自身矛盾

>   解决办法：🤬 永远不要用 == 

​	

#### 空数组 == false、空对象 != true

>   对照 [== 真值表](# 真值表)

```js
[] == true   // false
[] == false  // true
[[]] == true   // false   // 如果空数组里有一个空元素，仍是false
[{}] == true   // false
--------------------------------------------------------------------------------
{} == true   // false      // chrome 输出要加小括号 ({}) == true 
{} == false  // false     // chrome 输出要加小括号 ({}) == false 
```

>   空对象 != false 、空对象 != true

![image-20200922194348459](https://i.loli.net/2020/09/22/9CJbfiISPNchU4D.png)

​	

### x === y

>   没有任何费解（仅两个规则）

1.  **基本类型看值是否相等 **

2.  **对象看地址是否相等 **

    ```js
    [] !== []   // 地址不同
    {} !== {}   // 地址不同
    ```

#### 真值表

![js_equal](https://i.loli.net/2020/09/22/Wepb9YxNXkusT3C.png)

#### 重点

```js
0 === []          // false
0 === 1           // false       
true === false    // false     
true === true     // true    
...
------------------------------------------
[] === []    // false  
```

-   空数组 和 空数组，不相等。

-   如果会画「内存图」就很清晰了。

    +   地址不同

    ```js
    [] /* #101 */  ===  []  /* #108 */
    false
    ```

    数组属于对象类型，只能比较地址，不能比较内容


​	

### NaN !== NaN

>   唯一特例，强行记忆一下

​	

### 总结

+   空数组模糊相等 false、空对象模糊相等 true
+   空数组不全等于空数组
+   空对象不全等于空对象
+   NaN 不全等于 NaN

​	

​	

## 布尔运算符

### 或、且、非

-   ||
-   &&
-   !   取反

### 短路逻辑

>   常用

例1. 

```js
if(console){
  if(console.log){
    console.log('hi')
  }
}
// 简化为 👇
console && console.log && console.log('hi')
```

-   IE 中就没有 console 

-   以防 console 不存在报错

-   这种编码方式，叫做**「防御性编程」**

-   最新版 JS 还新增了更简单的写法 ——「[可选链语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/%E5%8F%AF%E9%80%89%E9%93%BE)」

    ```JS
    // 简化为 👇
    console?.log?.('hi')     // 如果console存在，就获取上面的log方法，如果也存在执行、打印 'hi'
    ```

​	

例2.

```js
if(!a){
  a = 100
}else{
  a = a
}
// 简化为 👇 
a = a || 100
```

+   a 的保底值

+   如果 a 是真值，a 就不变。如果 a 是假值，a 就等于 100

+   但上述方法其实存在一点 bug ：如果 a 是空字符串呢  ?！  5个falsy值都会使得 a 为假值，赋值为100

+   于是，JS 又发 明了新的语法 —— 直接在**函数参数**上写 ✅

    ```js
    function add(n){ 
      if(!n){ n=0 }   // n = n || 0
      return n+1
    }
    // 新语法 👇
    function add(n=0){
      return n+1
    }
    add(null)       //  1
    add(undefined)  //  1
    add('')         //  "1"
    ```


​	

>   ### 尽量都是用最新的 JS 语法 ⭐️

​	

​	

## 二进制位运算符

>   这些位运算符直接处理每一个比特位（bit），所以是非常底层的运算，**好处是速度极快，缺点是很不直观，许多场合不能使用它们，否则会使代码难以理解和查错。 **
>
>   有一点需要特别注意，**位运算符只对整数起作用**，如果一个运算子不是整数，会自动转为整数后再执行。另外，虽然在 JavaScript 内部，数值都是以64位浮点数的形式储存，但是做位运算的时候，是**以32位带符号的整数**进行运算的，并且返回值也是一个32位带符号的整数。

>   注：可以使用十进制数进行运算，会先自动转化成二进制，然后再计算

### 或、与、否运算符

#### 或

>   0b（零b）开头，表示后面写的是二进制数

```js
0b11   // 3
0b01   // 1
0b10   // 2
```

>   或 `|` 
>
>   -   只有两位都为 0，结果才为 0
>   -   否则为 1

运算过程：

```js
0b1111
  ||||
0b1010  
  ||||   
0b1111   
// 垂直比较对应位置，只要上下两个二进制数中有一位显示的是 1 那么最终结果的当前位置为 1。
// 都是 0 时，才得出 0
```

例

```js
0b1111 | 0b1010    // 15  // 因为没用二进制显示结果，所以默认被转换为了10进制 （8421）
// 正确写法 👇
(0b1111 | 0b1010).toString(2)  // "1111"   JS变态之处，只能用字符串形式表示二进制数，不能用数字
```

​	

#### 与

>   与 `&` 
>
>   -   两位都为 1 才是 1 
>   -   有一位是 0 ，结果就是 0

例

```js
(0b1111 & 0b1010).toString(2)    // "1010"
```

```js
0b1111
  &&&&
0b1010  
  ||||   
0b1010
```

​	

#### 否

>   否 `~`
>
>   +   1变成0，0变成1
>   +   它的返回结果有时比较难理解，因为涉及到计算机内部的数值表示机制。
>   +   详解查看 [JavaScript 网道](https://wangdoc.com/javascript/operators/bit.html#%E4%BA%8C%E8%BF%9B%E5%88%B6%E5%90%A6%E8%BF%90%E7%AE%97%E7%AC%A6)

```js
(~0b1111).toString(2)   // "-10000"
```

​		

​		

### 异或运算符

>   异或运算（^）在两个二进制位不同时返回1，相同时返回0。
>
>   +   两位相同，则返回 0 
>   +   两位不同，则返回 1

```js
(0b1111 ^ 0b1010).toString(2)    //  0101 省略最前面的 0，结果为  =>   "101" 
```

​	

​	

### 左移、右移运算符

>   `<<` 和 `>>`
>
>   +   [左移运算符](https://wangdoc.com/javascript/operators/bit.html#%E5%B7%A6%E7%A7%BB%E8%BF%90%E7%AE%97%E7%AC%A6)（<<）表示将一个数的二进制值向左移动指定的位数，尾部补 0
>   +   [右移运算符](https://wangdoc.com/javascript/operators/bit.html#%E5%8F%B3%E7%A7%BB%E8%BF%90%E7%AE%97%E7%AC%A6)（>>）表示将一个数的二进制值向右移动指定的位数。
>       如果是正数，头部全部补0；如果是负数，头部全部补1。

```js
0b0010         // 0010（二进制） =>  2（十进制）
```

#### 左移一位 👇

```js
0b0010 >> 1    // 0001（二进制） =>  1（十进制）
```

#### 右移一位 👇

```js
0b0010 << 1    // 0100（二进制） =>  4（十进制）
```

>   注意：结果仍会自动转换为十进制数，调用 .toString(2) 可以得到二进制（会省略开头0）

```js
(0b0010 >> 1).toString(2)   // 0010 => 0001  =>  "1"  （二进制） 会省略开头0
(0b0010 << 1).toString(2)   // 0010 => 0100  =>  "100"（二进制） 会省略开头0
```

##### 右移得负数情况

>   如果不懂二进制，这块也不太好理解是怎么转换的  😶

```js
-4 >> 1    // -2
/*
// 因为-4的二进制形式为 11111111111111111111111111111100，
// 右移一位，头部补1，得到 11111111111111111111111111111110,
// 即为十进制的 -2
*/
```

​	

#### 如果有多个 1

```js
(0b0011 >> 1).toString(2)   // 0011 => 0001  =>  "1"  （二进制） 会省略开头0 （吃掉最右边的1）
(0b0011 << 1).toString(2)   // 0011 => 0110  =>  "110"（二进制） 会省略开头0
```

​	

​	

### 头部补零的右移运算符

>   `>>>`
>
>   在正数的情况下，`>>` 和 `>>>`  基本没有区别
>
>   头部补零的右移运算符（>>>）与右移运算符（>>）只有一个差别，就是一个数的二进制形式向右移动时，头部一律补零，而不考虑符号位。所以，该运算总是得到正值。对于正数，该运算的结果与右移运算符（>>）完全一致，区别主要在于负数。

```js
(0b1111 >>> 1).toString(2)   // 0111  =>  "111"（二进制） 会省略开头0
```

​	

>   上面的运算符，看过一遍然后就忘记好了，因为这辈子可能都用不上

​	

​	

## 面试题 🙄

>   平时工作很少用。只有一种情况可能遇上，就是「面试题」中

>   文章：[位运算符在JS中的妙用](https://juejin.im/post/6844903568906911752) （仅做参考）

### 使用与运算符&判断奇偶

先给出结果：

```js
// 方法1.与2取余（最方便的方法、也好理解）
7 % 2 === 1  // true 奇数

// 方法2.二进制与运算符：两位都为1才是1
任意数 & 1   //  => 0 说明是偶数
任意数 & 1   //  => 1 说明是奇数
```

#### 测试

```js
(7).toString(2) // 把其转换为2进制 =>  "111"  最后一位是 1
7 & 0b001       // 7和二进制的1进行与运算，规则是二进制中对应的两位都是1结果就是1  =>  1

(7 & 0b001).toString(2)   // 0001 => "1" 二进制
```

>   原理
>
>   +   偶数的二进制位最后一位都是 0 ，而奇数二进制位的最后一位都是 1，所以**可以根据二进制判断奇偶**

>   问题转化：**如何获取任意数的二进制数的最后一位值 **
>
>   解决办法：跟二进制的 1 进行**与运算 **
>
>   -   **与运算的规则**：两个二进制数的对应位进行两两比较，**只要有一个是 0，结果就是 0 **，只有对应位都是 1 结果才得 1
>   -   因为二进制 1 的前五位都是 0，所以任意数与二进制 1 进行与运算，必然导致最终结果的前 5 为都是 0
>   -   因为前 5 位都是 0，自然就获取到最后一位，根据最后一位的结果 0 或 1 来判断出「偶 或 奇」
>
>   >   如果最后一位是 0 ，则二进制数为 000000 简写为 0    =>   偶数
>   >   如果最后一位是 1 ，则二进制数为 000001 简写为 1    =>   奇数

例

```js
123 & 1    // 1  奇数
124 & 1    // 0  偶数
125 & 1    // 1  奇数
```

>   注：可以使用十进制数进行二进制与运算，会先自动转化成二进制，然后再计算

>   虽然**位运算 **速度极快，但是**模运算**速度也不慢
>
>   +   “模”是“Mod”的音译，模运算多应用于程序编写中。 Mod的含义为**求余 **。

​	

### 使用 ~, >>, <<, >>>, | 来取整

>   下列方法都是基于该原理：
>
>   +   位运算符**只对整数起作用，不支持小数，会自动转为整数后再执行**  （直接抹掉小数位，不是四舍五入）
>   +   正数、负数同理

```js
window.parseInt(6.83)     // 6  (JS有直接提供取整的 api 呀~~)
------------------------------------------------------------- // 面试逼得必须会的方式👇 位运算
console.log(~~ 6.83)      // 6
console.log(6.83 >> 0)    // 6
console.log(6.83 << 0)    // 6
console.log(6.83 | 0)     // 6
console.log(6.83 >>> 0)   // 6
```

​	

#### 1. 两次取反 ~~

>   注意，这是二进制的取反：把 1 变成 0，把 0 变成 1

```js
console.log(~~ 6.83) // 两次取反，相当于负负得正，还是取的原值，但把小数部分抹去了   // 6
```

​	

#### 2. 右移 0 位  >> 0

```js
console.log(6.83 >> 0)  // 右移零位，就是原地不动，但位运算符会自动取整，所以这番运算会抹掉小数  // 6
console.log(-6.83 >> 0) // -6
```

运算过程：

1.  因为是位运算，第一步先转换为整数 6.83 => 6
2.  因为 6 的二进制形式是 “0110”   =>  右移 0 位 "0110"  => 转换成10进制输出 6

>   #### 左移 0 位 << 0， 同理可得
>
>   #### 头部补零的右移  >>> 0 ，同理可得

​	

#### 3. 与 0 进行或运算 | 0

>   或运算 `|`  ：只有两位都为 0，结果才为 0，否则是 1  （位运算符自动转换为整数）

```js
console.log(6.83 | 0)  // 与0进行或运算的本质：还是不改变原二进制数的 （细品）
```

运算过程：

1.  因为是位运算，第一步先转换为整数 6.83 => 6
2.  6 的二进制数与 0 的二进制数进行或运算： "110"  |  "000"   =>   "110"

​	

###  使用 ^ 来交换 a b 的值

```js
let a=1; let b=2;
[a,b] = [b,a]   // JS 最新的语法——交换两个值
a   // 2
b   // 1
```

位运算 —— 异或 ^

+   两位相同，则返回 0 
+   两位不同，则返回 1

```js
var a = 5
var b = 8
a ^= b    // 是 a = a ^ b 的简写
b ^= a
a ^= b
console.log(a)   // 8
console.log(b)   // 5
```

<img src="https://i.loli.net/2020/09/24/PZwUpg5fA9FrMRu.png" alt="image-20200924003549265" style="zoom:50%;" />

### 总结

>   关于位运算，只要记住下面 3 个问题的解法：
>
>   +   判断奇偶
>   +   取整
>   +   交换值

>   位运算用得很少容易忘，面试之前重新看看即可

​		

​		

## 其他运算符

>   奇葩运算符，一个比一个奇葩
>
>   很多「搞不清楚为什么 JS 可以这样写的」用法，其实是程序员自己发现的

### 点运算符

>   点运算符，只能用在对象上。如果遇到其他类型，会有一个特殊逻辑 ，👇

#### 语法

+   对象.属性名 = 属性值

#### 作用

+   读取/设置 对象的属性值

    ```js
    let a = {name:'sam'}
    a.name   // "sam"  读取属性值
    a.name = "jack" // 设置属性值
    ```

#### 有个疑问

>   ##### 问：不是对象，为什么也可以用点 `.`   还能获取到属性？  `'a-b-c'.split('-')`

-   JS 有特殊逻辑，点前面不是对象，就把它封装成对象

-   ```js
    var a = 1
    a.toString()
    ```

    ![未标题-1](https://i.loli.net/2020/09/24/jxqGZ4sWrfog8Th.jpg)

    （内存图）

    1.  JS 发现 a 没有 toString，就把 a 封装成对象 a' 

    2.  然后调用 封装对象 a‘  的 toString 方法
    3.  抹掉封装对象  a' = null


​	

>   每次使用 `.` 点运算符，都会①**创**建封装对象②调**用**封装对象上的方法③抹掉封装对象**(滚) **

```js
var a = 1 
a.xxx = 'sam'  // 这一行背后过程：创建 a'，然后把 xxx 放到 a'上，值为sam，最后 a’ 被抹掉
a.xxx  // undefined   // 这一行背后过程：创建a''，获取a'‘上的xxx属性，找不到xxx，返回 undefined
```

>   **每次点运算符，都会创建一个新的对象 **，用来容纳你操作的属性信息 （翻脸就不认人🙄） 

​	

JS  封装对象

-   number 会变成 Number 对象

-   string 会变成 String 对象

-   bool 会变成 Boolean 对象

-   程序员从来不用这三种对象👇，只用简单类型

    ```js
    new Number()
    new String()
    new Boolean()
    ```

    提醒一下，「永远不要用」的有 👇

    ```js
    ==
    ++ （for循环用一下)
    new Number() 、 new String() 、 new Boolean() （number/string/布尔的构造函数）  
    ```


​	

​	

### void 运算符

#### 语法

`void 表达式或语句`

```js
void console.log('hi')
void 1
```

​	

#### 作用

-   求表达式的值，或执行语句
-   **void 的值总是为 undefined **


​	

#### 需求/用法

>   需求：a 标签不跳转，而是点击 a 就执行 f 方法

```js
// 假设👇是 f 方法
function f(){ console.log('hi') }
```

+   方法一

    return 假值可以阻止默认动作（阻止 a 标签跳转）

    ```html
    <a href="http://example.com" onclick="f(); return false;">点击</a>
    ```

+   方法二

    点击 a 执行 js 语句 （改用 void 可以炫技）

    ```html
    <a href="javascript: void(f())">文字</a>
    ```

+   方法三

    即使不用 void ，也可以实现同样效果，👇

    ```js
    <a href="javascript:;" onclick="f();">文字</a>
    ```


​	

​	

### 逗号运算符

#### 语法

`表达式1, 表达式2, ..., 表达式 n`

#### 作用

+   将表达式 n 的值作为整体的值
+   也就是将最后一个表达式 ，作为 return 的内容

#### 使用

例 1

```js
let a = (1,2,3,4,5)
```

-   那么 a 的值就是 5，奇葩吧？

例 2

>   箭头函数，不用 { } 和 return，也能写多个语句 👇 ：用 () 和 ,

```js
let f = (x) => (console.log('平方值为'), x*x)
```

-   注意上面的括号不能省

​	

​	

## 运算符的优先级

>   先算什么，后算什么

### 优先级是什么

>   优先级处理的就是，当一个语句中有多个操作，到底先算哪个

#### 不同运算符

```js
1 + 2 * 3 是 (1 + 2) * 3 还是 1 + (2 * 3)   // 小学生都知道先算乘法
! a === 1 是 (! a) === 1 还是 ! (a === 1)   // 先求a的反，再与1进行布尔运算
new Person().sayHi() 先new还是先.sayHi()
```

#### 相同运算符

```js
从左到右 a + b + c  
从右到左 a = b = c = d
```

#### 优先级就是先算什么后算什么

>   具体规则想知道吗？
>   你：想。
>   不，你不想！看看[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Table)就知道为什么。—— JS 操作符的优先级，你可能这辈子都记不下来（丧😨）

MDN 总结的非常好 

+   将所有运算符按照优先级的不同从高（20）到低（1）排列

### 学习建议

>   运算符优先级很难记全，所以**直接 放弃**，不要记
>
>   如果遇到运算符优先级的题，直接猜

​	

### 优先级汇总

####汇总表位于 MDN

-   一共有20个运算符
-   怎么记忆呢

#### 技巧

-   只记「圆括号优先级最高」
-   会用圆括号就行
-   其他一律不记

>   面试官：问为什么不知道 / 没有记 JS 的优先级？
>
>   满分回答：
>
>   1.  因为 JS 的优先级太多了，没有一个人能确保把 JS 的优先级记得很清楚
>
>   2.  就算我记清楚了，也不能确保我的同事记得清楚。所以我就记了「优先级最高的是圆括号」
>
>   3.  我会在写代码时，非常清楚的用「圆括号」表明我的优先级，这样别人也可以很好的理解我的代码
>
>   （从**为了让同事能更好的理解代码**的角度，来反驳面试官/领导）

### 例 💋

#### 不同运算符

```js
if(!a===1){}  // 先算谁呢？
if( (!a) === 1)  // 圆括号优先级最高    // 查一下MDN可知，!高于===
```

#### 相同运算符

```js
a + b + c   // 从左到右顺序
a = b = c = d   // 从右到左倒序
```

例

```js
let a,b,c,d
a = b = c = d = 2
相当于👇
a = (b = (c = (d = 2)))    // (d=2)表达式的值就是2，赋给c，c=2表达式的值是2，再赋给 b...
```

1