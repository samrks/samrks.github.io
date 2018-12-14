# JS 函数的执行时机 🚀⌚


<!--more-->

​	

##  JS 函数的执行时机

>   执行时机，或说调用时机，是 JS 函数的要素之一

>   先抛出结论：JS 函数的执行时机不同，结果不同

下面逐例分析：函数调用执行的时机，是如何影响结果的

​	

### 例1

```js
let a = 1
function fn(){
  console.log(a)
}
```

输出：[未知]

分析：函数只有被调用才会执行。上述代码中的函数 fn 并未被调用，输出更无从说起。

​	

### 例2

```js
let a = 1
function fn(){
  console.log(a)
}
fn()
```

输出：1

​	

### 例3

```js
let a = 1
function fn(){
  console.log(a)   
  // 看到这可能会认为是打印 1
  // 但此时函数未被调用，可以忽略整个函数。不要提前把 a=1 带入到函数中做记号
}
a = 2 // a变成2
fn()
```

输出：2

​	

### 例4

```js
let a = 1
function fn(){
  console.log(a)
}
fn()   // 看时机：函数被调用，此时 a=1
a = 2
```

输出：1

​	

### 例5

```js
let a = 1
function fn(){
  setTimeout(() => {
    console.log(a)
  },0)
}
fn()
a = 2
```

输出：2

分析：

+   setTimeout 口语化理解相当于“过一会”、“尽快”（意思是忙完当前手头事，就立马执行里面的语句）
+   举个栗子：
    +   你在打游戏（运行这段代码）
    +   别人叫你去吃饭（`fn()`被调用） 
    +   你说马上去”（遇到 setTimeout ）
    +   “马上去” 的潜台词，是要先把这局游戏打完才去😜（继续向下执行` a=2`）
    +   现在打完了（其他代码全部走完）
    +   可以去吃饭了 （`console.log(a)`）

​	

​	

### 例6 💡

```js
let i = 0
for(i = 0; i<6; i++){  
  setTimeout(() => { 
    console.log(i)  
  },0)
}
```

输出：6  6  6  6  6  6

分析：

+   首先，`let` 声明一个 `i `。 `let i` 写在 for 循环外，所以  ` let i`（声明 i）不会参与到循环中。始终只有一个 `i` 

+   开始 for 循环，逐步拆分 ↓

    ```js
    i=0 （i<6）
    遇到 setTimeout（执行完其他代码，再执行这里 setTimeout 中的语句）—— stand by ①
    i++  =>  i=1 （i<6）
    遇到 setTimeout —— stand by ②
    i++  =>  i=2 （i<6） 
    遇到 setTimeout —— stand by ③
    i++  =>  i=3 （i<6）
    遇到 setTimeout —— stand by ④
    i++  =>  i=4 （i<6）
    遇到 setTimeout —— stand by ⑤
    i++  =>  i=5 （i<6）
    遇到 setTimeout —— stand by ⑥
    i++  =>  i=6  不符合条件 i<6。 循环结束。
    stand by 完毕，开始依次执行 ①②③④⑤⑥，此时 i=6
    console.log(i)  // 6
    console.log(i)  // 6
    console.log(i)  // 6
    console.log(i)  // 6
    console.log(i)  // 6
    console.log(i)  // 6
    ```

​	

#### 变形

```js
for(var i = 0; i<6; i++){  // 相当于只有一个 i
  setTimeout(()=>{
    console.log(i)  
  },0)
}
// 6 6 6 6 6 6
```

​		

​	

### 例7 💡

```js
for(let i = 0; i<6; i++){ // 相当于在每一次循环的代码块中各声明了一个i。一共声明了6个i
  setTimeout(()=>{ 
    console.log(i)
  },0)
}
```

输出： 0  1  2  3  4  5

分析：

+   for 和 let  连用，可以近似的理解为：每轮循环创建了一个作用域 {  }

+   let 特性，仅在当前作用域有效

    ```js
    {
    	let i = 0， 满足 i < 6
    	setTimeout(() => console.log(i), 0)  // stand by ①
    }
    {
    	let i = 1， 满足 i < 6
    	setTimeout(() => console.log(i), 0)  // stand by ②
    }
    ...
    {
    	let i = 5， 满足 i < 6
    	setTimeout(() => console.log(i), 0)  // stand by ⑥
    }
    {
    	let i = 6， 不满足 i<6 ，循环结束
    }
    // 此时 stand by 完毕，开始依次执行 ①②③④⑤⑥
    // 因为 let 存在作用域限制，所以每个 log 打印的都是其所在作用域的 i
    {
      console.log(i) // 0
    }
    {
      console.log(i) // 1
    }
    ...
    {
      console.log(i) // 5
    }
    ```

​	

#### 变形1

```js
for (var i=0; i<6; i++){
  ! function(j){
    setTimeout(function(){
      console.log(j)
    }, 0)
  }(i)
}
```

输出：0  1  2  3  4  5

分析：利用「立即执行函数」的参数，保存下 i 值

​	

#### 变形2

```js
for (var i=0; i<6; i++) {
  setTimeout((j) => {
    console.log(j)
  }, 0, i)
}
```

输出：0  1  2  3  4  5

分析：利用 setTimeout 的第 3 个参数，保存下 i 值。一旦定时器到期，会作为参数传递给 function








