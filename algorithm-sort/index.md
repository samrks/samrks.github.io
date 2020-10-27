# 排序算法


<!--more-->

​	

## 前言

>   算法中最简单的就是排序算法

​	

## 复习代码

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
```

>   一句话总结：
>   每次找到最小的数放前面，然后对后面的数做同样的事情

​	

## minIndex

>   用于找出数组中最小数字的下标

### 你永远都有两种写法

+   「递归」和「循环」 

### 目前的 minIndex （递归算法）

>   通过 min 函数的「递归」算法，来获取到最小值，从而找到下标

```js
let minIndex= (numbers) => numbers.indexOf(min(numbers)) 
let min = (numbers) => {
  if(numbers.length > 2){
    return min( [numbers[0], min( numbers.slice(1) ) ] )    // 递归
  }else{
    return numbers[0] < numbers[1] ? numbers[0] : numbers[1]
  }
}
```

#### 缺点

+   看着就繁琐（递归中用了很多括号、还引用了额外的帮助函数min ）
+   重写吧

	

### 重写 minIndex 🔔 （循环算法）

>   用循环

思路：【 index 】作为一个标志，始终代表着「数组 numbers」中的「最小值」的【下标】

-   第一步，假设下标为 0 的元素，是 numbers 中的最小值，也就是 index = 0

-   把下标 1 的元素与下标 0 的元素进行比较，如果下标 1 的元素，比下标 0 的元素还小，也就是下标 1 是最小值。而 index 始终表示最小值的下标，所以 index 需重新赋值为 1

-   👆这种方法叫【贪婪法】：只要判断是比自己小的值，就认为是最小的值
    （俗称「渣男法」：只要看到一个女生比现在的女朋友漂亮，就要换女朋友）

-   逐步分析 👇

    ```js
    minIndex([9,6,8,13,5,4])
    => (假设9[0]最小) index = 0
    => (9遇到6[1]  -  9>6) index = 1
    => (6遇到8[2]  -  8>6) index = 1
    => (6遇到13[3] - 13>6) index = 1
    => (6遇到5[4]  -  5<6) index = 4
    => (5遇到4[5]  -  4<5) index = 5
    => index 变化过程 0→1→4→5
    ```



#### 源代码（循环实现minIndex） 👇

```js
1et minIndex = (numbers) => {
  let index = 0 
  for(let i=1; i < numbers.length; i++){ 
    if( numbers[i] < numbers[index] ){  
      index = i 
    }
  }
  return index
}
minIndex([9,6,8,13,5,4])
```

+   index 表示当前最小值下标，已经为 0，所以循环体中，应该是从下标为 1 的元素开始，和下标为 0 的元素进行比较大小
+   全部遍历完，返回最小值下标

​	

#### 分析

-    一目了然，一听就会，一写就错
-    写错，就记得多测试几次


​	

## 启发 💡

>   是不是所有的「递归」都可以写成「循环」 ？？

>   答：是的

+   **所有的递归，都可以改写成循环**
+   这是已经被证明的事情
+   如果觉得递归不好理解，都可以改写成循环，一般来说循环会更好理解，但循环写起来会更麻烦、代码量更大

	

	

## 选择排序 select sort  ⭕️

>   选择排序两种思路
>
>   1.  递归
>   2.  循环

>   改写 sort

<img src="https://i.loli.net/2020/10/18/6UpiSzrxcTDAvJd.gif" alt="选择排序" style="zoom:67%;" />

### 递归写法

>   复习一下  |  递归思路：
>
>   +   长度大于2，就找最小值放到前面 + 并对后面所有值再次 sort
>   +   长度等于2，就直接判断大小 / 交换两个元素位置，然后返回数组（中止条件）

```js
let minIndex= (numbers) => numbers.indexOf(min(numbers)) 
let min = (numbers) => { 
  console.log(`numbers: ${numbers}`)
  return min( [ numbers[0], min( numbers.slice(1) ) ] )   // 递归
}
let sort = (numbers) => { 
  if(numbers.length > 2){ 
    let index = minIndex(numbers) 
    let min = numbers[index] 
    numbers.splice(index,1) 
    return [min].concat( sort(numbers) )    // 递归
  }else{ 
    return numbers[0] < numbers[1] ? numbers : numbers.reverse()  
  }
}
```

​	

### 循环实现分析

>   思路不变：
>
>   +   每次找到最小的数放前面，~~然后对后面的数做同样的事情~~ 
>   +   然后 i++

>   「循环的每次找到最小的数，放前面 —— 选择排序」这个思路比较容易想到，但是代码写起来却比较困难

尝试写循环代码 👇

```js
1et minIndex = (numbers) => {
  let index = 0 
  for(let i=1; i < numbers.length; i++){ 
    if( numbers[i] < numbers[index] ){  
      index = i 
    }
  }
  return index
}
```

```js
let sort = (numbers) => {
  for(let i=0; i<???; i++){  
    let index = minIndex(numbers)
    // index 是当前最小数的下标，i 表示当前下标
    // index 对应的数应该放到 i 处（交换 index 和 i 的元素，这就是👇swap需要实现的）
    swap(numbers, index, i) // swap 还没实现
    
    // index、i 都是 index 的意思，建议 i 改名
  }
}
```

#### 分析

-   怎么知道 `i < ???` 处应该写什么（结束条件是什么）
-   提前写好 minIndex 能有效简化问题
-   用 swap 占位能有效简化问题（虽然还不知道swap应该实现的代码，但是先写个占位符，有助于分析思路）


​	

#### 一、实现 swap

>   实现交换 index 和 i 的元素

1、常规方法

```js
let swap = (array, i, j) => {
  let temp = array[i]
  array[i] = array[j]
  array[j] = temp
}
swap(numbers, 1, 2)
```

<img src="https://i.loli.net/2020/10/14/7DQOofUIdyYxzra.png" alt="image-20201014170634347" style="zoom: 33%;" />

2、JS 解构赋值（析构赋值）

```js
let a = 1; let b = 2;
[a,b] = [b,a]
// a => 2
// b => 1
```

​	

##### 错误的实现 swap

```js
let swap = (a,b) => { 
  let temp = a
  a = b
  b = temp
}
let numbers = [100,200,300]  // 能否实现交换元素位置呢 ？？
swap(numbers[1], numbers[2])
console.log(numbers) // [100,200,300]  未实现交换位置
```

>   你会发现，上述 numbers[1] 和 numbers[2] 的值原封不动

分析：

-   如果 a / b 是简单类型，传参的时候就会**复制值**
    -   numbers[1] 传给 a，相当于 a 复制了 200 这个值，a 与 numbers 并没有连带关系。 b 同理
    -   相当于只是把 a(200) 和 b(300) 交换了值，但这一切与 numbers  并没有关系
-   而前面常规方法中 numbers 是(数组)对象，传参时是**复制地址**
    -   `let swap = (array, i, j) => {...}`  这里的 array 形参是接收一个数组实参，也就是接收一个对象，对象传递时只是复制了地址，所以 array 形参始终代表了传递进来的实参数组 numbers
    -   所以 array[i] 和 array[j] 交换位置，也就等同于控制 numbers 中对应元素交换了位置

+   这就是【传值 V.S. 传址】的区别

    内存图相关知识：所有放在 stack 中的都是直接复制值，放在 heap 里面只能复制地址


​	

#### 二、分析 i < ??? 应该写什么

>   怎么知道 i < ??? 处应该写什么

暴力分析（逐步拆解）

```js
let sort = (numbers) => { 
  for(let i=0; i<???; i++){ 
    let index = minIndex(numbers) 
    swap(number，index,i)
  }
}
```

假设 numbers 的长度为 n = 4 👇

<img src="https://i.loli.net/2020/10/14/iDFdghnRT8j5ft7.png" alt="image-20201014173526006" style="zoom:67%;" />

+   第一次循环 i = 0，当前遍历的元素下标为 0 ，需要进行比较的元素、其下标 index 的取值范围只能是 1\2\3
+   第二次循环 i = 1，当前遍历的元素下标为 1 ，需要进行比较的元素、其下标 index 的取值范围只能是 2\3
+   第三次循环 i = 2，当前遍历的元素下标为 2 ，需要进行比较的元素、其下标 index 的取值范围只能是 3
+   第四次循环 i = 3，当前遍历的元素下标为 3 ，需要进行比较的元素、其下标 index 无法取值，所以 i 不能等于 3，一定要 i = 3 和 一个空数组进行比较也可以，但多此一举

>   结论：i 的取值为 0\1\2  【 i 能取到的最大值小于 3 ，也就是  i < numbers 的长度减 1 】

```js
for(let i=0; i<numbers.length-1; i++ ){...}
```

​	

​		

#### 发现 2 个问题

##### 1、minIndex 查找范围有问题

```js
let index = minIndex(numbers)
```

这句话有问题，如果上次循环已经找到了第一个最小的数字，那么之后找最小数字的时候，就可以忽略第一个

```js
let index = minIndex( numbers.slice(i) )  + i
```

##### 👆分析

>   下标 i 、下标 index 对应元素的交换经过

```js
  [20,40,30,10]
=> 当前i=0 , minIndex([20,40,30,10])=>最小值下标index=3 , 0与3交换位置=>[10,40,30,20] , 10位置钉住
=> 当前i=1 , minIndex([xx,40,30,20])=>最小值下标index=3 , 1与3交换位置=>[10,20,30,40] , 10/20位置钉住
=> 当前i=2 , minIndex([xx,xx,30,40])=>最小值下标index=2 就是当前元素 i，无需交换位置
=> （结束）i不需要再取3，因为上一步已经将最后两个元素进行比较
```

>   所以说，每一轮 minIndex 的元素，都需要忽略前一个元素，也就是👇 

+   当 i = 0，需要从 [20, 40, 30, 10] 中找出最小值，忽略 0 个 👉 [10, 40, 30, 20]

+   当 i = 1，需要从 [xx, 40, 30, 20] 中找出最小值，忽略 1 个 👉 [10, 20, 30, 40]

+   当 i = 2，需要从 [xx, xx, 30, 40] 中找出最小值，忽略 2 个 👉 [10, 20, 30, 40]

    <img src="https://i.loli.net/2020/10/14/lOu7tRSIMvCso4q.png" alt="image-20201014184731035" style="zoom:50%;" />

>   综上，发现规律：**i 等于 N，就需要忽略前面 N 个元素**

```js
所以 =>  minIndex( numbers.slice(i) ) 
```

##### 2、为什么要 +i

>   +   如果不加 i ， 那么 index 的取值计算，每次都是从 0 开始
>
>   +   因为每轮都切掉前面一个元素，导致下标数值发生变化

```js
当 i=0，忽略0个，从 [20,40,30,10] 中找出最小值10/下标为【3】，实际在[20,40,30,10]对应下标仍为【3】
👉 [10, 40, 30, 20]

当 i=1，忽略1个(下标为0的元素)，从[40,30,20]中找出最小值20/下标为【2】实际在[10,40,30,20]对应下标应为【3】
👉[10, 20, 30, 40]

当 i=2，忽略2个(下标为0/1的元素)，从[30,40]中找出最小值30/下标为【0】实际在[10,20,30,40]对应下标应为【2】
👉不交换
```

得出规律

+   i=0时，minIndex => 3，index => 3  👉 相当于 minIndex(3) + i(0) = index(3)
+   i=1时，minIndex => 2，index => 3  👉 相当于 minIndex(2) + i(1) = index(3)
+   i=2时，minIndex => 0，index => 2  👉 相当于 minIndex(0) + i(2) = index(2)

>   index = minIndex(…) + i

```js
// 最终得出
let index = minIndex( numbers.slice(i) )  + i
```

​	

#### 再次分析 i< ???

```js
let sort = (numbers) => { 
  for(let i=0; i<???; i++){ 
    let index = minIndex( numbers.slice(i) )  + i 
    swap(number, index, i)
  }
}
```

假设 numbers 的长度 n = 4

<img src="https://i.loli.net/2020/10/14/RBM7parPAeHUo9u.png" alt="image-20201014192912000" style="zoom:67%;" />

+   i 等于 3 时，在 `minIndex(numbers.slice(3))`中， numbers 只剩 numbers[3]  也就是 numbers[i] 本身，只剩一个元素，无法再和其他元素进行比较大小了，也就不需要 minIndex 操作了，**所以 i = 3 是无意义的**
+   所以 i 的取值从 0 开始，最大就到 2 为止 
+   **结论**：i 的取值范围是 i < 3 ，也就是 i < n-1

​	

​	

### 循环代码（注释版）

```js
let sort = (numbers) => {
  for(let i = 0; i < numbers.length - 1; i++){
    console.log(`----`) // 这个log很精髓
    console.log(`i: ${i}`) // 打印i，知道这是第几次比较
    let index = minIndex(numbers.slice(i)) + i // 找到最小值下标
    console.log(`index: ${index}`)
    console.log(`min: ${numbers[index]}`)
    if(index !== i){  // 如果最小值下标index 与 i 不等，就交换二者位置，相等就什么也不做
      swap(numbers, index, i) 
      console.log(`swap ${index}: ${i}`) // 打印出调换的 index 和 i
      console.log(`numbers: ${numbers}`)
    }
  }
  return numbers
}

// 下面是sort中引用的两个帮助函数： swap / minIndex

let swap = (array, i, j) => {   // 用于交换位置
  // let temp = array[i]
  // array[i] = array[j]
  // array[j] = temp
  [array[i], array[j]] = [array[j],array[i]]  // 也可以用解构赋值法，交换元素位置
}

let minIndex = (numbers) => {  // 用于找出最小值下标
  let index = 0
  for(let i = 0; i < numbers.length; i++){
    if(numbers[i] < numbers[index]){
      index = i
    }
  }
  return index
}
```

​	

​	

### 汇总 ✅

#### 循环代码（纯净版）

>   思路：循环的每轮，找到未排序数组中的最小值的下标，用于交换位置，把最小值放到最前面
>
>   * 循环的每轮，都假设 i 为当前轮次的最小值下标，并且 i 还可以表示当前未排序数组的第一位元素。
>   * 通过 minIndex 找到未排序数组中的最小值下标
>   * 如果最小值下标与假设不符，就通过 swap 交换位置

三部分组成：sort（循环排序）、swap（实现交换位置）、minIndex（找最小值下标）

```js
let sort = (numbers) => {
  for(let i = 0; i < numbers.length - 1; i++){   // 👈 重点理解！！（边界：为什么-1）
    let index = minIndex(numbers.slice(i)) + i   // 👈 重点理解！！（最小值下标：为什么+i）
    if(index !== i){ swap(numbers,index,i) }
  }
  return numbers
}
let swap = (array, i ,j) => {
  [array[i], array[j]] = [array[j],array[i]]
}
let minIndex = (numbers) => {
  let index = 0
  for(let i = 0; i < numbers.length; i++){
    if(numbers[i] < numbers[index]){
      index = i
    }
  }
  return index
}
```

#### 补充：双层 for 循环写法

>   特点：代码量少，只需一个函数。但思路、算法过程不易理解

>   思路同上：每轮找未排序数组中的最小值的下标，用于交换位置，把最小值放到最前面
>
>   * 外层循环：每轮开局，都假设当前遍历元素 j（同时也是未排序数组的第一个元素）为最小值 minIndex
>   * 内层循环：当前未排序数组中的第一个元素 j 和 后面元素依次比较大小，确定当前最小值下标
>   * 如果当前最小值下标与假设不符，就交换二者位置

```js
let selectSort = arr =>{
  for (let j = 0; j < arr.length - 1; j++) {  // j表示每轮遍历的元素的下标；i表示下一位元素的下标
    let minIndex = j  // 每轮都假设当前元素是最小值，当前元素下标 j 是最小值下标
    for (let i = j + 1; i < arr.length; i++) { 
      if (arr[i] < arr[minIndex]) { //  当前元素与下一个元素，进行两两比较，找出最小值下标
        minIndex = i
      }
    }
    if (minIndex !== j) {  // 若最小值不是当前元素 j ，那就把最小元素与当前元素交换位置
      [arr[minIndex], arr[j]] = [arr[j], arr[minIndex]]
    } 
    // 每一轮都从未排序数组中找出最小值，并放到未排序数组的最前面(第一位)
    // 注：每一轮遍历的当前元素 j 所在位置，就是当前未排序数组的第一位
  }
  return arr
}
```

#### 对比：递归写法

>   特点：思路更简明、但需要帮助函数   【边界处理 ( 中止条件 ) 是关键！！】

>   递归思路：
>
>   +   长度大于2，就找最小值放到前面 + 并对后面所有值再次 sort
>   +   长度等于2，就直接判断大小 / 交换两个元素位置，然后返回数组。【👈中止条件（边界处理）】

```js
let minIndex = arr => arr.indexOf(min(arr))
let min = arr => {
  if(arr.length > 2){
    return min( [arr[0], min( arr.slice(1) ) ] )  // 递归
  }else{
    return arr[0] < arr[1] ? arr[0] : arr[1]   // 👈 边界处理很关键！！
  }
}
let sort = arr => {
  if(arr.length > 2){
    let index = minIndex(arr)
    let minNum = arr[index]
    arr.splice(index,1)
    return [minNum].concat(sort(arr))   // 递归
  }else{
    return arr[0] < arr[1] ? arr : arr.reverse()   // 👈 边界处理很关键！！
  }
}
```

​	

### 总结

>   选择排序的两种实现方式：递归、循环

-   思路：每次选择最小 / 大的，放在最前面 / 最后面。选到最后没得选了，就排完了 
-   假设有 10 个元素，就需要选择 8 次、对比 8 次。
    -   第 1 个元素不用选，默认第 1 个直接和第 2 个对比
    -   第 10 个也不用选，选第 9 个时，就已经和第 10 个对比了
    -   所以只需要对比 8 次
-   每次对比，需要进行一次搜索（找到最小值）
-   所以时间复杂度、大概是「 n 的平方 」

​	

​	

## 总结 👆

### 1. 所有递归都能改成循环

### 2. 循环的时候有很多细节

-   循环时特别容易被细节干扰，这些细节很难想清楚
-   要动手列出表格，找规律
-   尤其是**边界条件**很难确定
-   我们没有处理长度为 0 和 1 的数组（if length === 0 | 1 直接 return 即可）

### 3. 如果 debug

-   学会看控制台
-   学会打 log
-   打 log 的时候，注意加标记

​	

​	

​	

## 快速排序 quick sort ⭕️

>   特点就是「快」

>   只讲递归思路，不讲循环思路。通过上面的学习，已经知道递归更简单、循环非常复杂（细节很多）

### 递归思路

>   以某某为基准

-   想象你是一个体育委员
-   你面对的同学为 [12，3，7，21，5，9，4，6]   
-   **「以某某为基准，小的去前面，大的去后面」  **  （不会规定「你」需要站在哪里）
-   你只需要重复说上面这句话，就能让他们完成排序
-   神奇不神奇？
-   用图说明一下

```js
=> [12，3，7，21，5，9，4，6]
			   		 👆
=> 假设站在靠中间的 21 上
=> 喊话「以21为基准，比21小的，站到21前面，大的站到后面」
=> [12，3，7，5，9，4，6，21]   经过这一次排序，21 的位置就钉住了
						    			 👆
=> 再次随机找一个数（从中间找），喊话「以5为基准，比它小的站到前面，比它大的站到后面」
=> [12，3，7，5，9，4，6，21]
             👆
=> [3，4，   (5)，12，7，9，6，(21)]  // [3,4]是比5小的，[12,7,9,6]是比5大的，5的位置就钉住了

=> 接下来在[3,4]中间任选一个（假设选4吧），喊话「比4大的去前面，比4小的去后面」
=> [3]、(4)、(5)、[12,7,9,6]、(21)  // 此时 4 的位置就钉住了。3固定了吗？不行，每次指向谁，谁才能固定
=> 此时指向3，发现3就一个元素自身，那无需比较，3可以固定了
=> 下面跑到右边一组去，指向 7 吧，喊话「比7大的去前面，比7小的去后面」
=> (3)、(4)、(5)、[6,(7),12,9]、(21)   // 假设下面指向6，就一个可以直接固定了
=> (3)、(4)、(5)、(6),(7),[12,9]、(21)   // 下面指向 9 吧，所以9就钉住了，喊话「比9大的往后面去...」
=> (3)、(4)、(5)、(6)、(7)、(9)、[12]、(21)   // 就剩12一个元素了，此时指向 12，就一个所以直接固定位置
=> (3)、(4)、(5)、(6)、(7)、(9)、(12)、(21)
=> [3,4,5,6,7,9,12,21]
```

>   思路
>
>   -   一共 8 个元素，每指向一个元素，就会固定一个元素的位置
>   -   所以只需要指 8 次，就完成排序了
>   -   【这就是快速排序的思路】

​	

### 快排源码（递归）

>   下面是在[阮一峰写的版本](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)基础上、加工成 ES6

>   因为使用递归思想，所以必须考虑中止条件、对各种情况进行相应处理

逐步分析 👇

+   如果发现数组**只有一个元素**，就无需排序，直接放行（中止条件）

    注意：这里条件必须写 `arr.length <= 1`  ，不能只有`arr.lenght === 1` 这会导致递归无法结束

+   [pivot](https://zh.forvo.com/search/pivot/en/)  /ˈpɪvət/ —— 基准、中心点、轴

+   取地板（舍去小数部分）：Math.floor(3.5)  →  3  

+   pivot ，这里的 [splice](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 用于删除数组中的某个元素，并把它。（splice 会修改原数组）
    因为 JS 中 splice 提取后返回的是一个数组，必须通过 [0] 的方式才能拿到数字本身。并赋值给变量 pivot
    拿到基准数，并将基准数从 arr 中删除

+   既然已经拿到基准数的下标，为什么不直接通过 `let pivot = arr[pivotIndex]` 来获取到基准数，而是还要通过 splice 的返回值来获取基准数、这么麻烦的方式？？
    因为这里实际上是完成两个操作：第一目的是获取基准数、第二目的是还需要将基准数从 arr 中删除，所以使用 splice 正好可以同时完成这两个操作

```js
let quickSort = arr => {
  if(arr.length <= 1) { return arr }   // 最基本的情况：发现指向的数组只剩下一个元素
  let pivotIndex = Math.floor(arr.length / 2)  // 获取基准的索引、找到靠中间的数字（取地板）
  let pivot = arr.splice(pivotIndex, 1)[0]  // 拿到基准数，并将基准数从 arr 中删除
  let left = []
  let right = []
  for(let i=0; i < arr.length; i++){   // 遍历被删掉基准数后的数组 （执行喊话操作）
    if(arr[i] < pivot){ 
      left.push(arr[i])  // 如果当前遍历元素小于基准数，就放到left数组中
    }else{
      right.push(arr[i]) // 由此得到了三部分：左边数组、基准数、右边数组
    } 
  }
  return quickSort(left).concat( [pivot], quickSort(right) )  // 👈 代码的核心就是这句
  // 不断对左边数组快排、右边数组快排、连接两边数组和基准数
  // 停止条件是 数组只剩下一个元素，直接返回数组
}
```

>   思路：
>
>   +   对数组「找基准数、然后左右分半」 … 不断递归（循环）这个操作
>   +   直到数组只剩一个元素，就不再执行直接返回

>   所有算法的思路都很简单，难再代码实现

#### 代码（纯净版）

>   思路：
>
>   * 每次找一个中间基准数。将数组对半，大于基准数，放到左边数组，小于基准数放到右边数组。
>   * 然后再次分别对左边/右边数组排序（层层递归、压栈）
>   * 中止（边界）条件：数组<=1个元素，就可以直接返回这个具体值（开始弹栈）

```js
let quickSort = arr => {
  if(arr.length <= 1){ return arr }
  let pivotIndex = Math.floor(arr.length / 2)
  let pivot = arr.splice(pivotIndex,1)[0]
  let left = []
  let right = []
  for(let i=0; i<arr.length; i++){
    if(arr[i] < pivot){ left.push(arr[i]) }
    else{ right.push(arr[i]) }
  }
  return quickSort(left).concat([pivot], quickSort(right))
}
```

​	

### 补充说明

>   例：`let pivot = arr.splice(pivotIndex, 1)[0]`  因为 splice 的特性，导致必须用 [0] 来拿到基准数本身

```js
let arr = [16,32,45]
let a = arr.splice(1,1)
a  // [32]
------------------------------
let arr = [16,32,45]
let a = arr.splice(1,1)[0]
a  // 32
```

>   算法如果涉及到某个语言的细节，这是非常不好的。
>   最好的算法写法是用「伪代码」来写。自己发明语法，就不用纠结 API 的问题了。

>   面试时，可以尝试鸡贼的写法，询问面试官可否使用「伪代码」来写

​	

### 思路一句话

>   以某个元素为基准，小的往前放，大的往后放

​	

​	

​	

## 归并排序 merge sort ⭕️

>   是当前三种排序算法中最难理解的一个

###  「不」以某某为基准

-   想象你是一个体育委员
-   你面对的同学为 [12，3，7，21，5，9，4，6]
-   **左边一半排好序，右边一半排好序**
-   **然后把左右两边合并（merge）起来 **
-   神奇不神奇？
-   用图说明一下

​	

### 思路分析

```js
   [12，3，7，21，5，9，4，6]

=> [12,3,7,21]，[5,9,4,6]  // 【每次都把数组看作左边和右边，并让左右边自己排序】
    ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓        
=> [3,7,12,21]，[4,5,6,9]  // 左右排序完成（先忽略排序细节）

=> 现在只需要把左右两部分【结合】起来，怎么结合？ // 【合并】
=> [3, 7, 12, 21]，[4, 5, 6, 9]   
	  👆              👆
    // 左手指向左边第一个，右手指向右边第一个。比较两个值大小，把较小的放到容器中。 ✅✅
    // 此时左边的 3 更小，所以先把 3 放到下面容器中。然后左手指向下一个元素 7 
=> [3,                        ]
=> [3, 7, 12, 21]，[4, 5, 6, 9]    
	     👆           👆
   // 此时比较 7 和 4。把较小值 右边的 4 放到容器中。右手指向下一元素
   [3, 4,                      ]
=> [3, 7, 12, 21]，[4, 5, 6, 9] 
	     👆              👆
   // 此时比较 7 和 5。把较小值 右边的 5 放到容器中。右手指向下一元素
   [3, 4, 5,                   ]
=> [3, 7, 12, 21]，[4, 5, 6, 9] 
	     👆                 👆
   // 此时比较 7 和 6。把较小值 右边的 6 放到容器中。右手指向下一元素
   [3, 4, 5, 6,                ]
=> [3, 7, 12, 21]，[4, 5, 6, 9] 
	     👆                    👆
   // 此时比较 7 和 9。把较小值 左边的 7 放到容器中。左手指向下一元素
   [3, 4, 5, 6, 7,             ]
=> [3, 7, 12, 21]，[4, 5, 6, 9] 
	        👆                 👆
   // 此时比较 12 和 9。把较小值 右边的 9 放到容器中。右边元素排完了.
   [3, 4, 5, 6, 7, 9           ]

当某一边元素全部排完，另一边元素就可以全部照抄了 （左边还剩12,21直接放进容器中）✅✅
大功告成
=> [3, 4, 5, 6, 7, 9, 12, 21]

```

>   综上可知：**【合并】的算法**其实很简单：左右两边 index 同时遍历，把较小值 push 到容器中

>   问题是：怎么让左右两边自己先完成排序？ （递归思想 👇）

```js
数组 [12,3,7,21,5,9,4,6] 分成左右两边 [12,3,7,21]，[5,9,4,6] 之后
将左边 [12,3,7,21] 再次划分为两部分 [12,3]、 [7,21]
将左边 [12,3] 再次划分为两部分 [12]、 [3]  
当拆分到只有两个元素时，就可以直接比较大小排序了（如果不会，还可以再进行前面的【合并】操作）
=> [12]、 [3] 
=>  👆     👆    // 左手指向左边第一个，右手指向右边第一个。将较小值放到容器中
=> [3,   ]  
   // 此时右边全部排完。那么左边直接照抄落到容器后面即可
   // 大功告成
=> [3, 12]，同理得到 [7, 21]
```

+   上面 从 [12]、 [3] 得到 [3, 12]  这一步是最关键的一步。
+   将数组 [12,3]，[7,21]  拆分为 [12]  [3]，[7] [21] … **「使得所有数组，变成了排好序的数组」**怎么理解这句？
    +   被拆分后，数组中只有一个元素，一个元素也就无需排序。
    +   变相的理解成 **「此时所有的数组，都是无需排序的数组（排好序的数组）」 **

>   归并排序的算法：默认只能，对两个排好序的数组，进行排序
>
>   +   这个算法可能乍一看，很不可思议，好像什么都没做，就排好序了。
>   +   一定要拆解步骤，才能发现这里面的神奇之处。

<img src="https://i.loli.net/2020/10/16/bOHQqvBUZ365fjp.png" alt="image-20201016220730023" style="zoom: 50%;" />

```js
// 再合并（左手指左一、右手指右一，较小值放到容器中）✅✅
=> [3, 12]，[7, 21]
    👆       👆
   // 比较 3 和 7。把较小值 左边的 3 放到容器中。左手指向下一元素 12
   [3,        ]
=> [3, 12]，[7, 21]
       👆    👆
   // 比较 12 和 7。把较小值 右边的 7 放到容器中。右手指向下一元素 21
   [3, 7,     ]
=> [3, 12]，[7, 21]
       👆       👆
   // 比较 12 和 21。把较小值 左边的 12 放到容器中。左边全部排完。右边剩下[21]直接落到后面
   [3, 7, 12    ]

  // 大功告成
=> [3, 7, 12, 21]
```

​	

### 源码分析

```js
let mergeSort = arr => {
  if (arr.length === 1) {
    return arr
  }
  let left = arr.slice(0, Math.floor(arr.length / 2)) 
  let right = arr.slice(Math.floor(arr.length / 2))
  return merge(mergeSort(left), mergeSort(right))  // 👈关键
}
let merge = (a, b) => { // 👈 核心
  if (a.length === 0) return b
  if (b.length === 0) return a
  return a[0] > b[0] ? [b[0]].concat(merge(a, b.slice(1))) : [a[0]].concat(merge(a.slice(1), b))
  // 关键 👆
}
```

<img src="https://i.loli.net/2020/10/18/dUjuSe9kBQE7IKi.gif" alt="归并排序" style="zoom:50%;" />

#### 详细注释 👇

```js
let mergeSort = arr => {
  if (arr.length === 1) {  // 长度为1的数组，无需排序，所以默认它是已经排好序的数组【这点非常关键】
    return arr
  }
  // arr.slice(begin,end) 截取数组下标从begin到end的部分，返回新数组（包括begin，不包括end）
  // 原数组 arr 不改变
  // 省略 end，则 slice 会一直提取到原数组末尾
  let left = arr.slice(0, Math.floor(arr.length / 2)) // left是从下标0截取到一半的位置（不包括end）
  let right = arr.slice(Math.floor(arr.length / 2)) // right是从一半的位置，截取到末尾（包括begin）
  return merge(mergeSort(left), mergeSort(right)) 
  // 左右再次进行拆分操作。拆到数组只有1个元素，认为所有数组已经排好序。
  // 对排好序的数组进行 merge 合并（这才是归并算法的核心👇见下）
}
let merge = (a, b) => {  // 【前提条件：merge 接收的a、b两个数组，必须是已经排好序的两个数组】！！！
  if (a.length === 0) return b  // 一个空数组a和一个已经排好序的数组b，那就直接返回排好序的数组b
  if (b.length === 0) return a  // 同理
  return a[0] > b[0] ? [b[0]].concat(merge(a, b.slice(1))) : [a[0]].concat(merge(a.slice(1), b))
  // 👆这里就是递归的难理解之处，需要拆解步骤 ⚠️⚠️见下
}
```

#### 拆解 merge⚠️⚠️

>   【前提条件：merge 接收的 a、b 两个数组，必须是已经排好序的两个数组】！！！

```js
	 merge([1,5,10], [2,4,9])
=> a[0] > b[0] ? [b[0]].concat(merge(a, b.slice(1))) : [a[0]].concat(merge(a.slice(1), b))

---------------------------------------------拆解 👇-----------------------------------------
// 第一步：指向两个数组的第一位，比较大小
=> [1,5,10], [2,4,9]  
    ↑         ↑       
   1 > 2 否，执行 [a[0]].concat( merge( a.slice(1), b ) ) // 相当于把较小值摘出，再次merge剩余部分
=> [1, merge( [5,10], [2,4,9] )]
               ↑       ↑
   5 > 2 是，执行 [b[0]].concat( merge( a, b.slice(1) ) ) // 把较小值摘出，再次merge剩余部分
=> [1, 2, merge([5,10], [4,9]) ]
                 ↑       ↑
   5 > 4 是，执行 [b[0]].concat( merge( a, b.slice(1) ) ) // 把较小值摘出，再次merge剩余部分
=> [1, 2, 4, merge([5,10], [9]) ]
	                  ↑       ↑
   5 > 9 否，执行 [a[0]].concat( merge( a.slice(1), b ) ) // 把较小值摘出，再次merge剩余部分
=> [1, 2, 4, 5, merge([10], [9]) ]
		                   ↑     ↑
   10 > 9 是，执行 [b[0]].concat( merge( a, b.slice(1) ) ) // 把较小值摘出，再次merge剩余部分    
=> [1, 2, 4, 5, 9, merge([10], []) ]   
   											 ↑    ↑
   // 满足中止条件：一个空数组、一个已经排好序的数组 ，那就直接返回排好序的数组b
=> [1, 2, 4, 5, 9, 10]
```

 	

#### 补充：[slice 用法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

```js
arr.slice(begin, end)
```

+   **截取数组下标从 begin 到 end 的部分，返回一个新数组（包括begin，不包括end）**

+   **原始数组不改变 **

+   begin （可省略）

    提取起始处的索引（从 0 开始），从该索引开始提取原数组元素。
    如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，slice(-2) 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
    **如果省略 begin，则 slice 从索引 0 开始。 **
    如果 begin 大于原数组的长度，则会返回空数组。

+   end（可省略）

    提取终止处的索引（从 0 开始），在该索引处结束提取原数组元素。slice 会提取原数组中索引从 begin 到 end 的所有元素（包含 begin，但不包含 end）。
    slice(1,4) 会提取原数组中从第二个元素开始一直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
    如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 slice(-2,-1) 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。
    **如果 end 被省略，则 slice 会一直提取到原数组末尾。 **
    如果 end 大于数组的长度，slice 也会一直提取到原数组末尾。

```js
// 省略两个参数，可以实现深拷贝效果 ？！
let arr = [1,2,3,4,5]
let newArr = arr.slice()
console.log(`newArr: ${newArr}`)  // 6,2,3,4,5
newArr[0] = 6
console.log(`修改后，newArr: ${newArr}，arr: ${arr}`)  // 6,2,3,4,5   1,2,3,4,5 原数组不改变
```

​	

<img src="https://i.loli.net/2020/10/18/dUjuSe9kBQE7IKi.gif" alt="归并排序" style="zoom:50%;" />

### 代码（纯净版）

```js
let mergeSort = arr => {
  if (arr.length === 1) {
    return arr
  }
  let left = arr.slice(0, Math.floor(arr.length / 2)) 
  let right = arr.slice(Math.floor(arr.length / 2))
  return merge(mergeSort(left), mergeSort(right))  
}
let merge = (a, b) => {
  if (a.length === 0) return b
  if (b.length === 0) return a
  return a[0] > b[0] ? [b[0]].concat(merge(a, b.slice(1))) : [a[0]].concat(merge(a.slice(1), b))
}
```

​	

### 总结

#### 归并排序的思路

+   mergeSort

    +   拿到一个乱序的数组，会把数组分成**左右两部分**。然后对左右两边继续调用 mergeSort 再拆分 …
    +   拆分到所有元素独自成一个数组，达到中止条件。
+   merge
    +   接收两个数组作为参数（只接收排好序的数组）
    +   **每次都比较两个数组的首项，并提取出较小的值，放在最前面 **
        因为两个数组都是顺序排列，所以首项一定代表其所在数组的最小值。
        两个最小值对比得出的较小值，一定是所有元素中的最小值，所以摘出放在最前面
    +   对剩余数组，继续重复上一步操作 


​	

#### 代码

```js
let mergeSort = arr => {
  if(arr.length === 1){ 
    return arr
  }
  let left = arr.slice(0, Math.floor(arr.length/2))
  let right = arr.slice(Math.floor(arr.length/2))
  return merge(mergeSort(left), mergeSort(right))
}
let merge = (a,b) => {
	if(a.length === 0) return b
  if(b.length === 0) return a
  return a[0] < b[0] ? [a[0]].concat(merge(a.slice(1),b)) : [b[0]].concat(merge(a, b.slice(1)))
}
```

​	

#### 图示

##### 1

```js
merge(     a    ,    b    )
merge( [1,5,10] , [2,4,9] )
```

<img src="https://i.loli.net/2020/10/17/5PCb21HmSeoqput.png" alt="image-20201017041900582" style="zoom:67%;" />

##### 2

```js
mergeSort([12,3,7,21, 5,9,4,6])
```

<img src="https://i.loli.net/2020/10/16/bOHQqvBUZ365fjp.png" alt="image-20201016220730023" style="zoom: 50%;" />

##### 3

```js
mergeSort([6,3,2,7,1,5,8,4])
merge( mergeSort([6,3,2,7]), mergeSort([1,5,8,4]) )
merge( merge( mergeSort([6,3]), mS([2,7]) ), merge( mergeSort([1,5]), mergeSort([8,4]) ) )
m(m(m(mS([6]),mS([3])), m(mS([2]),mS([7])) ) , m(m(mS([1]),mS([5])), m(mS([8]),mS([4]))))
// 达到 mergeSort 中止条件 👆
m( m( m([6],[3]), m([2],[7]) ) , m( m([1],[5]), m([8],[4]) ) )
// 开始 merge 👇
m( m( [3].concat( m([6],[]) ) , [2].concat( m([7],[]) ) )  ...  ) ) 
m( m( [3].concat([6]), [2].concat([7]) ) , m( [1].concat([5]), [4].concat([8]) ) )
m( m( [3,6] , [2,7] ), m( [1,5], [4,8] ) )
m( [2].concat( m([3,6], [7]) ) , [1].concat( m([5], [4,8]) ) )
m( [2].concat( [3].concat(m([6], [7])) ) , [1].concat( [4].concat(m([5], [8])) ) )
m( [2].concat( [3].concat([6].concat(m([], [7]))) ),[1].concat( [4].concat( [5].concat(m([], [8])) ) ) )
  ...
  m( [2,3,6,7],[1,4,5,8] )
```

![image-20201017035332224](https://i.loli.net/2020/10/17/K1hxXcdqyJPMVaZ.png)

<img src="https://i.loli.net/2020/10/18/dUjuSe9kBQE7IKi.gif" alt="归并排序" style="zoom:50%;" />

​	

### 思路一句话

>   每次都分两部分，默认是排好序的，如果没有排好序就先排序。然后对排好序的两个数组，进行合并

+   怎么把无序的数组排好序？

    很简单，（变相理解）不断拆分数组，直到每个数组中只有一个元素，这样每个数组都是排好序的

+   然后两个一组，开始进行合并

  ​    

> 归并的思路确实很抽象，是上述三种算法种最难理解的（很哲学）
>
> 如果实在无法理解归并排序，那就学到快速排序就不要再学了

​	

​	

​	

## 总结

### 目前理清了三种排序

-   选择排序（递归、循环）
    +   每次选择最小的，放在最前面。选到最后没得选了，就排完了 
-   快速排序（递归）
    +   以某个元素为基准，小的往前放，大的往后放
-   归并排序（递归）
    -   每次都分两部分，默认是排好序的，如果没有排好序就先排序。然后对排好序的两个数组，进行合并
    -   是上述三种算法种最难理解的（很哲学）

​	

### 接下来

-   **计数排序 **（循环）
    -   比上面的排序算法都要**快 **


​	

​	

​	

## 计数排序 counting sort ⭕️

>   特性：速度非常快

>   注：本节如果提到哈希，实际说的是哈希表（因为我们并没有接触过哈希函数 … ）

### 思路

-   用一个新的**数据结构** —— **哈希表**，来作记录
    -   哈希表：一种 key: value 的形式。
    -   JS 的对象可以算是哈希表的一种形式，但不是纯粹的哈希表。
    -   因为 JS 对象具有隐藏属性、函数，而真正的哈希表里没有隐藏属性，只有数据。
        所以 JS 对象不能算是一个完全的哈希表
-   发现数字 N 就记 N：1，如果再次发现 N 就加 1
-   最后把哈希表的 key 全部打出来，假设 N：m，那么 N 需要打印 m 次
-   画图演示

​	

#### 扑克牌

+   「一副扑克牌，（不算大小王）共 52 张，乱序 」  

+   怎么对这副扑克牌排序 ，实现 【 AAAA , 2222 , 3333 , 4444 , … , JJJJ , QQQQ , KKKK 】 的排序结果 ？？

    ```js
    [1,2,1,3,4,1,K,K,J,J,5, ...... ]  // 52张牌乱序
     // 👇 哈希表：每碰到一个数，就对应位置记上一笔。（类似计数器）
     {
     	 A：0
     	 2：0
     	 3：0  
     	 J：0 
     	 Q：0	 
     	 K：0
     }
    ```

+   人类天生就会排序，只不过可能不会用代码表示出来

​	

### 示例分析

```js
	 [12,3,9,4,2,8,5,7]
=> 发现数字 N 就记 N：1，再次发现 N 就加 1。 
   同时记录最大值 max：假设第一个元素就是最大值，max = 0。遇到3 <12，max不变；9<12，max不变 ...
	 后面的数都比 12 小，所以 max 就是 12
=> {
     12:1  (max)
     3:1
     9:1
     4:1
     2:1
     8:1
     5:1
     7:1
   }
   现在完成两件事：
   1、x最大值为12
   2、所有数据次数记录完毕。
   接下来，开始循环
   
=> 设定 i = 0 ~ 12(max)，如果发现哈希表里存在 i 值，就把 i 值打印到一个数组里面
   i = 0, 哈希表中没有 0 => []
   i = 1, 哈希表中没有 1 => []
   i = 2, 哈希表中有 2  =>  [2, ]
   i = 3, 哈希表中有 3  =>  [2,3, ]
       4               =>  [2,3,4, ]
       5               =>  [2,3,4,5, ]
       6               =>  [2,3,4,5, ]
       7               =>  [2,3,4,5,7, ]
       8               =>  [2,3,4,5,7,8, ]
       9               =>  [2,3,4,5,7,8,9, ]
       10              =>  [2,3,4,5,7,8,9, ]
       11              =>  [2,3,4,5,7,8,9, ]
       12              =>  [2,3,4,5,7,8,9,12 ]   // 👈 排序完成

  // 当 i 从 0 ~ 12 走完，排好序的数组也就得出了
  // 需要两个条件姐就可以实现排序：哈希表、最大值max
```

​	

###  初步代码

>   补充： [in 操作符用法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in)

```js
let countSort = arr => {
  let hashTable = {}, max = 0, result = []
  for(let i = 0; i < arr.length; i++){  // 遍历数组，得到【哈希表】和【max】
    if(!(arr[i] in hashTable)){ // 发现数字 N 就记 N：1，再次发现 N 就加 1
      hashTable[arr[i]] = 1
    }else{
      hashTable[arr[i]] += 1
    }
    if(arr[i] > max) { // max：谁比我大，我就等于谁
      max = arr[i] 
    }
  }
  for(let j = 0; j <= max; j++){  // 遍历哈希表
    if(j in hashTable){
      result.push(j)   // 如果发现这个值在hash表中，就push到数组
    }
  }
  return result
} 
```

>   #### 上述代码有bug
>
>   +   遍历哈希表时，没有考虑「一个值在 hash 表中出现多个」的情况          

#### 举例验证 bug

```js
[4,2,2,5,2]
// 得到 hash表 👇
{
  4: 1,
  2: 3,
  5: 1  
}
// 遍历哈希表
for(let j = 0; j <= max; j++){  
  if(j in hashTable){  //  如果发现 j 在 hash表 中，就 push 到数组
    result.push(j)   
  }
}

// j 取 0~5 （如果发现 j 在hash表中，就把 j 添加到数组）
// j=0  =>  []
// j=1  =>  []
// j=2  =>  [2, ]
// j=3  =>  [2, ]
// j=4  =>  [2,4, ]
// j=5  =>  [2,4,5 ]    遍历完成，结果数组中少了两个元素 2
```

​	

### 完整代码 ✅

```js
let countSort = arr => {
  let hashTable = {}, max = 0, result = []
  for(let i = 0; i < arr.length; i++){
    if(arr[i] in hashTable){
      hashTable[arr[i]] += 1 
    }else{
      hashTable[arr[i]] = 1
    }
    if(arr[i] > max){ max = arr[i] }
  }
  for(let j = 0; j <= max; j++){
    if(j in hashTable){
      for(let i = 0; i < hashTable[j]; i++){  
        // 假设 j 出现了 3 次，就需循环 3 次(添加 j)的操作。
        // i 可以取几个值，循环就执行几次，所以 i 应该取 3 个值 （从0开始就取 i = 0，1，2）
        result.push(j)
      }  
    }
  }
  return result
}
```

<img src="https://i.loli.net/2020/10/18/yhsGoNZJfAFKXYB.gif" alt="计数排序（快）" style="zoom: 50%;" />

### 思路总结 ✅

+   遍历数组，得到一个 hashTable（记录出现过的元素 key，以及出现次数 value）。
+   同时，在这次遍历数组的过程中，找到数组最大值
    （开局假设第一个元素就是最大值max，依次比较，大于 max 的元素，就重新赋值给 max）
+   此时，已知 hashTable 和 max。
+   已知最大值 max，所以全部元素的取值都在 0 ~ max 这个范围之间
+   遍历 0 ~ max 这个范围之间的所有元素，如果与哈希表的 key 一致，就之间把这个元素 push 到数组中
    +   如果当前 key(元素) 的 value(出现次数) 不是 1，说明原数组中有 N 个该元素，那就需要把 N 个该元素都在此时 push 到数组中。所以 push 操作需要循环执行 N 次
    +   获取这个 value 次数，作为 for 循环执行次数 i 的依据，来控制 push 的执行轮次

​	

### 计数排序的特点 ✅

#### 数据结构不同

-   使用了额外的 hashTable （数据结构）

    -   计数排序中使用的数据结构升级了
    -   算法也就直接升级了，非常快

-   只遍历数组一遍（不过还要遍历一次 hashTable ）

    -   之前的排序算法，都会多次遍历数组
    -   选择排序：找第一个最小值，需遍历一遍数组。找第二个最小值，需再把余下元素遍历 …（重复遍历）

-   为什么计数排序，可以这么厉害，就遍历一遍数组呢？

    答：就是因为有 hashTable，这叫做「用空间换时间」

    +   hashTable 就是存储在内存中的一块空间。
    +   用多余的空间就可以节省多余的时间。
    +   通常空间、时间只能二选一：「用空间换时间」或「用时间换空间」
        （除非你智商碾压，二者都能实现）


​	

#### 时间复杂度对比

-   选择排序 O(n^2）
-   快速排序 O(n log2n)
-   归并排序 O(n log2n)
-   计数排序 O(n + max)   
    -   时间最少、速度最快，但空间占的多
    -   先遍历一个长度为 n 的数组，再遍历一个长度为 max 的数组

>   时间复杂度，到底是怎么计算出来的呢？ 
>
>   +   其实有特别简单的方法，几乎不需要太复杂的数学知识  👇 [详见下](# 时间复杂度)

​	

​	

​	

### 题外话：字母出现次数

前面讲过案例「如何统计一段文字中字母出现的次数，并打印结果」
其实就是借鉴了【计数排序】的思维

```js
let count = str => {
  let result = {}
  let newStr = str.replace(/[^a-zA-Z]/g, '')   // `HiImSam`
  console.log(newStr)
  for(let i=0; i<newStr.length; i++){
    if(newStr[i] in result){
      result[newStr[i]] += 1
    }else{
      result[newStr[i]] = 1
    }
  }
  return result
}
let str = `Hi, I'm Sam`
let newStr = str.match(/a-zA-Z/g)
count(str)
--------------------------------------------------
{
  H: 1
  I: 1
  S: 1
  a: 1
  i: 1
  m: 2
}
```

#### 补充正则

```js
提取数字....value.replace(/[^\d]/g, '')
提取中文....value.replace(/[^\u4E00-\u9FA5]/g, '')
提取英文....value.replace(/[^a-zA-Z]/g, '')
```

例

```js
let str = `Hi, I'm Sam`
let newStr = str.match(/[a-zA-Z]/g)  //  ["H", "i", "I", "m", "S", "a", "m"]
// match 返回数组
let newStr = str.replace(/[^a-zA-Z]/g, '')  // `HiImSam` （把字符串中所有非字母字符，替换为空）
// replace 返回字符串 （替换思想）
```

​	

​	

​	

## 时间复杂度

>   其实就是举一个比较大的数组，看一下当前算法的规模

### 以「选择排序」为例

```js
// 假设数组长度为 1000 （且处于最坏情况，每次都需要对比，没有任何一次是不需要对比的）
第1次遍历，要从 [0,...,999] 找到最小值 min1（放到首位），需要对比999次
第2次遍历，要从 [1,...,999] 找到最小值 min2（放到首位），需要对比998次
第3次遍历，要从 [2,...,999] 找到最小值 min3（放到首位），需要对比997次
...
第999次遍历，要从 [998,999] 找到最小值 min999（放到首位），需要对比1次
```

+   长度为 1000 的数组，最坏情况下（每次都需要对比）  1+2+3+4+…+998+999

```js
   => 1 + 2 + 3 + 4 + … + 998 + 999
   => 1≈1000 + 2≈1000 + 3≈1000 + 4≈1000 + ... + 555≈1000 + ... + 998≈1000 + 999≈1000
   => 大概1000个1000，也就是 1000^2 
   => n^2
```

>   综上：
>
>   -   选择排序：每次找最小的
>   -   时间复杂度，是 n 的平方  （这是最坏情况）

​	

### 以「快速排序」为例

>   找一个基准数，然后左右分两队

```js
// 假设数组长度为 1000
    [0,...,500,...,999]
            ↑
     ↙            ↘        // 以500为基准，依次和 500 进行比较
[0,...,500) , [500,...,999] // 左边全是比500小的，比较了 500 次。右边全是比500大的，比较了 500 次

// 然后 [0,...,500) 中再找一个基准数 250
       [0,...,250,...,500)
               ↑
            ↙   ↘
         250      250  // 左边是比250小的，比较了250次。右边是比250大的，比较了250次，以此类推

// 得出图示 👇
       [0,...,250,...,500)      [500,...,999]       |    => 500 * 2 = 1000 次
               ↑                      ↑             | 
            ↙   ↘                 ↙   ↘          | 
         250      250            250     250        |    => 250 * 4 = 1000 次
        ↙ ↘     ↙ ↘          ↙ ↘    ↙ ↘       |
      125  125  125  125      125  125  125  125    |    => 125 * 8 = 1000 次
             ...                      ...                        ...

/* 
	虽然每一排比较次数都是 1000 次，但是减少的速度特别快（每次折半） 
	看 1000 能除以多少次 2，就知道这个树形图，能分裂出多少个 1000 次
	1000 ≈ 1024 = 2^10，1000大概是2的10次方，所以最多除以10次。
	如果把树形图比喻成塔，塔最高10层，每层1000次】
*/

	 复杂度：
=> 1000 * 10层   // log(2)1000 ≈ 10层  (2^10=1024)
=> N * log(2)N 
```

>   【快速排序】的时间复杂度，是 N * log(2)N 

​		

>   基本上画出 4 步的图示，就能找到规律，无需花费更多精力来记忆

​	

​	

### 以「归并排序」为例

>   思路：每次对半分。对排好序的两个数组，进行合并

```js
// 假设数组长度为 1000
            [0,...,999]
              ↙    ↘    // 拆分成左右两部分，各操作 1 次（共2次）
merge([0,...,500], [500,...,999])   // merge合并：分别用左边500个数和右边500个数做对比（共1000次操作）
        ↙ ↘            ↙ ↘  // 拆分成左右两部分，各操作 1 次（共4次）
      250    250      250    250   // merge：拆分成两部分，每部分250个数，逐个对比（共1000次操作） 
     ↙↘    ↙↘      ↙↘    ↙↘
   125 125 125 125  125 125 125 125  // merge：拆分，每部分125个数，逐个对比（共1000次操作）
                  ...
// 拆分到，每个数组只有一个元素，就停止：[0]，[1]，[2]...[998]，[999]
// 问题就转换为：1000个数对半拆分，需要多少次就能拆分成1个1个的。 2^10 = 1024
// 所以拆分10次就会停止，得到每个数组只有一个元素
// 所以，需要10次拆分，每次拆分需要执行1000次合并操作
                  
   复杂度：
=> 1000次合并 * 10次拆分   // log(2)1000 ≈ 10次拆分   (2^10=1024)
=> 1000 * log(2)1000
=> N * log(2)N                  
```

例

```js
   merge([1,4,7],[2,5,9]) 
=> [1,4,7], [2,5,9]  
=> [1,2,4,5,7,9]    // 6个元素需要对比5次 / 1000个元素需要对比999次
=> 四舍五入就是 n 个元素需要对比 n 次
```

​	

>   「归并排序」的 时间复杂度：与快速排序一致  N * log(2)N

​	

​	

### 以「计数排序」为例

>   思路：每次对半分。对排好序的两个数组，进行合并

```js
// 假设数组长度为 1000
   [0,...,999]
=> 对数组进行一次遍历：得出 计数的【哈希表】hashTable ，并找出最大值（假设max是100）  （1000次操作）
=> hashTable 里就添加了1000个数据
=> 遍历 min(0) ~ max（100），对应哈希表，发现相同值，就push输出 （100次操作）  
                  
   复杂度：
=> 1000 + 100  // 元素在0~100之间
=> n + max   
// 如果哈希表有最小值、最大值，如元素在50~100之间，则应该再减去min（只需比较 50 次）
=> n + max - min (默认min是0)
```

>   「计数排序」的 时间复杂度： n + max - min (默认min是0)

​	

>   基本上画出 4 步的图示，就能找到规律，无需花费更多精力来记忆
>
>   不需要做特别精细的分析，只需要看衰减的规律即可

​	

​	

​	

## 算法学习总结

### 心法

-   战略上藐视敌人：思想上暗示自己算法是特别简单的东西
-   战术上重视敌人：在真正写代码时，要非常重视每一个细节  +1  -1    <   <=   … （细节难以确定时，写写画画逐步分析）

### 特点

-   思路都很简单
-   细节都很多
    -   不需要多强的智力，需要的是耐心、细心
-   **多画表，多画图，多 log **
-   如果实在不想陷入 JS 的细节，可以用「伪代码」

​	

​	

​	

## 还有哪些排序算法 ⁉️

冒泡排序 https://visualgo.net/zh/sorting   点击 BUB    （visualgo只提供伪代码思路参考）

插入排序 https://visualgo.net/zh/sorting 点击 INS 

希尔排序 http://sorting.at/  自己选择 Shell Sort 

基数排序 https://visualgo.net/zh/sorting  点击 RAD

<img src="https://i.loli.net/2020/10/17/2oEFuvt6h4gCS1n.png" alt="image-20201017230048582" style="zoom:80%;" />

<img src="https://i.loli.net/2020/10/17/WBvQIe2P5AlkKMt.png" alt="image-20201017230300167" style="zoom: 50%;" />



### 冒泡排序 bubble sort

>   最 low 的排序。[思路](https://zhuanlan.zhihu.com/p/45501356)

>   https://visualgo.net/zh/sorting    （查看图示 + 伪代码）

两两对比，较大的往后接着对比。

每一轮找出一个最大值，冒泡到最后

```js
let bubbleSort = arr => {
  for(let i = 0; i < arr.length - 1; i++){    // i 代表轮次（两两比较）
    for(let j = 0; j < arr.length - i; j++){  // j 代表当前轮选中元素的下标
      if(arr[j] > arr[j+1]){
        [arr[j], arr[j+1]] = [arr[j+1], arr[j]]  // 交换元素
      }
    }
  }
  return arr
}
```

<img src="https://i.loli.net/2020/10/18/ZLokT13XRFIMNOC.gif" alt="冒泡排序" style="zoom:67%;" />

​	

### 插入排序 Insertion Sort

>   参考扑克牌思路，很好理解。  [思路](https://zhuanlan.zhihu.com/p/45638675)

>   扑克牌思路：大部分人抓完牌，手上拿着的牌就已经都是排好序的。

>   https://visualgo.net/zh/sorting 点击 INS   （查看图示 + 伪代码）

拿起一张牌，依次和前面的牌对比（所以起始值从下标为 1 的元素开始，才能保证前面有值可对比）

比前面的小，就插入到前面去

<img src="https://i.loli.net/2020/10/18/2cF5Xo8ERxI4fWz.gif" alt="插入排序" style="zoom:50%;" />

#### 思路

-   **从第一个元素开始，该元素可以认为已经被排序 **
-   取出下一个元素，**在已经排序的元素序列中从后向前扫描 **
-   把取出的元素放到已排序的元素中间的合适位置
-   重复步骤2~3

就像排队一样，依次每次挑一个同学，把该同学“插入”到已经排好的部分队伍里。

​	

#### 代码

+   开局默认第一个元素（前面元素）是已经排好序的。
+   取出下一个待排序元素，与前面已排好序的元素进行比较
+   如果后面的元素小于前面已排好序的某个元素，就把后面元素插入到前面已排好序的元素的**相应位置 **

```js
// 插入法JS版
function insertionSort(arr) {
  // 开局默认下标0的元素已排序，所以待排序数组的下标取值从1开始
  for(let i = 1; i < arr.length; i++) {
    // i 表示当前待排序数组元素的下标
    // j 表示当前已排序数组元素的下标（默认下标0的元素已排序,所以 j 初始值一定为0）
    // 已排序元素始终在待排序元素的前面，所以 j 的取值一定小于 i
    // 综上 j = [0,i)
    for(let j = 0; j < i; j++) { 
      // 当前取出的待排序元素arr[i]，依次和前面已排序元素进行比较
      if(arr[i] < arr[j]) {  
        arr.splice(j, 0, arr[i])  // 在 arr[j] 前面插入 arr[i]，然后把原本的 arr[i] 删除
        arr.splice(i+1, 1) // 因为上一步已经在前面插入一个元素，导致后面元素下标后移一位，原本需要被删除 i 位置上的元素，现在的下标变成了 i+1
        break  // 跳出内层循环，i++
      }
    }
  }
}
let arr = [10, 34, 21, 47, 3, 28]
insertionSort(arr)
console.log(arr)
```

插入法普通版 for

```js
function insertionSort(arr) {
  for(var i = 1; i < arr.length; i++) {
    var temp = arr[i]
    for(var j = i; j > 0 && arr[j-1] > temp; j--) {
      arr[j] = arr[j-1]
    }
    arr[j] = temp
  }
}
-----------------------------------------------------
function insertionSort(arr) {
  for (var i = 1; i < arr.length; i++) {
    var temp = arr[i];
    for (var j = i - 1; j >= 0; j--) {
      if (arr[j] > temp) {
        arr[j + 1] = arr[j];
      } else {
        break;
      }
    }
    arr[j + 1] = temp;
  }
  return arr;
}
-------------------------------------------------------
let arr = [10, 34, 21, 47, 3, 28]
insertionSort(arr)
console.log(arr)
```

插入法普通版 while

```js
function insertionSort(arr) {
  for (var i = 1; i < arr.length; i++) {
    var temp = arr[i];
    var j = i - 1;
    while (j >= 0 && arr[j] > temp) {
      arr[j + 1] = arr[j];
      j--;
    }
    arr[j + 1] = temp;
  }
  return arr;
}
let arr = [10, 34, 21, 47, 3, 28]
insertionSort(arr)
console.log(arr)
```



​	

### 希尔排序 Shell Sort

>   极其少见
>
>   算法应该是比较复杂的，是生想出来的。现实生活中没有可参考的例子、数学中也没有例子

>   1959年，一个叫 Shell 的人发明的

貌似是间隔着排，先大间隔、然后中间间隔 …  可能会节省一些中间的步骤

​	

### 基数排序 Radix Sort

>   特别适合用于**多位数排序 **
>
>   +   指未排序数组中的元素，有一位数得、也有两位数、三位数、四位数、五位数的 … （形式多样的数组）

>   死记硬背，顺序非常重要，记错了就完了（但是可以理解这个算法的精神 👇 ）

![image-20201017233654544](https://i.loli.net/2020/10/17/clBkPyNVCHuzg2n.png)

+   先根据个位数排序，个位是 0 的从下往上叠在一起，个位是 1 的从下往上叠在一起 … 
    +   然后**按照个位 0-9 堆叠的从下往上的顺序**（这个顺序非常重要）展开所有元素，成一个数组
+   对这个新数组，根据十位数进行排序，十位数 0-9 从下往上堆叠。
    +   然后再按0-9从下往上的顺序展开成数组
+   …
+   所有位数都重复上述操作
+   最后展开的数组，就是排完序的数组

<img src="https://i.loli.net/2020/10/18/1K4BkxOvnt6dl3E.gif" alt="基数排序" style="zoom: 67%;" />

​	

​	

### 堆排序 Heap Sort

-   堆排序应该是排序的终点了，因为没有比堆排序更复杂的排序了
-   其他复杂的排序，基本都是在堆排序的基础上进行改进而已
-   搞定了堆排序，就搞定了**「树」**这个数据结构，就搞定了排序的最难的一部分
