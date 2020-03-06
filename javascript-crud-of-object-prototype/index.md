# JS 对象的基本用法




「增」「删」「改」「查」<!--more-->



## 回顾

### 七种数据类型

-   number、string、booleansymbol
-   undefined、null
-   object

四基(本类型)两空一对象

(bigInt)



### 五个 falsy 值

+   null、undefined
+   0、NaN
+   `''` （空字符串）



​	

## JS 对象

>   是学习 JS 的三座大山之一
>
>   +   对象（原型）
>
>   +   this
>   +   AJAX

​	

##  对象 object 

>   对象的基础知识

>   object ，是第七种数据类型，唯一 一种「复杂类型」
>
>   其他六种（numbe、string、boolean、symbol、null、undefined），叫做「简单类型」。因为这六种不包含其他任何东西，而 object 对象包含其他内容

### 定义

-   无序的数据集合
-   键值对的集合

### 写法

```js
let 对象名 = {
  key: value    // 属性名/键名 :  属性值
}
```

+   对象的写法，与 block（代码块）类似，只是碰巧都有 { } 。
+   要注意区分 { }  是对象，还是代码块

例

```js
let obj = { 'name': 'sam', 'age': 18 }   // 不论'name'/'age'(属性名)是否有引号，它都只能是字符串
let obj = new Object({'name': 'sam'})    // 正规写法
console.log({ 'name': 'sam', 'age': 18 })  // 创建匿名对象
```

+   JS 既然可以通过**`字面量`**方式创建对象，为什么还要有第二种 **`new Object()`** 的方式创建 ?
    +   实际上**第二种 `new Object()` 才是正规创建对象的写法**，第一种属于简化版
    +   因为简化了代码，所以通常都是用第一种写法



​	

###  细节 

-   ==**键名是字符串**==，不是标识符，可以包含任意字符

    >   只要是**字符串**就行：空串、空格串、emoji 、数字字符串 … （任何一个 Unicode 能表达的串都 ok ）
    >
    >   标识符 规则：（变量）不能以数字开头

-   属性名的引号可省略，省略之后需按照标识符的规则命名，特例：允许纯数字的键名

-   **就算引号省略了，键名也还是字符串（重要）**

例

```js
var obj1 = { '': 1 }
var obj2 = { 2: 123, name:'fff', 'age':12 }
var obj3 = { '  ': 2 }
var obj4 = { '👍': 'zan' }


// Object.keys(对象名)  获取对象中的 key名 组成的数组

Object.keys(obj1)  // [""] // 空串也是字符串，合法
Object.keys(obj2)  // ["2", "name", "age"]
Object.keys(obj3)  // ["  "]
Object.keys(obj4)  // ["👍"]

// 所以不论怎么写，key 都是字符串
```



#### 属性名

>   每个 key 都是对象的属性名（property）

#### 属性值

>   每个 value 都是对象的属性值

​	

### 奇怪的属性名

所有属性名会自动变成字符串

```js
let obj = {
  1: 'a',       // "1"
  3.2: 'b',     // "3.2"
  1e2: true,    // "100"
  1e-2: true,   // "0.01"
  .234: true,   // "0.234"
  0xFF: true    // "255"
};
Object.keys(obj)  // ["1", "100", "255", "3.2", "0.01", "0.234"]
```

>   JS 可能会自动换算「属性名」，所以如果不想被自动换算，给属性名加上「引号」即可解决

#### 细节

-   **`Object.keys(obj)`** 可以得到 obj 的所有 key 组成的数组
-   这个 API 需要会使用



#### 「变量」作属性名  

如何用变量做属性名

-   之前都是用**常量**做属性名（所有不是变量的都是常量）

-   **`let p1 = 'name'`**

-   **`let obj = { p1 : 'sam'}`** 这样写，属性名为 **`'p1'`**

-   **`let obj = { [p1] : 'sam' }`** 这样写，属性名为 **`'name'`**    （ ES 6 ）

    ```js
    let aa = 'xxx'  // 想用变量a作为属性名
    var obj = { aa: 1111 }     // {aa: 1111}
    var obj = { 'aa': 1111 }   // {aa: 1111}
    var obj = { [aa]: 1111 }   // {xxx: 1111}  // ES6之后
    ```

    ES6之前，实现变量作属性名  ↓↓ ，需两行代码实现。ES6之后一行 ↑↑ 即可

    ```js
    let aa = 'xxx'
    var obj = {} 
    obj[aa] = 1111
    console.log(obj)  // {xxx: 1111}
    ```

对比

-   不加 [ ] 的属性名会自动变成字符串

-   加了 [ ] 则会当做变量求值

-   值如果不是字符串，则会自动变成字符串

    ```js
    var obj = {
      [1+2+3+4]: '十'
    }
    console.log(obj)  // { 10: "十" }
    Object.keys(obj)  // [ "10" ]
    ```



​	

### 对象的隐藏属性（原型 💡）

隐藏属性

-   JS 中，每一个对象 都有一个 隐藏属性  `__proto__`
-   这个隐藏属性，储存着其 **共有属性组成的对象**的地址
-   这个**共有属性组成的对象**，叫做原型
-   也就是说，隐藏属性 储存着 原型的地址
    -   `__proto__` 存储了一个地址，这个地址所代表的内存空间中的对象，叫做原型 / 共有属性

代码示例

```js
var obj = {}
obj.toString() // 居然不报错
```

因为 obj 的隐藏属性**对应的对象**（原型 / 共有属性）上有 toString()

​	

>   举个栗子：什么叫共有属性
>
>   +   将共有的属性，提取出来单独存储成一个对象。最大的好处，就是**省内存**
>   +   每次声明一个 chinese 时，无需重复写入：国籍、肤色、发色 … 等 chinese 公共的属性，直接用一个**特定属性**（–proto–），**存储**共有属性所在的**内存地址**即可

```js
var chinese1 = { name: '小兰' }
var chinese2 = { name: '小红' }
```

![image-20200831153130156](https://i.loli.net/2020/08/31/J8ItoARveuXHyKU.png)

​	

​	

### 超纲知识

>   前面提到，对象中所有的 key 都是字符串

>   实际上，ES 6 中稍微做了调整：**除了字符串，symbol 也能做属性名**

```js
let a = Symbol()
let obj = { [a]: 'Hello' }
```

这有什么用呢？

-   目前，屁用都没用，很久很久以后可能会有用（方方从没用过）
-   在学习「迭代」时会用到（但前端不流行迭代，所以根本没机会用）



​	

​	

## 增删改查

>   「增删改查」对象的属性

### 删除属性

[delete 操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete)：用于删除对象的某个属性

```js
delete obj.xxx
delete obj['xxx']
```

-   作用：删除 obj 的 xxx 属性
-   请区分「属性值为 undefined」和「不含属性名」

#### 不含属性名

直接删除属性名

``` js
var obj = {name: 'sam', age: 18}
delete obj.name  // 或 delete obj['name']
console.log(obj)  // {age: 18}
// console.log(obj.name) // undefined
```

判断是否删除成功

```js
'xxx' in obj === false   // 返回 true 说明该属性名已被删除 
（不能省略引号）
```



#### 含有属性名，但是值为 undefined

仅删除属性值，保留属性名

```js
'xxx' in obj && obj.xxx === undefined
```

```js
var obj = {name: 'sam', age: 18}
obj.name = undefined
console.log(obj)  // {name: undefined, age: 18}
// console.log(obj.name) // undefined
```



#### 注意 obj.xxx === undefined

-   `obj.xxx === undefined` 不能断定 'xxx' 是否为 obj 的属性

    ```js
    let obj = {}
    let obj2 = {x:undefined}
    obj.x === undefined // true
    obj.x === undefined // true   所以说 这句话无法判断出 x 到底是不是 obj 的属性
    ```

-   用 in 和 hasOwnProperty 可以判断

    ```js
    let obj = {}
    let obj2 = {x:undefined}
    'x' in obj   // false
    'x' in obj2  // true
    obj.hasOwnProperty('x')    // false
    obj2.hasOwnProperty('x')   // true
    ```



#### 类比

-   你有没有卫生纸？
-   A: 没有 // 不含属性名
-   B: 有，但是没带 // 含有属性名，但是值为 undefined



>   #### 程序员就是这么严谨
>
>   +   「没有」和「undefined」是两个概念
>   +   没有就是没有，undefined 就是 undefined
>   +   绝不含糊
>   +   需要细心，发现细微的区别

​	

​	

### 查看所有属性（读属性）

例

```js
var obj = {name: 'sam', age: 18}
```

#### 查看自身所有属性

>   无法打印【共有属性 `__proto__`】

```js
Object.keys(obj)     // ["name", "age"]
Object.values(obj)   // ["sam", 18] 
Object.entries(obj)  // [Array(2), Array(2)]  => 0:["name", "sam"]  1:["age", 18]
```

<img src="https://i.loli.net/2020/08/31/VAgqEPHwR3W94Lt.png" alt="image-20200831220206721" style="zoom: 67%;" />

#### 查看自身+共有属性

>   dir 指以目录的形式，可以查看到【共有属性 `__proto__`】

```js
console.dir(obj)   // 查看 obj内容 及 共有属性 【推荐】
```

<img src="https://i.loli.net/2020/08/31/MkgY6QsHGdwa95E.png" alt="image-20200831220009771" style="zoom: 80%;" />

```js
obj.__proto__      
// 也可以直接打印共有属性（但不推荐此法，因为隐藏属性的命名是不固定的，不同浏览器可能规定不同）
```

或者自己依次用 Object.keys 打印出 `obj.__proto__`



#### 判断一个属性是自身的还是共有的

>   判断一个属性是否是某个对象的属性，可以用 `in`  ，但是 in 无法区分是自身的还是共有的
>
>   ```js
>   "name" in obj      // true
>   "toString" in obj  // true
>   ```

```js
obj.hasOwnProperty('toString')   // false
obj.hasOwnProperty('name')       // true
obj.hasOwnProperty('age')        // true
```

​	

​	

### 原型

>   原型，就是隐藏属性 所指向的对象

#### 每个对象都有原型

-   原型里存着对象的共有属性
-   比如 obj 的原型就是一个对象
    -   `obj.__proto__ `存着这个原型对象的地址
    -   这个原型对象里有 toString / constructor / valueOf 等属性



#### 对象的原型也是对象

>   既然每个对象都有原型，且原型也是对象，那么可以推出：原型上也有原型

-   所以对象的原型上也有原型
-   obj = { } 空对象的原型即为所有对象的原型
-   这个原型包含所有对象的共有属性，是**对象的根**
-   这个原型也有原型，**是 null**     【/nʌl/】
    -   原型为 null 的对象，就是对象的根

```js
console.log(obj.__proto__)  // 原型对象（根对象）
console.log(obj.__proto__.__proto__)   // null  原型上的原型
```

​	



​	

### 查看属性

#### 两种方法查看属性

-   **中括号语法：obj['key'] **

-   点语法：obj.key

-   坑新人语法：obj[key]      // 中括号里是变量，【变量 key】 值一般不等于【字符串 'key'】

    ```js
    var obj = {name: 'sam', age: 18}
    obj['name']   // 'sam'
    obj.name      // 'sam'
    obj[name]     // undefined
    
    console.log(name)   // ""
    window.name = 'age'
    obj[name]    // 18   // 等同于 obj['age']
    ```

    变态

    ```js
    obj['na'+'me']   // 'sam'
    
    obj[console.log('name')]   
    // name   // 先执行log命令，打印内容
    // undefined   // log 函数的返回值为 undefined，相当于执行 obj[undefined] => undefined
    ```




​	

#### 请优先使用中括号语法

-   【点语法】会误导你，让你以为 key 不是字符串
-   等你确定不会弄混两种语法，再改用点语法

>   obj.name 等价于 obj['name']
>   obj.name 不等价于 obj[name]
>
>   简单来说，obj.name 这里的 **name 是字符串，而不是变量**

>   let name = 'sam'
>   此时 obj[name] 等价于 obj['sam'] ，而不是 obj['name'] 和 obj.name

​	

#### 考题

>   区分变量` name` 和 常量字符串 `'name'`

代码

```js
let list = ['name', 'age', 'gender']
let person = {name:'sam', age:18, gender:'man'}

for(let i = 0; i < list.length; i++){
  let name = list[i]
  console.log(person???)
}
// 使得 person 的所有属性被打印出来
```

选项

1.  console.log(person.name)      ✘          // sam sam sam
2.  **console.log(person[name])**    ✔     // sam 18 man



>   区分 name 和 'name' 为什么这么重要
>
>   +   因为如果你现在不搞清楚，那么你在学 Vue 的时候，会更加迷惑

​	

​	

### 修改或增加属性（写属性）

#### 直接赋值

>   直接赋值，name 属性已存在，就相当于修改属性值；name 属性不存在，就会新增这个属性，值为 sam

```js
let obj = {name: 'sam'} // name 是字符串
obj.name = 'sam' // name 是字符串 ✔
obj['name'] = 'sam'  // ✔

obj[name] = 'sam' // 错，因name为变量，值不一定等于'name'

obj['na'+'me'] = 'sam'  // ✔

let key = 'name'; obj[key] = 'sam'
let key = 'name'; obj.key = 'sam' // 错，因为obj.key等价于obj['key']，相当于给obj增加了key属性 值为sam
```

​	

#### 批量赋值

```js
let obj = {name: 'sam'}
Object.assign(obj, {name:'123', age: 18, gender: 'man'}) 
// name 属性已存在，就相当于修改属性值；name 属性不存在，就会新增这个属性，值为'123'

console.log(obj)  // {name:'123', age: 18, gender: 'man'}
```

assign ：赋值的意思

Object.assign() ：是 ES6 新出的 API

​	

​	

### 修改或增加共有属性

>   JS 特性：
>
>   -   读取时，可以读取到（原型上的）共有属性。
>   -   写入时，只写在自己身上，不会影响（原型）共有属性

####  无法通过自身修改或增加共有属性

>   原型上的属性，无法通过自身直接修改

```js
let obj = {}, obj2 = {} // 共有 toString 方法
obj.toString = 'xxx'    // 只会在改 obj 自身属性，不会覆盖共用的 toString 方法

obj.toString     // 'xxx'
obj.toString()   // 报错 obj.toString is not a function

obj2.toString    // ƒ toString() { [native code] }   还是在原型上的方法
obj2.toString()  // "[object Object]"
```



#### 偏要修改或增加原型上的属性

```js
obj.__proto__.toString = 'xxx' // 不推荐用 __proto__
Object.prototype.toString = 'xxx' 
console.dir(obj.toString)

// obj.__proto__ 存的地址，等价于 window.Object.prototype 存的地址
obj.__proto__ === window.Object.prototype  // true
```

-   这是 JS 非常危险的特型，一旦修改，会使得原型上的属性非常不可信 —— JS 的脆弱性
-   **一般来说，不要修改原型**，会引起很多问题：代码崩溃/异常…



​	

​	

### 修改隐藏属性

#### 不推荐使用 ` __proto__` 修改原型

例1

```js
let obj = {name:'sam'}
console.log(obj)   // {name:"sam", __proto__: Object}
obj.__proto__ = null
console.log(obj)   // {name:"sam"}    没有proto原型了，变成非常纯净的对象，不能调用任何功能
```

例2

```js
let person = {name:'sam'}
let person2 = {name: 'jack'}
let common = {kind: 'human', '国籍': '中国'， hairColor: 'black'}
person.__proto__ = common
person2.__proto__ = common
```

![image-20200902210827510](https://i.loli.net/2020/09/02/lMKTZxJaV1Ifcto.png)

>   上述，使用 ` __proto__` 直接修改原型，不推荐，性能非常低

​	

#### 推荐使用 Object.create 修改对象的原型

>   规范的修改对象的原型，使用 Object.create  【功能：用于指定原型】
>
>   ```js
>   var obj = Object.create({name:'sam'})
>   console.log(obj)  // { __proto__:{name:'sam'} } 
>   ```

用法 ↓

```js
let common = {kind: 'human', '国籍': '中国'， hairColor: 'black'}
let obj = Object.create(common)  // 以common为原型对象，创建obj
obj.name = 'sam' // 点方法，挨个添加属性，或 批量修改/添加属性 Object.assign(obj,{ ... })
... 
```

Object.create()  第二个参数，写法比较麻烦

```js
let person = Object.create(common, {
  name: { value: 'sam' }
})
console.log(person)  // {name:'sam', __proto__: Object}  => 
```

>   规范的写法：大概是，要改就一开始就改；别后来再改，如`person.__proto__ = common`  影响性能

​	

​	

​	

## 总结

### 删

```js
delete obj['name']
'name' in obj // false  // in 用于判断某个对象中是否含这个属性，缺点：无法区分是自身的，还是原型上共有的
obj.hasOwnProperty('name')  // false  // 只有对象自身含有这个属性，才会返回 true
```

### 查

```js
Object.keys(obj)
console.dir(obj)  // 目录形式，详细
obj['name']
obj.name // 记住这里的 name 是字符串
obj[name]  // 记住这里的 name 是变量
```

### 改

```js
改自身 obj['name'] = 'jack'
批量改自身 Object.assign(obj, {age:18, ...})
                          
改某个共有属性 obj.__proto__['toString'] = 'xxx'  // 强烈不推荐
改某个共有属性 Object.prototype['toString'] = 'xxx'

换原型 obj.__proto__ = common  // 强烈不推荐
换原型 let obj = Object.create(common)

// 注：所有 proto 代码都是强烈不推荐写的。学习时可以用用，但是工作中不要用
```

### 增

基本同上，已有属性则改；没有属性则增。



>   +   查：属于读，可以读到原型链
>   +   改 和 增：属于写，只能改自身，不能改到原型














