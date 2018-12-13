# JS 数组


「数组的基本方法」「伪数组」「数组的增删改查」「索引越界！」 <!--more-->

​	

## 数组对象

>   一种特殊的对象

## JS 其实没有真正的数组

>   只是用对象去模拟数组

其他语言、基于底层的语言，比如 C 语言，会告诉你【数组是连续的内存分配】

## JS 数组不是典型数组

### 典型的数组

-   元素的数据类型相同

-   使用连续的内存存储

-   通过数字下标获取元素

    <img src="https://i.loli.net/2020/09/09/LwjU9cQGYxJ2pIu.png" alt="image-20200909185250767" style="zoom: 67%;" />

### 但 JS 的数组不这样

>   JS 的数组，实际上是用 key / value 模拟的，本质是对象

 <img src="https://i.loli.net/2020/09/09/L4CnFMADp1UZy9j.png" alt="image-20200909185229921" style="zoom: 67%;" />

-   元素的数据类型可以不同

-   内存不一定是连续的（对象是随机存储的）

-   不支持数字下标，而是通过**字符串下标**

    -   这意味着数组可以有任何 key

    -   比如 

        ```js
        let arr = [1,2,3]
        Object.keys(arr)  // ["0", "1", "2"]
        arr['xxx'] = 'xxx'   // 下标为'xxx'，值为'xxx'
        arr.yyy = 'yyy'
        ```

    <img src="https://i.loli.net/2020/09/09/pvBV85qdohkwCXt.png" alt="image-20200909190244384" style="zoom:67%;" />



## 创建一个数组 💡

### 新建

```js
let arr = [1,2,3]           // 简写
let arr = new Array(1,2,3)  // 正规：方便理解原理
let arr = new Array(3)      // [empty × 3] 数组为空，但 length为3
```

<img src="https://i.loli.net/2020/09/16/rgz8kMomTPKeqvB.png" alt="image-20200909190739086" style="zoom:67%;" />



### 转化（ Array.from 用字符串创建数组）

```js
let arr = '1,2,3'.split(',')  //  ["1", "2", "3"]
let arr = '123'.split('')  
Array.from('123')    // 最新的ES标准提供的api：会把不是数组的东西，尝试变成数组
```

例

```js
Array.from(123)  // []
Array.from(true) // []
Array.from({name: 'sam'}) // []
Array.from({0:'a', 1:'b', 2:'c'}) // []
Array.from({0:'a', 1:'b', 2:'c', length: 3}) // ["a", "b", "c"]
```

总结：`Array.from()` 只在符合以下条件的情况下，才能**把对象转换成数组**

-   条件1 ：这个对象需要有 0,1,2,3… 这种形式的属性，符合数组的下标
-   条件2： 这个对象需要有 length 属性

>   满足以上条件，`Array.from(xxx)  ` 就可以尝试把 一个对象 转化成 真正的数组



#### 变态情况

1.  前面提到：JS 的数组 不支持数字下标，而是通过**字符串下标**。
    那这里要转化的对象`{0:'a', 1:'b', 2:'c'}`的 key 值 0,1,2 都是数字，为什么可以被识别？

    >   答：当 JS 发现 0,1,2 是数字格式，会先自动调用  ` (1).toString()` 方法，把数字全部转化为 字符串

2.   对象中的下标为 0,1,2  但 length 不为 3  的情况

     <img src="https://i.loli.net/2020/09/09/UHFVzoadMnNmTAL.png" alt="image-20200909193047308" style="zoom: 67%;" />

     >   答：会以 length 为主，有多余的就删，不足就补 undefined。总之，length 值不变

3.  对象中的下标为乱序

    <img src="https://i.loli.net/2020/09/09/qZGQfSHPaLI8WkM.png" alt="image-20200909194144196" style="zoom: 80%;" />

    >   答：不会依照下标的乱序来转换数组，而是**自动调整为顺序**排列内容



### 伪数组

+   伪数组的原型链中并**没有数组的原型**
+   换言之，如果一个数组，不具有数组的共有属性（push、pop…等方法），那它就是一个伪数组
+   可以通过 `Array.from()` 变成真正的数组

下面的是【真数组】

```js
arr = [1,2,3,4]  // new Array(1,2,3,4)
arr.__proto__ === Array.prototype                      // true
arr.__proto__.__proto__ === Object.prototype           // true
arr.__proto__.__proto__ === Array.prototype.__proto__  // true
```

下面的是【伪数组】

```html
<div>1</div>
<div>2</div>
<div>3</div>

<script>
  let divList = document.querySelectorAll('div')  // 获取文档中所有div元素，组成一个数组
  console.log(divList) // 伪数组
  divList.push(4)  // 报错 TypeError: divList.push is not a function 伪数组不具有数组的共有属性
  
  let divArray = Array.from(divList)  // 把【伪数组】转换为【真数组】
  console.log(divArray)
</script>
```

>   divList【伪数组】  ↓↓ 

  <img src="https://i.loli.net/2020/09/09/yim4LHjaRksDtP1.png" alt="image-20200909234530344" style="zoom:67%;" />

---

>   divArray 【真数组】 ↓↓

 <img src="https://i.loli.net/2020/09/16/jILdt1VFwzRAuhg.png" alt="image-20200909235849310" style="zoom:67%;" />

>   总结：没有数组共有属性的「数组」，就是伪数组
>
>   +   不知道 JS 为什么要设计伪数组，没什么用。
>   +   如果拿到的是伪数组，尽量转换成真数组，再操作

​	

### 合并两个数组，得到新数组

>   不改变原数组

```js
let arr1 = [3,3,3], arr2 = [4,4,4]
arr1.concat(arr2)   // [3, 3, 3, 4, 4, 4]  // 返回一个新数组，原数组不改变
```



### 截取一个数组的一部分

>   不改变原数组

```js
let arr1 = [1,2,3,4,5,6]  
arr1.slice(2) // 从第3个元素开始，获得新数组 [3,4,5,6]   // 原数组 arr1 不改变
```



### 截取数组全部

>   不改变原数组

-   相当于把原数组复制了一遍
-   slice(0)  经常用于**复制数组**。JS 并没有提供专门的复制数组的方法，所以要实现复制效果，就使用 slice(0)

```js
let arr1 = [1,2,3,4,5,6]
let arr2 = arr1.slice(0) // 全部截取，赋给arr2
```

>   注意，所以 JS 原生提供的，都是【浅拷贝】，没有深拷贝
>
>   深拷贝在【押题】中讲解





## 数组的增删改查 ❤️

### 删元素

#### 🙁 跟对象一样

```js
let arr = ['a','b','c']
delete arr['0']  // arr[0] 可不加引号，此时js会自动加引号
arr  // [empty, 'b', 'c']    // 变成【稀疏数组】
```

 <img src="https://i.loli.net/2020/09/11/CLlgDfeTjXp45EA.png" alt="image-20200911140518100" style="zoom:67%;" />

```js
delete arr[1]
delete arr[2]
arr // [empty × 3]      // 变成【稀疏数组】
```

 <img src="https://i.loli.net/2020/09/11/EiaCWkvlwBNKsLG.png" alt="image-20200911140713367" style="zoom:67%;" />

>   神奇，即使把元素全部 delete ，数组的长度也没有变

​	

##### 稀疏数组

>   稀疏数组：只有长度 length，但没有对应的下标，这种数组就是稀疏数组
>
>   （感兴趣可以取了解一下，稀疏数组没什么好处，倒是有很多 bug）

例 1

```js
let arr = [1,2,3]   
arr[3] = 4   // 数组原本3个元素，下标0,1,2   // 现在添加第4个元素、下标为3
arr // [1,2,3,4]  // 仍是普通数组，没有异常

arr[100] = 101  // 数组原本4个元素，下标0,1,2,3，现在跳级直接添加下标为100的元素
arr // [1, 2, 3, 4, empty × 96, 101]  // 如果跳级添加下标，会导致数组变成【稀疏数组】// length: 101
```

<img src="https://i.loli.net/2020/09/11/vdE3DBLiFbVCmzo.png" alt="image-20200911202028919" style="zoom:67%;" />

例 2

```js
let arr = [1,2,3]
delete arr[1]
arr // [1,empty,3]  length: 3    // 使用 delete 删除数组元素，也会导致【稀疏数组】
```

<img src="https://i.loli.net/2020/09/11/5ZfoIiLpnPVKGEY.png" alt="image-20200911204943966" style="zoom:67%;" />

​	

#### 🙁 如果直接改 length 可以删元素吗

```js
let arr = [1,2,3,4,5]
arr.length = 1  
arr // [1]
```

 <img src="https://i.loli.net/2020/09/11/zOqHmASX64RioIs.png" alt="image-20200911141223421" style="zoom:67%;" />

-   我X，居然可以？！
-   JS 真神奇
-   **重要忠告**：不要随便改 length



>   🈲 尽量不要使用 delete 和 改 length 的方式，删除数组元素

​	

#### 😍 推荐 3 种 API

>   一个对象提供的函数，就叫做 API 。书写形式： `对象.方法名()` 

##### 删除头部的元素

```js
arr.shift() // arr 被修改，并返回被删元素
```

##### 删除尾部的元素

```js
arr.pop() // arr 被修改，并返回被删元素
```

##### 删除中间的元素（允许增加元素）

```js
arr.splice(index)               // 从下标为 index 的位置开始删除后面所有元素
arr.splice(index, 3)            // 从下标为 index 的位置开始删除 3 个元素
arr.splice(index, 1)            // 从下标为 index 的位置开始删除 1 个元素
arr.splice(-index, 3)           // 从倒数第 index 位开始删除 3 个元素（index为负时，不表示下标）
arr.splice(index, 1, 'x')       // 并在删除位置添加 'x'
arr.splice(index, 1, 'x', 'y')  // 并在删除位置添加 'x'，'y'
```

+    [splice MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)   功能非常强大



##### 示例

```js
let arr = [1,2,3]
arr.shift()  // 1    // arr.shift() 删除第一个元素，返回被删除元素
arr // [2,3]
```

<img src="https://i.loli.net/2020/09/11/ScLXmxnqrwU3F4T.png" alt="image-20200911142458067" style="zoom:67%;" />

```js
let arr = [1,2,3]
arr.pop() // 3     // arr.pop() 删除最后一个元素，返回被删除元素
arr // [1,2]
```

<img src="https://i.loli.net/2020/09/11/OPdyEGeqILhiVJm.png" alt="image-20200911142654272" style="zoom:67%;" />

```js
let arr = [1,2,3,4,5,6,7]
arr.splice(2,1) // [3]     // arr.slice(2,1) 在下标为2的位置删除1个元素，返回被删除元素组成的数组
arr // [1,2,4,5,6,7]
```

<img src="https://i.loli.net/2020/09/11/C4yLNmfHTQDEGd5.png" alt="image-20200911161346168" style="zoom:67%;" />

```js
let arr = [1,2,3,4,5,6,7]
arr.splice(2,3) // [3,4,5]     // 从下标2开始删除3个元素，返回被删除元素组成的数组
arr // [1,2,6,7]
```

<img src="https://i.loli.net/2020/09/11/rGLqo2wQA8SOaUZ.png" alt="image-20200911161904377" style="zoom:67%;" />

```js
let arr = [1,2,3,4,5,6,7]
arr.splice(2,3,0)      // [3,4,5]  // 从下标2开始删除3个元素，添加元素0。返回被删除元素组成的数组
arr                    // [1,2,0,6,7]
arr.splice(2,1,3,4,5)  // [0]      // 从下标2开始删除1个元素，添加元素3,4,5。返回被删除元素组成的数组
arr                    // [1,2,3,4,5,6,7]
arr.splice(-1,1,777)   // [7]      // 从倒数第1位删除1个元素，添加元素777。返回被删除元素组成的数组
```

<img src="https://i.loli.net/2020/09/11/bMUgNwFE9hIYiuo.png" alt="image-20200911163912672" style="zoom:67%;" />

​	

​	

###  查看所有元素

>   遍历

#### 🙁 查看所有属性名 

```js
let arr = [1,2,3,4,5]
arr.x = 'xxx' 

Object.keys(arr)   // 遍历属性名
Object.values(arr) // 遍历属性值
for(let key in arr){ console.log(`${key}: ${arr[key]}`) }  // for-in 更适用于遍历对象
```

>   Object.keys 、 Object.values、for…in… 这 3 种方法，都更适用于遍历对象，不适合遍历数组
>
>   +   通常我们遍历数组，只希望查看对应正常顺序的下标的元素。
>   +   如果遍历数组，获取到一个下标为 'x' 的元素，可能是很奇怪的😱（如下图）
>   +   所以上述三种方式，虽然可以用，但**不是**最适合/最常见的遍历数组的方式

<img src="https://i.loli.net/2020/09/11/I9P3AHVaoGLns5r.png" alt="image-20200911171202022" style="zoom:67%;" />

​	 

#### 😍 查看数字（字符串）属性名和值

##### for 循环

>   +   用 for 循环遍历（可以自己控制下标）
>
>   +   让下标  `i`  从 `0` 增长到 `length-1 `

```js
let arr = [1,2,3,4,5]
arr.x = 'xxx'

for(let i=0; i<arr.length; i++){ 
  console.log(`${i}: ${arr[i]}`) 
}
```

<img src="https://i.loli.net/2020/09/11/gqvSKQzCmRE1OU4.png" alt="image-20200911172024995" style="zoom: 67%;" />

+   **for 循环，是访问数组的比较常见的形式**
+   for…in… 用来访问对象



##### forEach

>   forEach 接收一个函数作为参数。这种函数，称为回调函数
>
>   -   forEach 的作用：就是遍历数组的每一项，**访问到每一个元素都执行一遍回调函数**。
>
>   -   该回调函数，默认参数
>
>       -   【第1个形参】表示元素本身，通常写作 item 
>       -   【第2个形参】表示当前下标，通常写作 index
>       -   【第3个形参】表示数组本身，通常写作 array （大部分情况用不到）
>
>       形参的名称无所谓，顺序是关键

**例 1**

```js
let arr = [1,2,3,4,5]
arr.forEach(function(item, index){ // 回调函数的默认第1个形参item表示元素本身、第2个形参index表示当前下标
  console.log(`${index}: ${item}`)
})
```

+   也可以用 forEach/map 等原型上的函数

    <img src="https://i.loli.net/2020/09/11/zxrMyUu7FSOdq6j.png" alt="image-20200911173038869" style="zoom:67%;" />

###### 手写 forEach

>   为了更好地理解上述代码（forEach）是怎么实现遍历数组的
>   下面手动封装一个 forEach 函数

```js
function forEach(array, fn){
  for(let i = 0; i<array.length; i++){  // 访问传入的 array 的每一项，对每项执行什么操作呢？↓
    fn(array[i], i, array)  // 对每一项调用 fn，把数组的相关值作为参数传入到 fn 中
  }
}
```

-   forEach 用 for 访问 array 的每一项
-   对每一项调用 `fn(array[i], i, array)`。数组有几项，fn 就执行几次
-   为什么要传入 array 呢？不为什么，规定如此。

**例 2**

```js
let arr = ['a','b','c']
forEach(arr, function(x,y,z){
  console.log(x,y,z)
})
```

<img src="https://i.loli.net/2020/09/11/Vm5hNDX2Jxz19wi.png" alt="image-20200911175532430" style="zoom:67%;" />

-   注意：例 2 是 `forEach(arr,function)`的形式 ，例 1 是 `arr.forEach(function)` 的形式。
-   两种写法其实是等价的。因为 JS 会自动把顺序倒过来，按照 例2  的形式执行



##### 面试问：区别是什么

>   两者区别，回答的关键点就是：**前者能实现、而后者做不到的**

+   实际上大多数情况，for 和 forEach 都是通用的
+   只有一种情况
    +   for 循环里面有 break 和 continue，而 forEach 是不支持的
        forEach 只是个普通函数，而 for 是个 关键字，关键字的功能可能会更强大一点

示例

```js
let arr = [1,2,3,4,5,6,7,8]
for(let i=0; i<arr.length; i++){
  console.log(`${i}: ${arr[i]}`)
  if(i===3){break;}
}
-----------------------------------------------------
let arr = [1,2,3,4,5,6,7,8]
arr.forEach(function(item, i){
  console.log(item)
  if(i===3){return false}  // 无效
})
```

<img src="https://i.loli.net/2020/09/11/nBW2EFRGiOMr4v5.png" alt="image-20200911181705734" style="zoom:67%;" />

**⭐️区别：**

1.  for 循环中间，可以 break 或 continue。而 forEach 一旦开始就会一直走到尾，即使 return 也无法结束
2.  for 是关键字，{  } 是块级作用域。而 forEach 是一个函数，{  } 是函数作用域



​	

​	

### 查看单个属性

#### 跟对象一样 

```js
let arr = [111,222,333] 
arr[0]
```

注意：不论给下标什么数字，一定都会变成字符串。JS 中没有「数字下标」这么一说

​	

#### 索引越界 ⭐️

>   「索引越界」指给的数组索引（下标）不存在

下述两种情况，需要注意避免

```js
arr[arr.length] === undefined 
arr[-1] === undefined
```

例

```js
for(let i=0; i<=arr.length; i++){  // i===length时，数组中并没有对应元素，值为 undefined
  console.1og(arr[i].toString())
}
// 报错：Cannot read property 'toString' of undefined （ undefined不是对象，没有toString ）
```

访问任何不存在的下标时，得到的结果都是 undefined 

>   当碰到报错 Cannot read property 'xxx' of undefined，很有可能是数组的索引越界了
>
>   解决办法：console.log 大法

<img src="https://i.loli.net/2020/09/11/YxgLyoprKOVJPha.png" alt="image-20200911194052958" style="zoom:50%;" />

​	

#### 查找某个元素是否在数组里

##### ❌ 方法一：遍历

>   降智写法

```js
let arr = [11,22,33,44,55]
for(let i=0; i<arr.length; i++){
  if(arr[i]===22){
    console.log('arr中存在22')
  }else{
    console.log('不存在')
  }
}
```

##### 😍 方法二：API

>   indexOf 返回只要不是 -1 ，就说明存在

```js
arr.indexOf(item)  // 存在返回下标，否则返回 -1
```

例

```js
let arr = [11,22,33,44,55]
arr.indexOf(22)  // 1
arr.indexOf(11)  // 0
arr.indexOf(66)  // -1
```

​	

#### 查找满足条件的元素

##### ❌降智写法：遍历

```js
let arr = [1,2,3,4]
for(let i=0; i<arr.length; i++){
  if(arr[i]%2===0){ console.log(`数组中的偶数：${arr[i]}`) }
}
// 数组中的偶数：2
// 数组中的偶数：4
```

##### 😍更好的方法：使用 find 

```js
arr.find(item => item%2 === 0)  // 找第一个偶数
```

完整写法

>   find 用法：找到**第一个**符合条件（return true）的元素，就停止，并返回这个元素

```js
let arr = [1,2,3,4]
arr.find(function(x){
  return x%2 === 0
})
// 2
```

​	

#### 查找满足条件的元素的索引

>   findIndex 用法：找到**第一个**符合条件（return true）的元素，就停止，并返回这个元素的下标

```js
arr.findlndex(item => item%2 === 0)  // 找第一个偶数的索引
```

例

```js
let arr = [11,22,33,44]
arr.findIndex(function(x){
  return x%2 === 0
})
// 1
```

​	

​	

### 增加数组中的元素

#### ❌ 不推荐

```js
let arr = [1,2,3]   
arr[3] = 4   // 数组原本3个元素，下标0,1,2   // 现在添加第4个元素、下标为3
arr // [1,2,3,4]  // 仍是普通数组，没有异常

arr[100] = 101  // 数组原本4个元素，下标0,1,2,3，现在跳级直接添加下标为100的元素
arr // [1, 2, 3, 4, empty × 96, 101]  // 如果跳级添加下标，会导致数组变成【稀疏数组】// length: 101
```

<img src="https://i.loli.net/2020/09/11/vdE3DBLiFbVCmzo.png" alt="image-20200911202028919" style="zoom:67%;" />

>   `arr[index]` 的方式，可以增加数组元素
>
>   但如果跳级增加下标，会导致数组变成【稀疏数组】，所以不推荐这种方式

​	

​	

#### ✅ 在尾部加元素

```js
arr.push(newItem)      // 修改 arr，返回新长度 
arr.push(item1, item2) // 修改 arr，返回新长度
```

例

```js
let arr = [1,2,3]
arr.push('a','b','c')  // 6   // 修改原数组，并返回该数组的新长度
arr // [1, 2, 3, "a", "b", "c"]

arr.push(666)
arr // [1, 2, 3, "a", "b", "c", 666]
```

<img src="https://i.loli.net/2020/09/11/brX1aSTU8y5tVx4.png" alt="image-20200911210043411" style="zoom:67%;" />

​	

#### ✅ 在头部加元素

```js
arr.unshift(newItem)      // 修改 arr，返回新长度 
arr.unshift(item1, item2)  // 修改 arr，返回新长度
```

例

```js
let arr = [1,2,3]
arr.unshift('x','y','z') // ["x", "y", "z", 1, 2, 3]
arr.unshift(777)         // [777, "x", "y", "z", 1, 2, 3]
```

<img src="https://i.loli.net/2020/09/11/eXJwHjhYCqEMTBN.png" alt="image-20200911210353301" style="zoom:67%;" />

​	

#### ✅ 在中间添加元素

>   [splice MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)  功能强大
>
>   前面讲，可以用 splice 来[删除指定位置的元素](# 删除中间的元素（允许增加元素）)，这里用来[往指定位置添加元素]()

```js
arr.splice(index, 0, 'x')      // 在 index 处插入'x'，原元素向后移一位  // 第2个参数写 0 表示不删除元素
arr.splice(index, 0, 'x', 'y')
```

例

```js
let arr = [1,2,3]
arr.splice(1,0,666)   // []   // 返回被删除元素组成的数组，0表示不删除元素，所以返回空数组
arr // [1, 666, 2, 3]
```

<img src="https://i.loli.net/2020/09/11/tzx81W4aqw7bvXA.png" alt="image-20200911210947513" style="zoom:67%;" />

>   splice：可以删除元素、添加元素 （会用 splice 基本可以对数组进行任何操作）

​	

​	

### 修改数组中的元素

#### 直接修改

例

```js
let arr = [1,2,3]
arr[1] = 0
arr // [1,0,3]
```

例

```js
let arr = [1,2,3]
arr.splice(1,1,0)  // [2]    // 把下标为1的元素删除，插入一个0  // 相当于修改元素
arr // [1,0,3]
```

​		

#### 反转顺序 reverse

```js
arr.reverse() // 修改原数组
```

例

```js
let arr = [1,2,3]
arr.reverse()  // [3,2,1]
arr            // [3,2,1] 
```

##### 著名面试题

>   翻转字符串

```js
let s = 'abcde'
s.split('')                     // ["a", "b", "c", "d", "e"]
s.split('').reverse()           // ["e", "d", "c", "b", "a"]
s.split('').reverse().join('')  // "edcba"
```

​	

#### 自定义顺序 sort

>   把乱序，改为【从大到小】或【从小到大】的顺序

```js
arr.sort((a,b) => a-b)    // 会改变原数组
```



##### 从小到大

```js
let arr = [5,2,4,3,1]
arr.sort()   // [1,2,3,4,5]  // sort 默认【从小到大】排序
```

##### 从大到小怎么实现 ？

```js
// sort 接收一个回调
let arr = [3,5,2,4,1]
arr.sort(function(a,b){   // 两个形参，表示每次进行两两比较的两个数值
  if(a>b){
    return 1
  }else if(a===b){
    return 0
  }else{
    return -1
  }
})  
// [1, 2, 3, 4, 5]
----------------------------------------------------------------------------------
arr.sort(function(a,b){
  if(a>b){
    return -1
  }else if(a===b){
    return 0
  }else{
    return 1
  }
})
// [5, 4, 3, 2, 1]
```



##### 例

```js
let arr = [
  {name:'Kate', score: 99}, {name:'Jack', score: 85}, {name:'Ryan', score: 100}
]
```

需求：根据每一项的 score 进行排序

```js
arr.sort() // 显然无效，因为 JS 看不出要怎么比较，我们需要指定给 JS：让它用每一项的 score 来进行比较
```

怎么指定？用回调函数

```js
let arr = [
  {name:'Kate', score: 99}, {name:'Jack', score: 85}, {name:'Ryan', score: 100}
]
arr.sort(function(a,b){  // a/b代表两两比较的数组元素（也就是3个对象）
  if(a.score > b.score){return 1}
  else if(a.score === b.score){return 0}
  else{return -1}
})
```

<img src="https://i.loli.net/2020/09/11/xTQS7NZazk9vcG8.png" alt="image-20200911220038076" style="zoom: 67%;" />



##### 简化写法

```js
let arr = [5,2,4,3,1]
arr.sort(function(a,b){
  return a-b // a>b返回一个正数，a<b返回负数，a===b返回0
})
// [1, 2, 3, 4, 5]
-------------------------------------------------
let arr = [5,2,4,3,1]
arr.sort(function(a,b){
  return b-a  // b>a返回一个正数，b<a返回负数，a===b返回0
})
// [5, 4, 3, 2, 1]
```

==> 箭头函数，进一步简化

```js
arr.sort((a,b)=>a-b)  // 从小到大
arr.sort((a,b)=>b-a)  // 从大到小
```

```js
arr.sort((a,b) => a.score - b.score)  // 从小到大
arr.sort((a,b) => b.score - a.score)  // 从大到小
```

​		

​	

​	

## 数组变换 ⚡️

>   高级 API

```js
map([🐮, 🥔, 🐔, 🌽], cook)     // n变n
 => [🍔, 🍟, 🍗, 🍿]

filter([🍔, 🍟, 🍗, 🍿], isNotMeet)    // n变少
 => [🍟, 🍿]

reduce([🍔, 🍟, 🍗, 🍿], eat)    // n变1
 => 💩
```

<img src="https://i.loli.net/2020/09/11/VQ18gT6l2UtSwqK.png" alt="image-20200911221934791" style="zoom:50%;" />

​	

### map

>   n 变 n
>
>   +   ES6 新方法 map
>   +   map 会对数组进行遍历，可以对每一项都执行指定的操作（对数组元素每一项进行一一映射）
>   +   map 方法的返回值为【数组每一项经过回调处理后的返回值 组成的新数组】
>
>   +   **返回新的数组，不改变原数组**
>   +   语法： `Array.map(function(ele, index, arr){...});`  回调函数
>
>       +   参数1：遍历的元素  item
>       +   参数2：元素下标  index
>       +   参数3：原数组 array （通常用不到这个参数）

#### 例 1

```js
let arr = [1,2,3,4];
arr.map(function(ele,index,arr){
  console.log("ele ==> ", ele," index ==> ", index ," arr ==> ",arr)
}) 
// [undefined,undefined,undefined,undefined] 
```

<img src="https://i.loli.net/2020/09/11/xSYXyQrDTsPc96e.png" alt="image-20200911230953734" style="zoom:67%;" />

+   因为回调里没有 return，所以会采用函数的默认返回值 undefined，作为每一项回调处理后的返回值，并将这4个返回值组成新的数组，作为最终返回值

+   所以最后结果就是 4 个 undefined 组成的新数组

    >   总结：
    >
    >   对每一项元素进行处理的语句要写在 return 后面。
    >
    >   否则每一项处理后的返回值都是 undefined



#### 例 2

>   需求：对 arr 进行操作（每个数乘2），返回新的数组[2,4,6,8]

```js
// 以前的方法: for循环
let arr = [1,2,3,4]
let newArr = [];
for(let i = 0; i < arr.length; i++){
  newArr.push(arr[i]*2);   // 返回新数组
}
console.log(newArr)  // [2,4,6,8]
--------------------------------------------------------------
// ES6: map
let newArr = arr.map(function(ele){
  return ele*2
});

// 用箭头函数进行重构：return 的语句可以直接写在箭头后，省略'return'
let newArr = arr.map(ele => ele*2);
console.log(arr, newArr) // [1,2,3,4]  [2,4,6,8]
```



#### 例 3 ：计算每一项的平方

>   需求：把 arr 的每一项都平方  （这个也可[用 reduce 实现](# 计算每一项的平方)）

+   for 循环（旧）

    ```js
    let arr = [1, 2, 3, 4, 5, 6]
    for(let i=0; i<arr.length; i++){  
      arr[i] = arr[i] * arr[i]   // 在原数组上进行修改
    }
    arr // [1, 4, 9, 16, 25, 36]
    ```

+   使用 map 简化过程（ES6 新增）

    ```js
    let arr = [1, 2, 3, 4, 5, 6]
    arr.map(item => item * item)  // [1, 4, 9, 16, 25, 36]  返回新数组，原数组不改变
    ```



​	

​	

### filter

>   n 变少
>
>   +   用于**筛选**数组中符合条件的**全部元素**，返回的是这些元素组成的新数组
>   +   **返回新的数组，不改变原数组**

#### 区别于 find 方法

+ **find方法** 查找符合条件的**第一个元素**，并返回这个**元素**；没有符合条件的元素，返回 **undefined**
+ **filter方法** 查找符合条件的**全部元素**，并返回这些元素组成的**新数组**；没有符合条件的元素，返回**空数组`[ ]`**



#### 例 1

需求：筛选出 arr 中的偶数（也可以[用 reduce 实现](#筛选出所有偶数)）

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.filter(item => item%2 === 0 )   // [2, 4, 6]  // 把符合条件、返回true的item，筛选出来组成新数组
```

#### 例 2

需求：筛选出大于等于3的元素

```js
let arr = [1, 2, 3, 4, 5, 6]
let newArr = arr.filter((ele , index , arr) =>{
  return ele >= 3
  console.log(ele)  // 不会执行这句，因为函数中遇到 return 就返回（结束）了
})
console.log(newArr); // [3,4] 
```

需求：筛选出大于等于10的元素

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.filter( ele => ele >= 10 ) // []
```



#### 例 3 ：删除元素

>   需求：删除数组中值为 3 的元素

方法一：filter 

会把**符合条件的全部元素删除**

```js
let arr = [3,4,5,6,3,3,3,7]
arr = arr.filter(ele => ele!==3 )  // 筛选出不等于3的元素，相当于从数组中删除值为3的元素
// filter会返回新的数组，再重新赋值给原数组，相当于从原数组中删除 值为3的元素
```

方法二：findIndex + splice  

会把**符合条件的第一个元素删除**

```js
let arr = [3,4,5,6,3,3,3,7]
let index = arr.findIndex(ele => ele === 3)  // 找到下标，通过splice方法从数组中删除对应下标的元素
arr.splice(index,1) // [3]
arr // [4, 5, 6, 3, 3, 3, 7]
```

​	

​	

### reduce

>   n 变 1
>
>   +   reduce 是数组里面最难理解，也是功能最强大的 API（第二名是 splice）
>   +   reduce 可以代替 map 和 filter，只不过对新人有一定难度

#### 求数组元素之和

##### 用 for 循环实现

```js
let arr = [1,2,3,4,5]
let sum = 0  // 作为结果的初始值为0
for(let i=0;i <arr.length;i++){
  sum += arr[i]   // sum = sum + arr[i]
}
console.log(sum)  // 15
```

##### 用 reduce 来实现

```js
let arr = [1,2,3,4,5]
// arr.reduce(()=>{}, 0)  // reduce中，0作为结果的初始值，写在第2个参数上 
										     // 第1个参数是一个函数：规定每一次遍历要对上一次的结果进行什么操作
arr.reduce((total,current)=>total+current, 0)  // 15
// reduce的回调函数，默认形参1是上一次的返回值（初始0），形参2是当前元素
// 求和，就是要把上一次的结果再加上当前元素，获得二者的和，所以回调执行的就是 total+current
// total初始值为0（相当于上面for循环例子中的sum）
```

-   第1轮，遍历获取元素 1，执行回调，total为初始值0 + current是当前遍历到的元素1，回调返回0+1=> 1
-   第2轮，遍历获取元素 2，执行回调，total为上一次的返回值 1 + current：2，回调返回 1+2 => 3
-   第3轮，遍历获取元素 3，执行回调，total为上一次的返回值 3 + current：3，回调返回 3+3 => 6
-   第4轮，遍历获取元素 4，执行回调，total为上一次的返回值 6 + current：4，回调返回 6+4 => 10
-   第5轮，遍历获取元素 5，执行回调，total为上一次的返回值 10 + current：5，回调返回 10+5 => 15

##### 变形

计算数组中成员的总年龄

```js
var arr = [
  {name: "张三", age: 40},
  {name: "李四", age: 50},
  {name: "王五", age: 60}
]
arr.reduce((total, current) => {
  console.log(total, current.age) // 0 40   // 40 50   // 90 60
  return total + current.age
}, 0) 
// 150
```





#### 计算每一项的平方

##### 用 map 实现

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.map(item => item * item)  // [1, 4, 9, 16, 25, 36]  返回新数组，原数组不改变
```

##### 用 reduce 实现

```js
let arr = [1, 2, 3, 4, 5, 6] 
// arr.reduce((total,current)=>{ return total.push(current*current) }, [])  
// 报错 total.push is not a function 因为 push 的返回值并不是新的数组，而是length，所以不能用 push
// 连接两个数组，用 concat
arr.reduce((result, item)=>{ return result.concat(item * item) }, []) 
// [1, 4, 9, 16, 25, 36]
```

+   补充：concat 用法：不修改原数组，返回新数组

    ```js
    let arr = [0,1]         // [0, 1]
    arr = arr.concat(2)     // [0, 1, 2]  所以必须重新赋值给arr，才能实现push效果
    arr = arr.concat([3,4]) // [0, 1, 2, 3, 4]
    ```



​	

#### 筛选出所有偶数

##### 用 filter 实现

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.filter(item => item%2 === 0 )   // [2, 4, 6]  // 把符合条件、返回true的item，筛选出来组成新数组
```

##### 用 reduce 实现

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.reduce((result,item)=>{
  if(item%2===1){  // 如果是奇数，不做任何处理，直接返回 原result
    return result
  }else{ // 如果是偶数，就把偶数连到result中，再返回新数组，作为下一轮的result
    return result.concat(item)
  }
},[])
```

###### 简写

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.reduce((result,item)=> item % 2 === 1 ? result : result.concat(item) , [])
```

###### 再进一步探索（炫技）

```js
let arr = [1, 2, 3, 4, 5, 6]
arr.reduce((result,item)=> 
  // item % 2 === 1 ? result : result.concat(item) 
  // 原理：如果item是奇数就不concat到result中
  // ==> 换言之，若是奇数，可concat一个空 // item%2===1 ? result.concat() : result.concat(item)
  // ==> 也就是，我总是需要concat一个东西 ==> 那就可以转换为
  result.concat(item % 2 === 1 ? [] : item)    
, [])

// [2, 4, 6]
```



#### 将多维数组，转为一维数组

```js
// reduce的参数total和current也可以执行【数组合并】这样的操作

var arr = [1, 2, 3, [4], 6, [1, 2, 3, [4]]];
var fun = function(arr) {
  var newArr = arr.reduce((total, current) => {
    if (current instanceof Array) {
      //递归
      var news = fun(current); // 返回一个数组
      total = total.concat(news);
      return total // 将返回的数组拼接回total中
    } else {
      total.push(current);
      return total
    }
  }, []) //[]：表示给total赋初始值，total是一个空数组
  return newArr
}
console.log(fun(arr));
```

多维数组转为一维数组，也可以用flat方法等【方法很多，可以查看es6入门2/zuoye目录中的大家的解法】

<img src="https://i.loli.net/2020/09/12/Y48EAJj5kaqrOMo.png" alt="image-20200302180403884" style="zoom:67%;" />



---

+ reduce 叫做累加器
+ 对数组进行遍历，执行加减乘除操作等等 
+ reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。
+ 常用来【求和】
+ 语法：`array.reduce(function(total, current, currentIndex, arr), initialValue)`
    + **参数 total ： 必需。上一次循环计算结束后的return返回值；**
        + 如果回调函数没有return，则第二次循环时total为undefined
    + **参数 current **： 必需。当前的元素
    + 参数 currentIndex：可选。当前元素下标
    + 参数 arr：可选。原数组
    + **参数 initialValue**：可选。传递给函数的初始值





## 题目

### 第一题：把数字变成星期

```js
let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
let arr2 = arr.map((item,index)=>{
  if(item===0){return '周日'}
  if(item===1){return '周一'}
  if(item===2){return '周二'}
  if(item===3){return '周三'}
  if(item===4){return '周四'}
  if(item===5){return '周五'}
  if(item===6){return '周六'}
})
console.log(arr2) 
// ['周日', '周一', '周二', '周二', '周三', '周三', '周三', '周四', '周四', '周四', '周四','周六']
```

参考答案

```js
let arr = [0,1,2,2,3,3,3,4,4,4,4,6]
let arr2 = arr.map((i)=>{
  const hash = {0:'周日',1:'周一',2:'周二',3:'周三',4:'周四',5:'周五',6:'周六'}
  return hash[i]
})
console.log(arr2)
```



### 第二题：找出所有大于 60 分的成绩

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let scores2 = scores.filter(item=>item > 60)
console.log(scores2) // [95, 91, 82, 72, 85, 67, 66, 91]
```

参考答案

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let scores2 = scores.filter(n => n>= 60)
console.log(scores2) //  [95,91,82,72,85,67,66, 91]
```



### 第三题：算出所有奇数之和

```js
let arr = [95,91,59,55,42,82,72,85,67,66,55,91]
let arr2 = arr.reduce((sum,n)=>{
  if(n%2===0){return sum}
  else{return sum+n}
})
console.log(arr2)
```

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let sum = scores.reduce((sum,n)=> n%2===1 ? sum+n : sum, 0)
console.log(sum)  // 598
```

参考答案

```js
let scores = [95,91,59,55,42,82,72,85,67,66,55,91]
let sum = scores.reduce((sum, n)=>{
  return n%2===0?sum:sum+n
},0)
console.log(sum) // 奇数之和：598
```



​	

## 面试题

### 数据变换

```js
let arr = [ 
  {名称: '动物', id: 1, parent: null}, 
  {名称: '狗', id: 2, parent: 1}, 
  {名称: '猫', id: 3, parent: 1}
] 
// 数组变成对象 
{ 
  id: 1, 名称：'动物', children:[
    {id: 2, 名称：'狗', children: null},
    {id: 3, 名称：'猫', children: null}
  ]
}
```

解

test-1，先想当然的试试

```js
arr.reduce((result,item)=>{
  result[item.id] = item
  return result
}, {})
```

<img src="https://i.loli.net/2020/09/12/4AuoY3vtjL5SzQx.png" alt="image-20200912012628093" style="zoom:67%;" />

test-2，

如果在parent为null时，才往 result 添加 id，值为 item.id；parent不为空，就把 item 添加到 result.children数组中

初始化 result 把 id 和 children 加进去（注意children是数组）

```js
arr.reduce((result,item)=>{
  if(item.parent === null){
    result.id = item.id
    result['名称'] = item['名称']
  }else{
    delete item.parent
    item.children = null
    result.children.push(item)
  }
  return result
}, {id: null, children: []})
```

完。








