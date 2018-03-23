# 深入理解 Vue 数据响应式


<!--more-->



## 相关文章（参考）

[Vue响应式原理-理解Observer、Dep、Watcher](https://g.yuque.com/weixiaozhudelaocaipi/yy8wge/vn5cw7)

[深入响应式原理—— Vue 官方文档](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%A6%82%E4%BD%95%E8%BF%BD%E8%B8%AA%E5%8F%98%E5%8C%96)

[Vue3将要使用Proxy作为数据驱动，不想进来看看吗？](https://www.codenong.com/j5e5e70f751882548ff3/)

[vue3.0尝鲜 -- 摒弃Object.defineProperty，基于 Proxy 的观察者机制探索](https://g.yuque.com/weixiaozhudelaocaipi/yy8wge/pi0dhp?language=en-us)

[利用Proxy自动添加响应式属性](https://segmentfault.com/a/1190000016028331)

[详解Vue响应式原理](https://blog.fundebug.com/2019/07/10/responsive-vue/)

[你真的理解Vue的数据响应式吗](https://zhuanlan.zhihu.com/p/143933911)

​	

## 什么是数据响应式

### 响应

“响应”，中文的意思也就是“回应”。比如，别人叫你一声或者给你发消息，你回复了他，这个过程就叫响应。

### 数据响应式

[Vue 的官方文档](https://link.zhihu.com/?target=https%3A//cn.vuejs.org/v2/guide/reactivity.html)已经很明确的告诉我们： 

+   只要修改数据 data（`render(data)` 做出响应）就会重新渲染视图（不需要开发者再去操作 DOM）
+   这个联动的过程，就是 vue 的数据响应式
+   这也是 Vue 最独特的特性之一 —— **非侵入性的响应式系统**。

#### 怎么理解“非侵入性”

我觉得，可以理解为“不可篡改的”：

+   即用户无法绕过 Vue 的监听，去篡改内部数据
+   Vue 做到了「无论用户以任何方式修改 data 中的内部数据，都会被当前 Vue 组件监听到，并通知对应的 watcher，从而重新渲染与数据关联的组件」
+   拓展：该特性依靠「代理 proxy」实现

​	

## 浅析「Vue 响应式原理」

>   Vue 是通过 Object.defineProperty 来实现数据响应式

当我们把一个对象作为 data 选项传入 Vue 实例，Vue 会遍历 data 中的所有属性，并用 Object.defineProperty 对这些属性进行改造，**用 getter / setter 监控每个属性的读写**。

每个组件实例都对应一个 watcher 实例，它会根据 getter 的触发来记录渲染视图用到的数据依赖项。**当这些数据的 setter 触发（数据被修改）时**，会通知 watcher，从而**使数据关联的组件重新渲染**。

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/data.png" alt="data" style="zoom:50%;" />

>   [如何追踪变化](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%A6%82%E4%BD%95%E8%BF%BD%E8%B8%AA%E5%8F%98%E5%8C%96)
>
>   -   当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，并使用 Object.defineProperty 把这些 property 全部转为 getter/setter。
>   -   这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 能够追踪依赖，在 property 被访问和修改时通知变更。
>   -   每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。



## 深入响应式原理

### 普通对象 🆚 传入 Vue 的普通对象

```js
const myData = { // myData 是一个普通对象
  n: 0
}
console.log(myData)  // （输出如下图）
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201229170817094.png" alt="image-20201229170817094" style="zoom:80%;" />

```js
const myData = { // myData 是一个普通对象
  n: 0
}
new Vue({
  data: myData  // 把 myData 作为 data 选项，传入 Vue 实例
})
console.log(myData) // 看看现在的 myData 是什么样子（输出如下图）
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201229171546578.png" alt="image-20201229171546578" style="zoom:80%;" />

### Vue 对 data 做了什么

>   Vue 最独特的特性之一，是其非侵入性的响应式系统。数据模型仅仅是普通的 JavaScript 对象。而当你修改它们时，视图会进行更新。

当我们把 data 选项传入 Vue 实例，Vue 会遍历 data 中的所有属性（property），进行「监听和代理」

+   **监听：数据响应式的底层细节**
+   代理：不暴露任何接口，防止用户篡改数据

从而将这些数据改造成非侵入性的响应式数据

​	

### 前置知识

#### Object.defineProperty

>   [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)：`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

**使用方式**

```js
Object.defineProperty(obj, prop, descriptor)
// obj 代表定义属性的对象；prop 代表定义的属性名称；descriptor 代表定义的内容。
```

>   需求 ⭕️ 用 Object.defineProperty 添加属性 n，值为 0

```js
let data1 = {}
Object.defineProperty(data1, "n", {  // 表示向 对象data1 中添加属性 "n"，值为 0
  value: 0
}) 
console.log(data1)    // { n: 0 }
console.log(data1.n)  // 0
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201229231800071.png" alt="image-20201229231800071" style="zoom:67%;" />

#### getter / setter

>   [MDN：getter/setter 描述 ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#%E6%8F%8F%E8%BF%B0)、[示例](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#%E8%87%AA%E5%AE%9A%E4%B9%89_Setters_%E5%92%8C_Getters)
>
>   -   get：属性的 getter 函数。当访问该属性时，会调用此函数。不传入任何参数
>   -   set：属性的 setter 函数。当属性值被修改时，会调用此函数。接受一个参数（也就是被赋予的新值）

>   需求 ⭕️ 用 Object.defineProperty 添加属性 n，并监听 n 的读取。
>
>   +   使用 Object.defineProperty 把这些 property 全部转为 getter/setter。
>   +   设置 getter / setter，当我们读取 n 时会自动执行 getter，修改 n 时会自动执行 setter

```js
let data2 = {}
data2._n = 0  // 添加一个媒介属性 _n 用于存储 n 的原值 0
Object.defineProperty(data2, "n", {
  get() {
    // return this.n 为什么 getter 不直接返回 this.n，而需要额外声明一个属性 _n，这不多此一举吗 ？
    return this._n
  },
  set(value) {
    this._n = value
  }
})
console.log(data2.n) // 0
```

>   疑问：为什么 getter 不直接返回 this.n，而需要额外声明  _n 来存储下 n 值，并用 _n 作为中转呢 ？

原因：

-   所有读取 n 的操作（data2.n、this.n），都会自动调取 getter 方法的返回值，作为 n 值
-   如果读取 n 时，getter 返回 this.n，this.n 仍是读取 n 值的操作，会接着自动获取 getter 返回值，于是这就成了一个「死循环」，不停的读取 n，始终拿不到终值
-   所以声明一个媒介属性 _n 来存储 n 值。当读取 n 时，getter 返回 _n 值，写入 n 时 setter 修改 _n 值

​	

>   疑惑：为什么不直接在声明对象时就定义属性 `n : 0` ，用 Object.defineProperty 不是把过程复杂化了吗 ？
>
>   ```js
>   let data1 = { n: 0 } 
>   console.log(data1.n)  // 0
>   data1.n = 100
>   console.log(data1.n)  // 100
>   ```
>
>   字面量形式创建对象，易读易写，难道不香 ？ 答：不香（往下看原因）

​	

####  为什么使用这个 API （作用）

>   当需求变复杂 ↓
>
>   -   需求 ⭕️ 添加属性 n，默认值 0 。同时，限制 n 的赋值永远不能小于 0
>   -   比如：赋值 `data2.n = -1` 是无效的，赋值 `data2.n = 1` 有效

```js
let data2 = {}
data2._n = 0
Object.defineProperty(data2, "n", {
  get() {
    // others ...
    return this._n 
  },
  set(value) {
    if (value < 0) return  // 👈 如果 value 小于 0，就直接返回，不执行赋值操作
    this._n = value
    // others ...
  }
})
```

>   所以，使用 Object.defineProperty 把对象的属性全部转为 [getter/setter](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#%E8%87%AA%E5%AE%9A%E4%B9%89_Setters_%E5%92%8C_Getters)
>
>   +   **这样可以实现在读写触发的同时，添加其他操作。**
>   +   Vue 就是利用 Object.defineProperty 的这一特性，实现数据监听的效果

​	

### 监听的逻辑 📌

>   Vue 的响应式原理，通过 Object.defineProperty 实现

#### 示例

```js
// demo.vue 组件
const myData = {
  name: 'jack',
  sex: 'male',
  age: 20
}
new Vue({
  data: myData,  // 当我们把 myData 作为 data 选项传入 Vue 实例，Vue 会 ... 🔗
  template: `
		<div>
			姓名：{{ name }}
			年龄：{{ age }}
		</div>
	`
}).$mount("#app")
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201230185537838.png" alt="image-20201230185537838"  />

#### 模拟 Vue 内部操作

>   🔗 Vue 会遍历 data，并通过 Object.defineproperty 将 data 中的每一项数据改造成 getter/setter

```js
// 当 Vue 接收到 myData，会遍历对象、使用 Object.defineProperty 进行改造
for(let key in myData) {
  let val = data[key] // val 存储当前遍历属性的原值
  Object.defineProperty(data, key, {
    get() {
      // others... 通知 watcher 添加依赖项 📌
      return val // 触发 get 函数，返回属性的值
    },
    set(newVal) {
      val = newVal // 触发 set 函数，修改属性的值
      // others... 通知 watcher 渲染关联组件 📌
    }
  })
}
```

> 每个组件实例都对应一个 watcher 实例
>
> +   在组件**渲染**的过程中，getter 被触发的数据，会被 watcher 记录为「渲染视图的**依赖项**」
>
> -   当**依赖项**的 setter 触发时（说明数据被修改），会通知 watcher，从而使它**关联的组件重新渲染**

#### 示例分析

```js
// demo.vue 组件
const myData = {
  name: 'jack',
  sex: 'male',
  age: 20
}
const vm = new Vue({
  data: myData,
  template: `
		<div>
			姓名：{{ name }}  
			年龄：{{ age }}
		</div>
	` // 👆 视图渲染，触发了 name 和 age 的 getter，通知 watcher 记录属性为依赖项
}).$mount("#app")

myData.name = 'rose' 
// 👆 触发 name 的 setter，依赖项的 setter 被触发，通知 watcher 重新渲染相关组件
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201230185813820.png" alt="image-20201230185813820"  />

>   以上
>
>   一个简单的数据响应式，就是这样实现的



### 拓展：代理的逻辑

>   需求 ⭕️ 向对象中存储一个属性 n，条件是 n 的赋值永远不能小于0
>
>   +   请问，我们怎样保证「不管用户怎么修改 n 的值，都可以满足上述要求」

#### 实验一

（[在线示例](https://jsbin.com/voxomus/5/edit?html,js,console)）

```js
let myData = {}
myData._n = 0 // 始终用 _n 存储 n 值（ _n 相当于一个中转属性）
Object.defineProperty(myData, "n", {
  get() {
    return this._n // 必须中转，如果直接返回 this.n 会陷入死循环
  },
  set(value) {
    if (value < 0) return
    this._n = value
  }
})
myData.n = -1
console.log(myData.n) // 0 修改无效
myData.n = 1
console.log(myData.n) // 1 修改成功
```

##### 钻空子

[在线示例](https://jsbin.com/voxomus/edit?html,js,console)

```js
// 看似需求已经实现，但如果对方直接修改 myData._n，那么上述 set 就无法限制了 ！
// 虽然 n 不能设置为小于0的值，但 _n 可以，它只是myData上的一个普通的属性，没有set监听，可以任意赋值

myData._n = -1
console.log(myData.n) // -1  篡改成功
```

>   结论：实验一可以通过修改中转属性，绕过监听，轻易篡改数据 n



#### 实验二

>   根据实验一的“钻空子”，提出解决思路：
>
>   +   不在 myData 对象上暴露出任何不被监听的属性接口（最好就不要起对象名了）
>   +   换句话说，需求 ⭕️ 要让任何访问、任何接口，都必须能被监听到
>   +   如果数据本身，就是一个函数的返回值，那么每次调用数据都会执行这个函数，我们只需在函数内部在实现对数据的监听，这样就可以防止用户偷跑、篡改数据
>   +   具体实现参照「代理」的思想

写一个 proxy 函数，参数接收一个匿名对象，**在匿名对象中存放真实数据**，在 proxy 函数中实现对数据的代理监听

+   使用匿名对象，因为没有提前声明对象名的对象，就自然无法被外界访问
+   每次调用 myData 上的数据，都会执行 proxy 方法，把真实数据转移到 obj 进行监听，再返回 obj，保证调用myData 的属性时，一定调用的是 obj 上被监听了的属性
+   可保证 myData 上一定没有暴露任何未被监听的属性（_n），所有属性都经过 proxy 被监听处理了

[在线示例](https://jsbin.com/voxomus/15/edit?html,js,console)

```js
let myData = proxy({ data: { n: 0 } })

function proxy({ data }) {  // 解构赋值，参数 data 就是 { n: 0 }，存储了真实数据
  const obj = {} 
  // 把 data 上的所有数据转移到 obj 上进行监听，然后返回 obj，这样 保证 obj 上的所有数据都是被监听到的 
  Object.defineProperty(obj, "n", { 
    get() {
      return data.n
    },
    set(value) {
      if (value < 0) return;
      data.n = value
    }
  })
  return obj // obj 就是 data 的代理对象
}

myData.n = -1
console.log(myData.n) // 0  修改无效
myData.n = 1
console.log(myData.n) // 1  修改成功
```

##### 钻空子

>   如果用户自己写一个数据对象 hackData
>
>   再把 hackData 放到代理中，然后修改这个 hackData，就可以实现篡改数据的目的

[在线示例](https://jsbin.com/voxomus/21/edit?html,js,console)

```js
let myData = proxy({ data: { n: 0 } }) // 原始数据
let hackData = { n: 0 } // 声明新的数据，用于篡改原始数据
myData = proxy({ data: hackData })

function proxy({ data }) {
  const obj = {} 
  Object.defineProperty(obj, "n", { 
    get() {
      return data.n
    },
    set(value) {
      if (value < 0) return;
      data.n = value
    }
  })
  return obj
}

console.log(myData.n) // 0
hackData.n = -1
console.log(myData.n) // -1 篡改成功
```

>   结论：用户可以强行往 proxy 中塞一个 hackData，这样只要 hackData 就可以篡改原始数据 n

#### 实验三 

>   根据实验二的 “钻空子”
>
>   +   用户强行往 proxy 中塞了一个 hackData，想通过 hackData 篡改我方原始数据 n
>   +   需求 ⭕️ 如果用户擅自篡改数据（传入自己写的数据），就要拦截这种行为（升级 proxy 2.0 ）

[在线示例](https://jsbin.com/soranoq/5/edit?html,js,console)

```js
let myData = proxy2({ data: { n: 0 } }) // 原始数据
let hackData = { n: -1 }  // 声明新的数据，用于篡改原始数据
myData = proxy2({ data: hackData })

function proxy2({ data }) {  // data 接收 hackData：{ n: 0 }
  // 原本应该遍历 data 上的所有 key，这里做了简化，假设 data 上只有一个数据 n
  let value = data.n  // 声明变量来存储原始的n值（这里存储n，目的是后面把n删掉）
  
  // delete data.n  
  // 可以省略删除操作，因为下面添加虚拟属性n时与data中原本的属性重名，会自动覆盖（删除）原本的属性
  
  Object.defineProperty(data, "n", { // 这样就完全监控了 n
    get() {
      return value
    },
    set(newValue) {
      if (newValue < 0) return
      value = newValue
    }
  })
  // 上面就是新添加的代码，用于完全监听 n【监听的逻辑】
  // 相当于安装监听器，如果用户想绕过监听，就立马知道并拦截你

  // 下面仍是【实验二中的代理逻辑】
  const obj = {}
  Object.defineProperty(obj, "n", {
    get() {
      return data.n
    },
    set(value) {
      if (value < 0) return 
      data.n = value
    }
  })
  return obj // obj 就是代理
}

// 用户如果直接修改 myData 的数据，走正常的代理逻辑。
// 用户如果想绕过代理，篡改原始数据，会走监听逻辑
// 只要经过 proxy2 就一定会处于监听之下，因为会删掉原本的数据 n

// 篡改原始数据（会创建虚拟属性来替代原本属性，通过对虚拟属性的监听实现数据监听）
hackData.n = -1  
console.log(myData.n) // 0 修改无效
hackData.n = 1
console.log(myData.n) // 1 修改成功

// 直接修改（通过代理实现数据监听）
myData.n = -1
console.log(myData.n) // 1 修改无效
myData.n = 0
console.log(myData.n) // 0 修改成功
```

#### 总结

```js
// 下面这句代码看着眼熟吗 ?
let myData = proxy2({ data: hackData })
let vm = new Vue({ data: hackData })

// myData 就相当于 vm
// proxy2 就相当于 new Vue，只不过没加 new 而已
```

>   前面实验推出的 proxy 代码，**就是 Vue 内部的源代码**

##### 一定要注意 💡

-   前面实验的**研究方法**比知识本身更重要
-   这些方法能让你**不读源码**，也能了解真相（底层原理）



##### new Vue 对 data 做了什么

>   理清了代理的逻辑，现在我们可以说说 new Vue 到底对 data 做了什么

```js
import Vue from "vue/dist/vue.js"

const hackData = {
  n: 0
}
console.log(hackData) // 精髓 👈👈👈 通过log发现不同之处，从而进行推理验证

const vm = new Vue({
  data: hackData,
  template: `<div>{{n}}</div>`,
}).$mount("#app")

setTimeout(() => {
  hackData.n += 10;
  console.log(hackData) // 精髓 👈👈👈
}, 0)
```

>   只要把 hackData 传给 new Vue 就会马上对 hackData 进行篡改
>
>   +   hackData上原本的数据 n 会消失，取而代之的是 get n、set n（虚拟数据）
>   +   关键点就在两个 log 上发现蛛丝马迹（这种研究方法很重要）









## 监听存在的问题（Vue 的 bug）📌

>   （面试可能会问哦）
>
>   我们已知：数据应提前在 data 中定义好才能使用。如果在 data 外动态的添加新的数据，或引用没有定义在 data 的数据，会怎样呢 ？

### 情景一：引用未定义的数据会怎样

>   Vue 实例的 data 中没有定义 n ，但又在视图中使用 n，会怎么样

[代码示例](https://codesandbox.io/s/empty-field-ofmwn)

```js
new Vue({
  data: {},
  template: `
    <div>{{n}}</div>
  `
}).$mount("#app")
```

>   在视图中使用未在 data 中定义的数据，Vue 会给出一个警告报错 `[Vue warn]` 

-   数据没有传入实例的 data 中，就无法实现监听数据变化，不监听就不能实现刷新视图，违背数据响应式原则

    ![image-20201231110534602](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201231110534602.png)

#### 解决办法

>   必须在 data 中把 n 提前声明好

>   由于 Vue **不允许动态添加根级响应式 property**，所以你必须在初始化实例前声明所有根级响应式 property，哪怕只是一个空值（[文档](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%A3%B0%E6%98%8E%E5%93%8D%E5%BA%94%E5%BC%8F-property)）

[在线代码](https://codesandbox.io/s/serverless-fast-hl420?file=/src/main.js)

```js
new Vue({
  data: {
    n: 1 // 👈👈👈 null、undefined..都不会报错
  },
  template: `
		<div>{{n}}</div>
	`,
}).$mount("#app")
```

​	

### 情景二 ：动态添加对象属性（在 data 外新增 key）

>   需求：点击按钮，视图中显示 1

[代码示例](https://codesandbox.io/s/tender-leaf-8ogdx?file=/src/main.js)

```js
new Vue({
  data: {
    obj: {
      a: 0 // obj.a 会被 Vue 监听、代理
    }
  },
  template: `
    <div>
      obj.b：{{obj.b}}             
      <button @click="set">赋值</button>
    </div>
  `,
  methods: {
    set() {             
      this.obj.b = 1  // 请问，页面中会显示 1 吗？
    }
  }
}).$mount("#app") 
```

![image-20201231181614259](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201231181614259.png)

>   请问，点击“赋值”按钮，视图中会显示 1 吗 ？

-   答案：不会显示 1。
-   因为 Vue 不能监听到未在 data 中定义的数据 `obj.b`
-   但为什么引用未定义的 obj.b 没有警告报错呢 ？

>   情景一视图引用 data 中未定义的 n 会报错，那为什么情景二中引用未提前定义的 obj.b 却不会报错 ？

-   因为 Vue 的数据响应式是数据的**根级响应**。Vue 只会检查第一层属性，检查不到第一层属性内部定义的属性
-   也就是说，对于 `this.obj.b`，Vue 只会检查 data 中是否有定义 `this.obj`，如果没有定义 obj 就会警告报错，但 Vue 不会再深层检验 obj 的内部属性
-   **情景二视图中虽然引用了未提前声明的 `obj.b`，但 Vue 会检查到 `obj `已经在 data 中提前声明了，所以此处的引用不会报错，但也不会生效，因为 `b` 确实没有在 data 中声明。**
-   综上，情景二的这种用法，仅是不报错而已，但也并不会渲染出视图

#### 解决办法

>   使用 Vue.set 或者 this.$set，为已有对象添加新的属性（[文档](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%AF%B9%E4%BA%8E%E5%AF%B9%E8%B1%A1)）

[在线代码](https://codesandbox.io/s/rough-darkness-wh3mw?file=/src/main.js)

```js
new Vue({
  data: {
    obj: {
      a: 0
    }
  },
  template: `
    <div>
      obj.b：{{obj.b}}
      <button @click="set">赋值</button>
    </div>
  `,
  methods: {
    set() {
      Vue.set(this.obj, 'b', 1)       // 👈👈👈 
      // this.$set(this.obj, 'b', 1)  // 👈👈👈
      // 两种写法没有区别（是同一个函数）
      console.log(Vue.set === this.$set) // true (加$是为防止data中有同名属性set)
    }
  }
}).$mount("#app")
```

![image-20201231182143985](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201231182143985.png)

#### Vue.set 和 this.$set 执行了哪些操作

```js
Vue.set(obj, key, value)  // 往obj中新增一个key属性
```

-   新增 key 
-   对 key 自动创建代理和监听（如果没有创建过）
-   触发 UI 更新（但并不会立刻更新 —— 异步更新，这块内容值得单独写一篇博客了）
    -   执行 UI 更新是 Vue 做的，set 只会触发 Vue 来执行更新

[示例演示](https://codesandbox.io/s/youthful-swanson-yz7pm?file=/src/main.js)

```js
new Vue({
  data: {
    obj: {
      a: 0  // 👈
    }
  },
  template: ` 
    <div>
      {{obj.b}} 👈
      <button @click="setB">set b</button> 👈
      <button @click="addB">add b</button> 👈
    </div>
  `,
  methods: {
    setB() {
       Vue.set(this.obj, 'b', 1) // 👈👈   // 或 this.$set(this.obj, 'b', 1)
    },
    addB(){
      this.obj.b += 1  // 👈👈
    }
  }
}).$mount("#app");
```

​	

### 情景三：data 中的数组如何处理？

>   有没有某种情况是「没办法把 key 提前在 data 中声明好」的
>
>   +   有，如果数据是一个数组类型，就无法提前把所有元素声明好
>   +   Vue 中的数组，可以理解成对象的形式（具体见下）

#### 尝试：动态添加数组元素

>   [示例](https://codesandbox.io/s/proud-worker-xwyoo) 

```js
new Vue({
  data: {
    array: ["a", "b", "c"]
  },
  template: `
    <div>
      {{array}}
      <button @click="setD">set d</button>
    </div>
  `,
  methods: {
    setD() {
      this.array[3] = "d"  // 请问，页面中会显示 'd' 吗 ？ 答：不会
    }
  }
}).$mount("#app");
```

>   如上代码，点击 button 按钮，往数组中添加一个元素，毫无反应，为什么会这样呢 ？
>
>   +   分析：数组 `array: ["a", "b", "c"]` 理解成对象的形式 👇

```js
{
  0: "a",
  1: "b",
  2: "c",
  length: 3
}
```

>   data 中的数据 array，相当于在 data 中声明了 0、1、2 三个属性
>
>   +   往数组（对象）中新增元素，相当于[情景二](# 情景二 ：动态添加对象属性（在 data 外新增 key）)「在 data 外新增 key，动态添加对象属性，Vue 无法监听到该属性」
>   +   所以 `this.array[3] = "d"` 是无效的

综合「情景一、情景二」，对于「引用未声明的数据」或「动态添加对象属性」给出解决办法：

-   方案一：视图中直接引用的根级属性，必须在 data 中提前声明
-   方案二：动态添加对象属性，需用 set 方法

>   如果采用方案一：提前在数组中定义数据

```js
data:{
  array: ['a', 'b', 'c', undefined, undefined, ...] 
},
methods: {
  setD() {
    this.array[3] = "d"  
    // 虽然可行，但无法预知有多少个 key、需要对应多少个 undefined
    // 所以这种「提前把 key 都写到 array 」的方案不可取
  }
}
```

-   数组的长度可以一直增加（下标就是 key），不可能提前预知有多少个 key（如果这个数组是所有用户的列表），我们不可能提前把数组的 key 都声明出来

>   如果采用方案二：不能提前声明，那就使用 Vue.set 或者 this.$set

```js
this.$set(this.array, 3, 'd')
// set(Array, Index, value)
```

-   通过 set 新增数组 key，这是可行的方案。但每次添加新元素都要执行 set，比较繁琐

+   有更简单的办法吗 ？

#### 尤雨溪的做法

>   测试：既然是数组，可以用数组方法 push 添加元素吗？
>
>   结果：可以，push 生效

```js
this.array.push('d')
console.log(this.array) // 看看 vue 里的 push 
```

>   疑惑：为什么直接添加元素会失败，但 push 添加元素却成功呢 ？
>
>   原因：Vue 里的 push 方法，已经不是数组原型上的 push 方法了（如下图 ）

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/122334问题.png" alt="122334问题" style="zoom:67%;" />

>   如图，array 看起来是普通数组，但当我们把 array 传给 Vue 之后，Vue 会篡改这个数组，往中间插入一层原型，原型上有 7 个方法。此 7 法与数组原型上的方法**同名**（代码被修改），调用时就会覆盖原型方法

7 个新的 api 会执行两个操作

-   首先，新 api 会调用「旧 api」方法

-   然后，往数组上添加「监听&代理」

>   总结
>
>   -   **尤雨溪（Vue）篡改了数组的 API** （Vue 文档中「[变异方法](https://cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E5%BC%82%E6%96%B9%E6%B3%95-mutation-method)」章节有解释）
>   -   这 7 个 API 都会被 Vue 篡改，调用后会更新 UI

​	

####  具体是怎么篡改的呢 ？📌

>   尤雨溪的思路：就是插入一层原型，原型上有 7 个方法
>
>   +   下面就是「模拟」篡改代码（怎么插入一层原型）

##### ES6 写法

###### demo 测试

>   通过继承的思路，实现添加一层原型

```js
class VueArray extends Array { 
  // 当前 VueArray 继承于 Array ，意思就是 VueArray 在 Array原型 的前面
  push(...args) { // 表示在 VueArray 上声明一个 push 作为共有属性，会放在 VueArray 的 _proto_ 上
    console.log("args", args)
    console.log("arguments", arguments)
    console.log("...args", ...args)
    
    super.push(...args)  // 表示执行 VueArray 的 push 时，会先调用 Array 上的原生 push 方法
    console.log('你 push 了') 
  }
}
const a = new VueArray(1,2.3,4)
console.log(a)
a.push(5) // 往数组添加元素5，同时执行log
console.log("a", a)
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201228185413204.png" alt="image-20201228185413204" style="zoom:67%;" />

>   这样写的作用是
>
>   +   **当我们执行 VueArray 的 push 方法时，效果与原生的 push 一样，但还会多执行一个 console.log**
>   +   第一层原型上只有一个 push 方法，第二层原型才是 Array 的原型

###### 模拟完整逻辑 📌

```js
class VueArray extends Array{ 
  push(...args){
    const oldLength = this.length  // this 就是当前数组（指代new出来的数组实例）记录下旧数组的长度
    super.push(...args) // push之后，this.length 已经更新
    console.log('你 push 了') 
    
    for(let i = oldLength; i < this.length; i++){ 
      Vue.set(this, i, this[i])  
      // 将每个新增的 key 都告诉 Vue ，Vue 得知数据变化、需要添加代理&监听、更新视图啦 
    } 
  }
}
const a = new VueArray(1,2,3,4)
console.log(a)
a.push(5)
```

>   注：此代码仅用于逻辑理解。实际上这种实现方式是比较低效的、不代表 Vue 的真实实现。
>
>   实际上 frank 没看过相关源代码（但尤雨溪的大概思路就是这样的）



##### ES5 写法 - 原型

>   ES5 代码，会比 ES6 更难理解（ES6 相当于给前端降低了门槛）
>
>   +   实现：新插入一层原型，**新原型的上层是实例、下层是数组的原型**
>   +   新原型上有 push 方法：执行一些开发者篡改的操作，然后再调用数组原型的 push 实现添加元素

```js
const vueArrayPrototype = { 
  push: function(){ 
    console.1og('你 push 了') // 这里添加的其他代码，都属于对 push 的篡改
    return Array.prototype.push.apply(this, arguments) // 透传
    // 👆 用户给我传了什么，我把所有东西透透的传给「数组原型上的的push方法」
  }
}
vueArrayPrototype.__proto__ = Array.prototype // 把我的原型指向数组的原型，形成原型链
// 上面这句话用的不是标准属性，仅学习使用 

const array = Object.create(vueArrayPrototype) // 使用我写的新原型，创建出一个数组
array.push(1) 
console.log(array)  // 可以看到三层原型链效果：一层是对象本身、二层是新原型、三层是数组原型

// 如果看不懂就算了，反正面试官也看不懂
```

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201228185413204.png" alt="image-20201228185413204" style="zoom:67%;" />


​	

## 总结

### 对象中新增的 key

-   Vue 没有办法事先监听和代理（导致 key 的变化完全不会影响 UI）
-   要**使用 set** 来新增 key，创建监听和代理，更新 UI 
-   最好提前把属性都写出来，不要新增 key 
-   注意：数组做不到「提前把 key 属性写好」

### 数组中新增的 key

-   数组可用 set 来新增 key，更新 UI
    -   this.$set 作用于数组时，并不会自动添加监听和代理，原因未知（只能问尤雨溪了）
        -   可以测试看看，set 了之后再用 `this.array[n] += 1` 是否会触发 UI 更新（答案是不会）
    -   使用 Vue 提供的数组变异 API 时，会自动添加监听和代理
-   不过尤雨溪篡改了 7 个 API 方便开发者对数组进行**增删**
    -   增，因为 Vue 无法监听到，所以需要修改代码，使 Vue 可以监听到
    -   删，虽然 Vue 已监听到了，但原生 API 直接删除，Vue 是不知道的，就会有多余的监听器，浪费内存，所以删除的 API 也修改一下，实现同时把监听器删除
    -   改、查：这两个操作肯定发生在 Vue 监听的环境中，不需要再帮 Vue 搞额外的处理，直接用原生 API 即可
-   这 7 个 API 会自动处理监听和代理，并（异步的）更新 UI
    -   push、pop、shift、unshift、splice、sort、reverse
    -   前 5 个是必要的，后 2 个可能是为了方便才提供的，用处不算大，当然如果有需求还是使用 Vue 提供的 API，效率更高一些








