# 算法入门


<!--more-->	

## 前言

>   前面学习的伪代码和流程图，是用来帮助我们思考的

>   本节是特别简单的算法入门。
>   —— 对于没学过算法的人可能觉得算法有点难，但当你学完后再来回顾，也许就会有上述的同感了

>   算法比你想象中简单
>
>   「什么是算法」「算法需要满足什么条件」…  等理论，这里就不讲了 
>   直接开始写代码 —— 代码可以是 JS 也可以不是，主要是靠大脑在思考



## ⭕️小试牛刀：找 2 个数中的较小值 minOf2

>   首先应该想：如何表示两个数
>
>   +   二元组？！ 可以的
>   +   但 JS 中没有二元组，退而求其次，可以使用【长度为 2 的数组】来表示这两个数
>       如果能想到这一点，就说明已经知道【什么是数据结构】了 👆
>   +   约定用 [a,b] 表示这两个数

>   必备知识 👇

### 数据结构

-   用数组 [a,b] 表示两个数字
-   你能想到这一点，就说明你在使用数据结构

### 编程知识

-   问号冒号表达式 `?   :`     或  if … else …

>   怎么从 [a,b] 中找出较小的呢（逻辑）
>
>   +   a < b 成立，就返回 a。否则返回 b  （比较大小，并返回较小值）

### 代码

>   [minOf2 的实现]()

-   numbers 是数组。
    判断中直接获取 numbers[0] 、 numbers[1]  相当于约定数组长度只能为 2
-   虽然约定是长度为 2 的数组，但如果调用时传了长度为 1 的数组，怎么办 （暂时不考虑这种情况）

```js
let minOf2 = (numbers) => { 
  if(numbers[0] < numbers[1]){ 
    return numbers[0] 
  }else{ 
    return numbers[1]
  }
}
```

### 优化代码

```js
let minOf2 = numbers => numbers[0] < numbers[1] ? numbers[0] : numbers[1]
```

### 再优化代码

```js
let minOf2 = ([a,b]) => a < b ? a : b     // 最简洁写法
```

>   这种写法叫做**「析构赋值」**，之后的课程会反复使用（解构）

### 调用

```js
minOf2([1,2])             // 1   这是小白调用法
minOf2.call(null, [1,2])  // 1   这是高手调用法 【推荐使用】（不使用this所以赋空值null）
```

​	

>   算法，就是把解决问题的思路，用代码表示出来，不论问题多么的简单（如两个数取最小）
>
>   什么是算法，就是用代码来解决问题
>
>   只不过编程界，有很多固定的问题，只有把这些固定问题都搞清楚，才可以说是会算法。只会一点是不够的

​	

### 现成 API

#### JS内置了 Math.min

>   Math.min 的核心，实际上就是上面我们写的 minOf2 的代码（比较大小，并返回较小值）

```js
Math.min(1,2） // 1  小白调用法
Math.min.call(null, 1,2)    // 高手调用法，不需要this，所以取null
Math.min.apply(null, [1,2]) // 高手调用法，区别于call，apply方法的第二个参数是【数组】形式
```

#### 关于 Math

-   看起来 Math 像 Object 一样，都是首字母大写，难道 Math 也是构造函数 ？？

-   实际上 Math 只是一个（首字母大写的）**普通对象 **

    ```js
    Math.__proto__ === Object.prototype 
    // Math不是函数，不具有函数的共有属性。打开 Math 的原型，就是【根对象】
    ```

-   以前接触过的所有对象，都是首字母小写，甚至全局对象 window 都是首字母小写
    **JS 中只有 Math 是唯一一个首字母大写的对象  ⭕️**⚡️🚩

-   这是唯一的特例：首字母大写是构造函数

​	

​	

​	



## ⭕️举一反三：找 3 个数中的最小值 minOf3

>   思路：先求前两个数中的较小值，然后拿这个较小值和第三个数进行比较，再得出一个较小值，它就是三个数中的最小值

### 代码

>   [minOf3 的实现]()

```js
let minOf3 = ([a,b,c]) => { 
  return minOf2([minOf2([a,b]), c])   // 先求 a、b 最小值
} 
```

或者

```js
let minOf3 = ([a,b,c]) => { 
  return minOf2([a, minOf2([b,c])])   // 先求 b、c 的最小值  【推荐写法：形式上更好看😝】
}
```

注意：

-   minOf3  调用了两次 minOf2  ，这并不是「递归」
-   如果 minOf3 调用了 minOf3 ，这才是「递归」

​	

## ⭕️逐步推理：找 4 个数中的最小值 minOf4

>   [minOf4 的实现]()

```js
let minOf4 = ([a,b,c,d]) => { 
  return minOf2([a, minOf3([b,c,d])])  // 用 minOf2 求 a 和 后面三个数中的最小值
}
```

>   得出推论：任意长度数组 求最小值，都可以基于 minOf2 并最终实现

>   提问：可否把「求最小值」写作一个函数 min 呢 👇

​	

## ⭕️推广：求任意长度数组の最小值 min

>   [min 的实现]()

### 细品

>   把 minOf? 全部用 min 替换

>   思路：先依次求后面所有元素的最小值 … 最后和第一位元素进行比较，最终得出所有元素的最小值
>   （不断**拆分**第1个元素和后面所有元素）

代码👇

```js
let min = (numbers) => { 
  return min(
    [ numbers[0], min( numbers.slice(1) ) ]   // 截取数组中下标为1及1之后的所有元素，组成新数组（也就是去掉数组中下标为0的元素）
  )
}
```

这个代码会死循环不停调用自己，直到报错为止，所有需要添加一个中止条件

<img src="https://i.loli.net/2020/10/09/PrIJV7Mx5WtbFvZ.png" alt="image-20201009174407724" style="zoom:67%;" />

### 完整代码：添加中止条件

>   跳出递归的条件：当 numbers 中只有两个元素时，不再拆分，直接判断两个元素大小，返回较小值

```js
let min = (numbers) => { 
  if(numbers.length > 2){  // 停止条件
    return min( 
      [ numbers[0], min( numbers.slice(1) ) ]  // 递归（拆分出第1个元素和后面所有元素）
    )
  }else{ 
    // return numbers[0] < numbers[1] ? numbers[0] : numbers[1]  // 👈👇两种写法均可
    return Math.min.apply(null, numbers) // 必须用 apply。
    // 因为min方法接收的参数必须是一个一个的数字，不接受数组，如果用call就是把整个数组传给min。所以必须用 apply 会把数组给展开成一个一个的数字
  }
}
min([2,4,3,1])   // 代入法：获取这个数组[2,4,3,1]中的最小值
```

<img src="https://i.loli.net/2020/10/10/z5yOrmA8ovWtx74.png" alt="image-20201010172038225" style="zoom:67%;" />

这就是[递归](# 递归) 👆

 

## 递归

>   先递进，再回归

>   一定要用**代入法**来理解，只靠看是看不懂的，眼睛只会欺骗你、

### 特点

-   函数不停调用自己，每次调用的参数略有不同
-   当满足某个简单条件时，则实现一个简单的调用（获取到一个基本值）
-   然后将基本值层层代入、回归
-   算出最终结果

### 理解

1.  可以用 **代入法** 快速理解递归

    +   层层剖开

    <img src="https://i.loli.net/2020/10/10/z5yOrmA8ovWtx74.png" alt="image-20201010172038225" style="zoom: 33%;" />

2.  可以用 **调用栈 **快速理解递归

    +   什么时候 **压栈、弹栈**

    +   每次进入下一行代码，就是压栈；开始回归后，就是弹栈

        <img src="https://i.loli.net/2020/10/10/z5yOrmA8ovWtx74.png" alt="image-20201010172038225" style="zoom: 33%;" />

​	

​	

---

---

​	

​	

## 排序算法

>   将一个正整数数组，从小到大排序

>   思路：递归思路、循环思路

### 用递归实现

-   代码简单，但不易理解。推荐使用**代入法**，层层剖开理解

### 用循环实现

-   代码多，但易于理解

​	

## 选择排序

>   思路：每次选择最小的放到前面，对后面的进行排序（递归）

## 🔴长度为 2 的数组排序 sort2

### 代码

>   析构赋值

```js
let sort2 = ([a,b]) => { 
  if(a < b){ 
    return [a,b]   
    // 注意：return的数组[a,b]是新数组，与原数组[a,b]是不同的内存空间。新数组只是复制了原数组ab的值
  }else{ 
    return [b,a]
  }
}
```

+   内存图原理——复制
    +   变量是对象，就是把对象存的地址复制
    +   变量是普通值，就是把值复制

### 优化代码

```js
let sort2 = ([a,b]) => a < b ? [a,b] : [b,a]
```

​	

## 🔴长度为 3 的数组排序 sort3

### 代码

>   思路：
>
>   -   先求三个数的最小值，作为返回数组的第一项
>   -   然后后面两个数字进行 sort2 操作（长度为 2 的数组排序）

```js
let sort3 = ([a,b,c]) => { 
  return [ min([a,b,c]), sort2([???]) ]
}
```

>   但，**我们发现无法将最小值从数组里删掉 ** 来单独进行sort2其余两个数字的排序

### 改进代码

>   思路：如果知道最小数字的下标，就可以把它从数组里删掉

#### 补充：splice 方法易错点

```js
numbers.splice(index, 1)  // 直接修改原数组 numbers，从中删除下标为 index 的元素
let rest = numbers.splice(index, 1)   // splice的返回值rest是被删除的元素组成的新数组
// 注意：上面两点经常被混淆，注意区别
```

#### 代码 👇

```js
let sort3 = (numbers) => { 
  let index = minIndex(numbers)
  return [ numbers[index] ].concat( 
    sort2( numbers.splice(index, 1) )  
  )
}
```

>   上面代码可忽略，比较复杂 :）

#### 优化 👇 

```js
let sort3 = (numbers) => { 
  let index = minIndex(numbers)  // 假设有👇minIndex函数，与min相似，只不过返回的是最小值的下标
  let min = numbers[index]  // 根据最小值下标index，获取到最小值本身min
  numbers.splice(index, 1)  // 从numbers数组的index处删掉1个数字，也就是从numbers中删掉最小值
  return [min].concat( sort2(numbers) )  // 返回一个从小到大排序后的数组
}
```

-   因为我们需要返回的结果是一个数组
-   而这个数组是需要两个部分拼接：最小值、sort2返回排序后的数组
-   JS 中使用 concat 可以实现两个数组的拼接（ruby语法中可用+直接相加两个数组）
-   所以把最小值放在一个数组中，这样就可以和 sort2 返回的数组，通过 concat 方法进行拼接

（自己写写这段代码）

​	

### 获取最小值下标 minIndex

```js
let minIndex = (numbers) => {
  return numbers.indexOf( min(numbers) )   // 先用min获取最小值，再用indexOf获取数组中当前元素的下标
}
```

>   有小 bug：如果最小值有两个（相同值），indexOf 只会返回第一个值得下标（）

>   这是一个取巧的办法，以后会教更好的



### 完整代码

```js
// min 函数：获取任意数组中最小值
let min = (numbers) => { 
  if(numbers.length > 2){ // 停止条件
    return min( 
      [ numbers[0], min( numbers.slice(1) ) ]    // 递归
    )
  }else{ 
    return Math.min.apply(null, numbers) // 必须用apply，call会传递整个数组。Math.min仅接收单个数字
  }
}

// minIndex 函数：获取任意数组中最小值的下标
let minIndex = (numbers) => {
  return numbers.indexOf( min(numbers) ) 
}

// sort3 函数：对长度为3的数组从小到大排序
let sort3 = (numbers) => { 
  let index = minIndex(numbers) 
  let min = numbers[index] 
  numbers.splice(index, 1) // numbers中仅保存最小值以外的两个元素，就可以使用sort2对numbers进行排序
  return [min].concat( sort2(numbers) )  // 返回一个从小到大排序后的数组
}
```



## 🔴长度为 4 的数组排序 sort4

```js
let sort4 = (numbers) => { 
  let index = minIndex(numbers) 
  let min = numbers[index] 
  numbers.splice(index,1) 
  return [min].concat( sort3(numbers) )
}
```



## 🔴推广：任意长度的数组排序 sort

>   递归

>   回顾：先获取最小值，然后从数组中排除最小值 … 进行大小比较 …

```js
let sort = (numbers) => {
  let index = minIndex(numbers)
  let min = numbers[index]
  numbers.splice(index, 1)
  return [min].concat( sort(numbers) )    // 死循环
}
```

### 思路一

>   中止条件
>
>   +   **当 numbers 中只有 2 个值时**
>   +   直接通过 三元运算符（问好冒号表达式）比较大小
>   +   并返回从小到大排序后的数组

```js
let sort = (numbers) => {
  if(numbers.length > 2){
    let index = minIndex(numbers)
    let min = numbers[index]
    numbers.splice(index, 1)
    return [min].concat( sort(numbers) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
// 用代入法看看 sort[12,5,8,7,9]
```

代入法 👇

```js
  sort[12,5,8,7,9]
= [5] + sort[12,8,7,9]
= [5] + ( [7] + sort[12,8,9] )
= [5] + ( [7] + ( [8] + sort[12,9] ))  // 👇 sort([12,9]) 进入停止条件 👇 
= [5] + ( [7] + ( [8] + ( 12 < 9 ? [12,9] : [9,12] )))
= [5] + ( [7] + ( [8] + ( [9,12] )))
= [5] + ( [7] + ( [8,9,12] ))
= [5] + ( [7,8,9,12] )
= [5,7,8,9,12]
```



### 思路二

>   开始「递进」，不断提取最小值。进入「回归」，将 每轮提取的最小值 进行逐个拼接

>   中止条件：不断提取最小值，**直到 numbers 长度为 1**，也就是只有一个元素时，直接返回 numbers 

```js
let sort = (numbers) => {
  if(numbers.length >= 2){
    let index = minIndex(numbers)
    let min = numbers[index]
    numbers.splice(index, 1)
    return [min].concat( sort(numbers) )
  }else{
    return numbers
  }
}
```

或

```js
let sort = (numbers) => {
  if(numbers.length === 1){
    return numbers
  }else{
    let index = minIndex(numbers)
    let min = numbers[index]
    numbers.splice(index, 1)
    return [min].concat( sort(numbers) )
  }
}
```

代入法 👇

```js
  sort[12,5,8,7,9]
= [5] + sort[12,8,7,9]
= [5] + ( [7] + sort[12,8,9] )
= [5] + ( [7] + ( [8] + sort[12,9] ))
= [5] + ( [7] + ( [8] + ( [9] + sort[12] )))  // sort[12] => [12]
= [5] + ( [7] + ( [8] + ( [9] + [12] )))  // 递进中止，开始回归
= [5] + ( [7] + ( [8] + [9,12] ))
= [5] + ( [7] + [8,9,12] )
= [5] + [7,8,9,12]
= [5,7,8,9,12]
```

​	

​	

## 代码调试

>   如果代码报错，如何调试

### 举例：splice易错点

>   [splice易错点](# 补充：splice 方法易错点)

```js
let sort = (numbers) => {
  if(numbers.length > 2){
    let index = minIndex(numbers)
    let min = numbers[index]
    let rest = numbers.splice(index, 1)  // splice的返回值与原数组，经常被搞混淆
    return [min].concat( sort(rest) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
```

<img src="https://i.loli.net/2020/10/11/CmW3uFPIqTzh6pj.png" alt="image-20201011040929836" style="zoom:80%;" />

### console.log 调试

```js
let min = (numbers) => { 
  if(numbers.length > 2){
    return min( 
      [ numbers[0], min( numbers.slice(1) ) ] 
    )
  }else{ 
    return Math.min.apply(null, numbers) 
  }
}
let minIndex = (numbers) => {
  return numbers.indexOf( min(numbers) ) 
}

let sort = (numbers) => {
  if(numbers.length > 2){
    let index = minIndex(numbers)
    console.log(`index: ${index}`)  // console调试大法
    let min = numbers[index]
    console.log(`min: ${min}`)   // console调试大法
    let rest = numbers.splice(index, 1)
    console.log(`rest: ${rest}`)   // console调试大法
    return [min].concat( sort(rest) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
```

<img src="https://i.loli.net/2020/10/11/go1R5Lx4HmpIsnU.png" alt="image-20201011041123739" style="zoom:67%;" />

-   打印结果，发现 rest 值有问题（正确的值应该是 `rest => [12,8,7,9]` ）
-   这时候就可以 mdn 查查 splice 写法是否正确（得知 splice 返回值为被删元素的数组，而非其余元素）
-   找到问题，得出正确写法：**无需获取返回值，numbers调用splice方法就会直接删除numbers中的指定元素 **
-   修改代码，如下 👇

```js
let sort = (numbers) => {
  if(numbers.length > 2){
    let index = minIndex(numbers)
    let min = numbers[index]
    console.log(`min: ${min}`)   
    numbers.splice(index, 1)  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
    console.log(`numbers: ${numbers}`)   
    return [min].concat( sort(numbers) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
```

<img src="https://i.loli.net/2020/10/11/e3ENdRUWbKF1BVY.png" alt="image-20201011042112671" style="zoom: 80%;" />

>   总结：新人一定要不停的  log 调试，当你把所有可能的问题都发现了，就成长了



### 举例：调用 min 报错

```js
let min = (numbers) => {
  if(numbers.length > 2){
    return min( [numbers[0], min( numbers.slice(1) ) ] )
  }else{
    return numbers[0] < numbers[1] ? numbers[0] : numbers[1]
  }
}
let sort = (numbers) => {
  if(numbers.length > 2){
    let index = numbers.indexOf( min(numbers) ) // 报错 Cannot access 'min' before initialization
    let min = numbers[index]
    numbers.splice(index, 1)
    return [min].concat( sort(numbers) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
sort([2,5,1,4,3])
```

<img src="https://i.loli.net/2020/10/11/CamSjRF7LkyqsKo.png" alt="image-20201011202800422" style="zoom:80%;" />

原因：

-   `let index = numbers.indexOf( min(numbers) )` 目的是获取到最小值的坐标，需要先调用 min函数 获取最小值
-   但是因为当前作用域中定义了同名的 min
-   JS 优先认为 ①`min(numbers)` 中的 min 是 ②`let min = numbers[index]`  中的 min
-   而执行 代码① 时，代码②还未执行，所以报错：不能在 min 初始化前访问

解决

+   给 min 变量改名



​	



## 完整代码 sort

```js
// min 函数：获取任意数组中最小值
let min = (numbers) => { 
  if(numbers.length > 2){ 
    return min( 
      [ numbers[0], min( numbers.slice(1) ) ]   
    )
  }else{ 
    // return Math.min.apply(null, numbers) 
    return numbers[0] < numbers[1] ? numbers[0] : numbers[1]
  }
}

// minIndex 函数：获取任意数组中最小值的下标
let minIndex = (numbers) => {
  return numbers.indexOf( min(numbers) ) 
}

// sort 函数：从小到大排序 ------------------------------------------------
let sort = (numbers) => {
  if(numbers.length > 2){
    let index = minIndex(numbers)
    let min = numbers[index]
    numbers.splice(index, 1)
    return [min].concat( sort(numbers) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
```

或

```js
let min = (numbers) => {
  if(numbers.length > 2){
    return min( [numbers[0], min( numbers.slice(1) ) ] )
  }else{
    return numbers[0] < numbers[1] ? numbers[0] : numbers[1]
  }
}
let sort = (numbers) => {
  if(numbers.length > 2){
    let index = numbers.indexOf( min(numbers) )    // （省略了 minIndex 函数）
    let minNum = numbers[index]
    numbers.splice(index, 1)
    return [minNum].concat( sort(numbers) )
  }else{
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()
  }
}
sort([2,5,1,4,3])
```

​	

​	

​	

## 总结

#### 求最小值

-   2个数
-   3个数
-   N个数

#### 排序

-   2个数
-   3个数
-   N个数

#### 用到的东西

-   本节只用到一个数据结构 —— 【数组】，提供了三个方法 slice（截取）、concat（连接）、splice（删除）
-   递归

