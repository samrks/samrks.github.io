# JS 阶段总结


<!--more-->

​	

## 知识点

### 基本概念

-   **内存**：内存图
-   **变量**：如何声明、哪些类型
-   **数据类型**：哪7种
-   **对象**：是什么、有什么属性、如何构造的

### 控制语句

-   if...else...
-   for...

### 对象

-   原型、原型链
-   对象分类：数 组是什么对象、函数是什么对象
-   如何 new 一个新对象：new 做了哪 4 件事情
-   构造函数：是什么
-   this 的隐式传递和显式传递：普通调用就是隐式传递、推荐用 call 显式传递

​	

​	

## 难点

### JS 三座大山

1.  原型
2.  this
3.  AJAX

### 已经遇到了前两座

-   前端门槛
-   反复学反复理解

​	

​	

## 最重要的知识 ⭐️

### 第一个重要知识：JS 公式 

```js
对象.__proto__ === 其构造函数.prototype
```

>   JS 唯一公式

​	

### 第二个重要知识：根公理

```js
Object.prototype 是根对象
Object.prototype 是所有对象的原型 
Object.prototype 是所有对象的直接或间接的原型
```

>   加了一个直接或间接，所谓公理就是 JS 规定好的

例

```js
{name:'frank'} 的原型: Object.prototype（直接原型）
[1,2,3] 的原型是: Array.prototype（直接原型） 的原型是  =>  Object.prototype（间接原型）
Object 的原型是: Function.prototype（直接原型） 的原型是 => Object.prototype（间接原型）
```

​	

### 第三个重要知识：函数公理

```js
所有函数都是由 Function 构造的
```

推出 👇

```js
任何函数.__proto__ === Function.prototype
任意函数有 Object / Array / Function
```

例

```js
let f1 = () => console.log('hi')
f1.__proto__ === Function.prototype  // true
```

​	

​	

## 总结

-   JS 公式、根公理、函数公理
-   基于这三个知识和基础知识，可以推出 JS 世界

（细品）

​	

​		

## 拨乱反正（误区）

### 乱一：「的原型」

#### XXX 的原型

>   提到 「xxx 的原型」，到底指的是 `__proto__` 还是 `prototype`

```js
- {name:'frank'} 的原型
- [1,2,3] 的原型
- Object 的原型
```

#### 解读

```js
- Object 的原型是 Object.__proto__：对
- Object 的原型是 Object.prototype ：错
```

#### 分析

```js
- 约定只要提到「xxx的原型」就是指「xxx.__proto__」
- 中文的「原型」无法区分 __proto__ 和 prototype，可以同时代指这俩
- 所以我们只能约定，「原型」默认表示 __proto__ 
- 只不过 __proto__ 正好等于某个函数的 prototype
```

​	

### 乱二：直接/间接原型

#### 矛盾了吗？

-   说 [1,2,3] 的原型是 Array.prototype
-   又说 Object.prototype 是所有对象的原型
-   那为什么 Object.prototype 不是 [1,2,3] 的原型

#### 怎么理解

-   原型分两种：直接原型 和 间接原型  
-   对于普通对象来说，Object.prototype 是直接原型
-   对于数组、函数来说，Object.prototype 是间接原型

```js
- {name:'frank'} 的原型: Object.prototype（直接原型）
- [1,2,3] 的原型是: Array.prototype（直接原型） 的原型是  =>  Object.prototype（间接原型）
- Object 的原型是: Function.prototype（直接原型） 的原型是 => Object.prototype（间接原型）
```

>   其实，只要在浏览器中输出一下 [1,2,3]  看看 proto 就能理解 直接原型 和 间接原型

>   比较好理解

​	

### 乱三：Object.prototype

#### 有人认为 Object.prototype 不是根对象

（质疑公理 😏）

#### 理由如下

-   Object.prototype 是所有对象的原型
-   Object 是 Function 构造出来的
-   所以，Function 构造了 Object.prototype
-   推论，Function 才是万物之源啊！
-   （看似无懈可击的推理，实际上是对更基础的知识没有理解好）

#### 反驳

>   要理解 <font color="red">Object.prototype</font> 和 <font color="red">Object.prototype 对象</font>  的区别

-   实际上，<font color="red">Object.prototype</font> 的值，本身是一个内存地址，指向的内存空间是  <font color="red">Object.prototype 对象</font>

-   只是我们日常习惯口头上称 <font color="red">Object.prototype</font> 为原型对象（实际上只是个地址）

-   “Function 构造了 Object.prototype”  是构造出了 Object 上存根地址的属性 `prototype: #202`，而不是构造出原型对象本身 （结合内存图理解）

-   对象里面从来都不会包含另一个对象，只会包含另一个对象的地址

![image-20200924190141275](https://i.loli.net/2020/09/24/KcdBQY1zrNC7uqR.png)

  

>   接下来我们要把 JS 世界的建造顺序理清楚

​	

## 再画 JS 世界

### JS 世界的构造顺序

1.  创建根对象 #101  (toString)，根对象没有名字

2.  创建函数的原型 #208  (call /apply)，原型 __p 为 #101

3.  创建数组的原型 #404  (push/pop)，原型 __p 为 #101

4.  创建 Function #342，原型 __p 为 #208

5.  用 Function.prototype 存储函数的原型，等于 #208

6.  此时发现 Function 的 _\_proto__ 和 prototype 都是 #208

7.  用 Function 创建 Object #909

8.  用 Object.prototype 存储对象的原型，等于 #101

9.  用 Function 创建 Array 

10.  用 Array.prototype 存储数组的原型，等于 #404

11.  创建 window 对象

12.  用 window 的 'Object' 'Array' 属性将 7 和 9 中的函数命名

>   记住一点，JS 创建一个对象时，不会给这个对象名字的

![JS世界构造顺序（内存图）](https://i.loli.net/2020/09/24/aMDNfGZqpbE8Yj9.jpg)

### 构造一个新对象的过程

1.  用 new Object() 创建 obj1
2.  new 会将 obj1 的原型 __p 设置为 Object.prototype，也就是 #101
    因为创建 JS 世界时，把原型的地址存到了 Object.prototype  上
3.  用 new Array() 创建 arr1
4.  new 会将 arr1 的原型 __p 设置为 Array.prototype，也就是 #404
5.  用 new Function 创建 f1
6.  new 会将 f1 的原型 __p 设置为 Function.prototype，也就是 #208

>   总结
>
>   `new XXX`， 就把 new 出来的实例的原型 __p 设为 `XXX.prototype`

​	

### 自定义构造函数创建过程

1.  自己定义构造函数 Person，函数里给 this 加属性
2.  Person 自动创建 prototype 属性和对应的对象 #502
3.  在 Person.prototype #502 上面加属性
4.  用 new Person() 创建对象 p
5.  new 会将 p 的原型 __p 设为 #502 

​	

​	

### 漂亮的图示 ⭐️

>   需求：清晰的理解下图的每一条线

>   Array、Object、Function 三个都是函数，所以他们本身的原型 __p 都指向「函数的原型」
>
>   而每一个函数都存了其构造出的「(孩子) 新对象」的原型（共有属性） prototype

![图片1](https://i.loli.net/2020/09/25/vZ5REwgmpKTuiSc.png)



​	

### prototype 和 _\_proto__ 区别

>   细品

1.   都存着**原型的地址**

2.   只不过 `prototype` 挂在函数上

     +   Object 是个构造函数，Object.prototype 就存储了 Object构造的新对象 的共有属性
     +   Array 是个构造函数，Array.prototype 就存储了 Array构造的新对象 的共有属性

3.   `__proto__` 挂在每个新生成的对象上

+   Object构造的新对象 obj1 的共有属性在 obj1._\_proto__ 上
    +   Array构造的新对象 arr1 的共有属性在 arr1._\_proto__ 上

4.   函数上的 prototype 指向这个函数构造出来的孩子的共有属性

5.   孩子上的 _\_proto__ 指向其父构造函数赋予这个孩子的共有属性

6.   每个函数都有 prototype、每个对象都有_\_proto__ ，而函数也属于对象，所以函数同时有 prototype 和 _\_proto__

```js
var obj = {}  
等价于
var obj = new Object()

// Object 上有 prototype
// obj 上有 __proto__

obj.__proto__ === Object.prototype   // true
```

​		

​	

## 总结 ⭐️

>   必须非常清晰的理解

>   不管怎么绕，都要清楚的理解
>
>   +   函数的构造函数 Function ，构造出了函数 f1
>   +   对象的构造函数 Object，构造出了对象 obj1
>   +   数组的构造函数 Array ，构造出了数组 array1
>   +   (window.)Function 的构造函数是 Function
>   +   array1 的构造函数是 Array
>   +   obj 的构造函数是 Object
>   +   Object 的构造函数是 Function
>   +   …

### 构造函数

-   是用来构造对象的
-   会预先存好对象的原型，原型的原型是根
-   new 的时候将对象的 __p 指向原型

### 对象

-   所有对象都直接或间接指向根对象
-   如果对象想要分类，就在原型链上加一环
-   用构造函数可以加这一环

### 思考

-   如果加了一环之后，想再加一环怎么办 （加两环，要用继承的方式）
-   以后会在「继承」里讲

​	

​	

## 例 ⚡️

>   把其他知识都忘掉，就记住以下
>
>   1.  **JS 公式**：对象._\_proto__ === 其构造函数.prototype
>   2.  **根公理**：Object.prototype 是所有对象的直接或间接的原型
>   3.  **函数公理**：所有函数都是由 Function 构造的

1.  Object.prototype 的原型是什么

    ```js
    // 1.约定只要提到「xxx的原型」就是指「xxx.__proto__」
    // 2.Object.prototype 是所有对象的原型（是根，根就不再往上有原型了）
    Object.prototype.__proto__ === null
    ```

2.  Function.prototype 的原型是什么

    ```js
    //（JS公式）等于问 Function.prototype 的「构造函数」.prototype
    // 重点在 Function.prototype 的构造函数 是 ？？
    Function.prototype.__proto__ == Object.prototype
    ```

3.  ```js
    var f = () => {}  // 箭头函数的原型是什么  
    // 所有函数都是由 Function 构造的
    f.__proto__ === Function.prototype
    ```

4.  Function 的原型是什么

    ```js
    // 所有函数都是由 Function 构造的
    Function.__proto__ === Function.prototype
    ```

5.  Array.prototype.toString 的原型是什么

    ```js
    // toString 是个函数
    // 所有函数都是由 Function 构造的
    Array.prototype.toString.__proto__ === Function.prototype
    ```

6.  Object 的原型是什么

    ```js
    Object.__proto__ === Function.prototype
    ```

7.  Array.prototype 是谁的原型

    ```js
    // 把公式一反过来用
    // 是用 Array 构造的实例的原型
    Array.prototype === [].__proto__
    ```



