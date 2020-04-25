# DOM 事件机制 & 事件委托


<!--more-->

​	

## DOM 事件流

>   为什么有「事件流」？

假如在一个button上注册了一个click事件，又在它的 父元素 div 上注册了一个 click 事件，那么当我们点击 button，是先触发父元素上的事件，还是button上的事件呢，这就需要一种约定去规范事件的执行顺序，就是事件执行的流程。

浏览器在发展的过程中出现了两种不同的规范

-   IE 9（微软） 以下的 IE 浏览器使用的是事件冒泡，先从具体的接收元素，然后逐步向上传播到不具体的元素。
-   Netscape（网景） 采用的是事件捕获，先由不具体的元素接收事件，最具体的节点最后才接收到事件。
-   而 W3C（万维网）制定的 Web 标准中，是同时采用了两种方案，事件捕获和事件冒泡都可以。

​	

## 事件的传播

>   又称「事件机制」 或 「事件模型」

一个事件发生后，会在子元素和父元素之间传播（propagation）。这种传播分成三个阶段。

>   +   第一阶段：从window对象传导到目标节点（上层传到底层），称为“捕获阶段”（capture phase）。
>   +   第二阶段：在目标节点上触发，称为“目标阶段”（target phase）。
>   +   第三阶段：从目标节点传导回window对象（从底层传回上层），称为“冒泡阶段”（bubbling phase）。

这种三阶段的传播模型，使得同一个事件会在多个节点上触发。

### 1.事件捕获

>   捕获是**从上到下**。

事件传播的最上层对象是 window，接着依次是 document，html（document.documentElement）和body（document.body），然后按照普通的 html 结构一层一层往下传，最后到达目标元素。

我们只需要将 addEventListener 的第三个参数改为 true ，就可以实现事件捕获。

### 2.事件冒泡

>   冒泡是**从下到上**。

所谓事件冒泡就是事件像泡泡一样从最开始生成的地方一层一层往上冒，越来越大。从目标元素开始，一层层往上传，最后经过 body、html 到达 window 结束。

addEventListener 默认就是把事件绑定在冒泡阶段（第三个参数空着或者传 falsy 值 ）。



<img src="http://imgsubmit.oss-cn-beijing.aliyuncs.com/img/image-20201027205345936.png" alt="image-20201027205345936" style="zoom:67%;" />

### 示例

```html
<div>
  <p>点击</p>
</div>
```

上面代码中，`<div>` 节点之中有一个``<p>``节点。

如果对这两个节点，都设置click事件的监听函数（每个节点的捕获阶段和监听阶段，各设置一个监听函数），共计设置四个监听函数。然后，对``<p>``点击，click事件会触发四次。

```js
var phases = {
  1: 'capture',
  2: 'target',
  3: 'bubble'
}

var div = document.querySelector('div')
var p = document.querySelector('p')

div.addEventListener('click', callback, true)
p.addEventListener('click', callback, true)
div.addEventListener('click', callback, false)
p.addEventListener('click', callback, false)

function callback(event) {
  var tag = event.currentTarget.tagName
  var phase = phases[event.eventPhase]
  console.log("Tag: '" + tag + "'. EventPhase: '" + phase + "'")
}

// 点击以后的结果
// Tag: 'DIV'. EventPhase: 'capture'
// Tag: 'P'. EventPhase: 'target'
// Tag: 'P'. EventPhase: 'target'
// Tag: 'DIV'. EventPhase: 'bubble'
```

上面代码表示，click事件被触发了四次：`<div>`节点的捕获阶段和冒泡阶段各1次，``<p>``节点的目标阶段触发了2次。

捕获阶段：事件从`<div>`向``<p>``传播时，触发`<div>`的click事件；
目标阶段：事件从`<div`>到达``<p>``时，触发``<p>``的click事件；
冒泡阶段：事件从``<p>``传回`<div>`时，再次触发`<div>`的click事件。
其中，``<p>``节点有两个监听函数（addEventListener方法第三个参数的不同，会导致绑定两个监听函数），因此它们都会因为click事件触发一次。所以，``<p>``会在target阶段有两次输出。

注意，浏览器总是假定click事件的目标节点，就是点击位置嵌套最深的那个节点（本例是`<div>`节点里面的`<p>`节点）。所以，`<p>`节点的捕获阶段和冒泡阶段，都会显示为target阶段。

事件传播的最上层对象是window，接着依次是document，html（document.documentElement）和body（document.body）。也就是说，上例的事件传播顺序，在捕获阶段依次为window、document、html、body、div、p，在冒泡阶段依次为p、div、body、html、document、window。	



### 一个特例

```html
<div id=ele>点我</div>
```

```js
// 👇先监听冒泡阶段的点击事件
ele.addEventListener('click', ()=>{
  console.log('2')
})
// 👇再监听捕获阶段的点击事件
ele.addEventListener('click', ()=>{
  console.log('1')
}, true)

// 点击div以后的结果
// 2  （冒泡）
// 1  （捕获）
```

```js
// 👇先监听捕获阶段的点击事件
ele.addEventListener('click', ()=>{
  console.log('1')
}, true)
// 👇再监听冒泡阶段的点击事件
ele.addEventListener('click', ()=>{
  console.log('2')
})

// 点击div以后的结果
// 1   （捕获）
// 2   （冒泡）
```

>   点击触发后的结果：与点击事件绑定在哪个阶段并无直接关系，而是谁写在前，谁先执行

当只有一个单一的元素被监听时（不存在父子元素关系），分别在捕获和冒泡两个阶段，监听这个元素的点击事件。这种情况下，点击事件被触发后，则不再遵循「先捕获后冒泡」的机制，而是「谁先监听，谁先执行」



## addEventListener 👂

### 事件绑定 API

```js
baba.attachEvent('onclick', fn)  // 微软IE5发明：默认进入冒泡阶段
baba.addEventListener('click', fn)  // 网景发明：默认进入捕获阶段
baba.addEventListener('click', fn, bool)·// ❤️W3C标准：加了参数 bool，用于指定让函数运行在哪个阶段
```

#### 如果 bool 不传 （或为 [falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)）

+   默认情况
+   就让 fn 走**冒泡**，即当浏览器在冒泡阶段发现 baba 有 fn 监听函数，就会调用 fn，并提供事件信息
+   大多数人习惯上都不会传这个参数（可见 W3C 可能更倾向于 IE 的方案：默认把 fn 放在冒泡阶段）

#### 如果 bool 为 true

+   就让 fn 走**捕获**，即当浏览器在捕获阶段发现 baba 有 fn 监听函数，就会调用 fn，并提供事件信息

​	

### 补充：事件移除

>   removeEventListener

通过 addEventListener() 添加的事件只能用 **removeEventListener()** 来移除

-   移除时，传入的参数与添加事件使用的参数相同
-   通过 addEventListener() 添加的匿名函数无法删除

```js
var btn = document.getElementById("myBtn")
btn.addEventListener("click", function () {
  alert(this.id)
}, false)
btn.removeEventListener("click", function () { // 匿名函数无法移除
  alert(this.id)
}, false)
```

```js
var btn = document.getElementById("myBtn")
var handler = function () {
  alert(this.id)
}
btn.addEventListener("click", handler, false)
btn.removeEventListener("click", handler, false)  // 有效！
```

​	

​	

## target 🆚 currentTarget

>   [Event.currentTarget](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/currentTarget) 找到事件**绑定**的元素。
>
>   区别与 [Event.target](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/target) ，是事件**触发**的元素。

### 区别

>   一个是用户点击的（触发事件的元素），一个是开发者监听的（事件绑定的元素）

-   e.target ：用户操作的元素 
-   e.currentTarget ：程序员监听的元素 
-   this 是 e.currentTarget，非常不推荐在监听代码里使用 this（因为经常会忘记  this 到底指代哪一个）

### 举例

```html
<div>
  <span>文字</span>
</div>
```

-   给 div 绑定点击事件，用户点击“文字”
-   e.target 就是 span 
-   e.currentTarget 就是 div

​	

​	

## 阻止默认事件

>   默认事件，又称「默认动作」「默认行为」
>
>   例如：表单一点击提交按钮(submit)就会刷新页面、点击a标签默认执行页面跳转或是锚点定位等。

```js
event.preventDefault()
```

如果调用这个方法，默认事件行为将不再触发。

### 使用场景1

>   使用a标签仅仅是想当做一个普通的按钮，点击实现一个功能，不想页面跳转，也不想锚点定位。

#### 方法一

```html
<a href="javascript:;">链接</a>
```

#### 方法二

使用 JS 方法来阻止：当我们点击A标签的时候，会先触发click事件，其次才会执行自己的默认行为。所以只需给其 click 事件 return false ，让执行中断

```html
<a id="test" href="http://www.google.com">链接</a>
<script>
  test.onclick = function(e){
    e = e || window.event  // 兼容不同浏览器
    return false
  }
</script>
```

[^window.event]:  IE 6/7/8的事件对象只能在 window上获取，[详解](https://www.jianshu.com/p/e8a6fad0f7bc)

#### 方法三

```html
<a id="test" href="http://www.google.com">链接</a>
<script>
  test.onclick = function(e){
    e = e || window.event
    e.preventDefault()
  }
</script>
```

### 使用场景2

>   限制输入框最多只能输入六个字符，如何实现？

```html
<input type="text" id='tempInp'>
<script>
  tempInp.onkeydown = function(ev) {
    ev = ev || window.event
    let val = this.value.trim() // trim去除字符串首尾空格（不兼容）
    // this.value = this.value.replace(/^ +| +$/g,'') 兼容写法
    let len = val.length
    if (len >= 6) {
      this.value = val.substr(0, 6);
      // 阻止默认行为去除特殊按键（DELETE\BACK-SPACE\方向键...）
      let code = ev.which || ev.keyCode  // 当前按下的按键的code码
      if (!/^(46|8|37|38|39|40)$/.test(code)) {  // 如果按下的是特殊按键，则阻止默认事件（按下无效）
        ev.preventDefault()
      }
    }
  }
</script>
```

​	

## 阻止事件传播

>   阻止事件进一步的 冒泡 / 捕获

```js
e.stopPropagation() 
```

### 示例

```html
<div class="level1">
  <div class="level2">
    <div class="level3">
			点我
    </div>
  </div>
</div>
```

```js
const level1 = document.querySelector('.level1')
const level2 = document.querySelector('.level2')
const level3 = document.querySelector('.level3')

level1.addEventListener('click', (e)=>{
  console.log("1")
}) 
level2.addEventListener('click', (e)=>{
  console.log("2")
}) 
level3.addEventListener('click', (e)=>{
  console.log("3")
  e.stopPropagation()  // 阻止冒泡，输出： 3
})

// 不阻止冒泡，点击文字，输出顺序： 3  2  1
```

```js
level1.addEventListener('click', (e)=>{
  console.log("1")
}, true) 
level2.addEventListener('click', (e)=>{
  console.log("2")
  e.stopPropagation()  // 阻止捕获，输出： 1  2
}, true) 
level3.addEventListener('click', (e)=>{
  console.log("3")
}, true)

// 不阻止捕获，点击文字，输出顺序： 1  2  3
```

​	

## 插曲：如何阻止滚动 🖱️

### scroll 不支持阻止默认事件

>   MDN 搜索 [scroll event](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/scroll_event) 
>
>   滚动事件，不能阻止默认事件。 那怎么让页面无法滚动呢？

### 解决办法

+   要阻止滚动，可阻止 wheel（鼠标滚轮） 和 touchstart（移动端触屏） 的默认动作
+   拖拽滚动条，还能实现滚动，所以还需要隐藏滚动条

### 代码

```html
<div id=x>
  <p>1</p>
  <p>2</p>
  <p>3</p>
  <p>4</p>
  ...  
  <p>100</p>
  <!-- p标签撑起页面，超出一屏高度，出现滚动条 -->
</div>
```

```html
<script>
  // PC 端
  x.addEventListener('wheel', (e)=>{  // 绑定滚轮事件 wheel，触发滚轮事件，就阻止执行
    console.log(2)
    e.preventDefault()
  })
  // 移动端
  x.addEventListener('touchstart', (e)=>{  // 手机端是触屏拖拽滚动，那就阻止touchstart触屏事件
    e.preventDefault()
  })
</script>
```

```html
<style>
  ::-webkit-scrollbar { width: 0 !important }   /* 隐藏滚动条 */
</style>
```

-   注意：你需要找准滚动条所在的元素（在 document 上）
-   用 overflow: hidden 也可以直接取消滚动条。但此时 JS 依然可以修改 scrollTop

​	

## 浏览器自带事件

-   一共 100 多种事件，[列表](https://developer.mozilla.org/zh-CN/docs/Web/Events) 在MDN上
-   用户打印、写字、全屏、复制粘贴、键盘按键、点击鼠标、拖放事件、媒体事件（比如直播：被播放、关闭、暂停、加速）…

+   非常多的事件，都可以被监听。
+   想一下全部理解，是不可能的。用到再查就可以

| 事件名称     | 应用场景                                      |
| ------------ | --------------------------------------------- |
| error        |                                               |
| abort        | 中止事件                                      |
| load         | 加载成功事件                                  |
| beforeunload | 关闭页面事件                                  |
| unload       | 关闭页面之后的事件                            |
| online       | 网络连上了，触发 online （从没用过）          |
| offline      | WiFi 网络突然断了，触发 offline（从没用过）   |
| focus        | 一个元素获取焦点                              |
| blur         | 一个元素失去焦点                              |
| pageshow     | 一个页面显示出来，会触发pageshow （从没用过） |
| submit       | 提交                                          |
| beforeprint  | 用户打印                                      |
| afterprint   | 用户打印                                      |
| …            | …                                             |

​	

## 自定义事件

>   开发者可以在【浏览器自带事件】之外，自定义一个事件

HTML

```html
<div id="div1">
  <button id="btn">
	  点击触发sam事件
	</button>
</div>
```

JS

```js
btn.addEventListener('click', () => {
  // new出自定义事件，new CustomEvent('事件名', 事件信息)
  const event = new CustomEvent('sam', { 
    detail: { name: 'sam', age: 18 }
  })
  // EventTarget.dispatchEvent(event) 触发事件
  btn.dispatchEvent(event) 
})
// 现在效果：点击 btn ，触发 sam 事件

// 监听 sam 事件
btn.addEventListener('sam', (e) => { 
  congsole.log('sam事件触发了')
  console.log(e.detail)
})
```

### 自定义事件，会冒泡吗？

+   测试：只监听 div1 的点击事件。看看点击 btn，会触发到 div1 的点击事件吗？
+   结果：不行，不冒泡。

```js
// 监听 div1 的 sam 事件
div1.addEventListener('sam', (e) => { 
  congsole.log('sam事件触发了')
  console.log(e.detail)
})
```

+   如果想实现自定义事件的冒泡，还需额外再给自定义事件 **开启冒泡属性**

```js
btn.addEventListener('click', () => {
  const event = new CustomEvent('sam', { 
    detail: { name: 'sam', age: 18 },
    bubbles: true  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 开启冒泡，添加这句就可以了
    // cancelable: false  // 是否可以阻止默认事件
  })
  btn.dispatchEvent(event) 
})
```





## 事件委托

>   又称「事件代理」

由于事件会在冒泡阶段向上传播到父节点，因此可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件。这种方法叫做事件委托（代理）。

### 使用场景 1

假设有一个列表，列表之中有**大量的子项**，我们需要在点击每个子项的时候响应一个事件

```html
<ul id="list">
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
  ......
  <li>item n</li>
</ul>
```

```js
div1.addEventListener('click', (e) => {
  const t = e.target
  if(t.tagName.toLowerCase() === 'li'){
    console.log('li 被点击了')
    console.log('li 内容是：' + t.textContent)
  }
})
```

-   如果给每个子项都绑定一个函数，那对于内存消耗是非常大的，效率上需要消耗很多性能。
-   借助事件委托，我们只需要给父容器 ul 绑定方法即可
-   这样不管点击的是哪一个后代元素，都会根据冒泡传播的传递机制，把容器的 click 行为触发，然后把对应的方法执行，根据事件源，我们可以知道点击的是谁，从而完成不同的事。

### 使用场景 2

-   在很多时候，我们需要通过用户操作**动态的新增子项元素**。
-   在最初并没有新增子项元素时，就无法给还未创建的子项元素绑定事件
-   这种情况就可以采用事件委托的形式，给父级元素绑定事件，监听到子项的动态变化。

[示例](http://js.jirengu.com/wuwox/1/edit?html,js,output)

```html
<button id="btn">新增按钮</button>
<ul id="div1">
  
</ul>
```

```js
let n = 0
btn.onclick = (e) => {
  n += 1
  const button = document.createElement('button')
  button.innerText = '按钮' + n
  div1.appendChild(button)
}

list.addEventListener('click', (e) => {
  const t = e.target
  if(t.tagName.toLowerCase() === 'button'){
    console.log('当前点击的是：' + t.innerText)
  }
})
```

​	

### 优点

-   减少内存消耗，提高性能  （例1）
    -   如果要监听100个按钮，需要100个监听器，就是100倍的内存。如果之间一个祖先 div，就是只需要一个监听器，节约了99个
-   可以监听动态的元素  （例2）
    -   如果当前元素还不存在，肯定没法直接监听到。只能监听祖先





### 封装事件委托

>   封装，需要考虑更多边界情况
>
>    -   写出这样一个函数 `on('click', '#testDiv', 'li', fn)`
>
>   -   当用户点击 `#testDiv` 里的 `li` 元素时，调用 `fn` 函数

[示例](http://js.jirengu.com/kuxeg/3/edit?html,js,output)

```html
<button id="btn">新增按钮</button>
<ul id="div1">

</ul>
```

```js
let n = 0
btn.onclick = (e) => {
  n += 1
  const button = document.createElement('button')
  const span = document.createElement('span')
  span.innerText = '按钮' + n
  button.appendChild(span)
  div1.appendChild(button)
}

on('click', '#div1', 'button', fm)
function fm(e,el){  
  // 不能用箭头函数，this 会获取不到 el
  // 箭头函数中的 this，只能获取到 window
  console.log(e)
  console.log(el)
  console.log(el.innerText)
  console.log(this)
}

function on(eventType, element, selector, fn){
  if(!(element instanceof Element)){
    element = document.querySelector(element)
  }
  element.addEventListener(eventType, (e) => {
    let el = e.target
    // 只要el不匹配，就不断获取el的父元素来匹配，直到el获取element，说明容器中压根没有匹配的el，结束循环
    // el 为 null，则不执行 fn
    while(!el.matches(selector)){ 
      if(el === element){  // 循环结束条件
        el = null
        break
      }
      el = el.parentNode
    }
    el && fn.call(el, e, el)
  })
  return element
}
```



## 答疑

### JS 支持事件吗

#### 答

>   不支持。因为 JS 本身没有「事件」（只是调用了 DOM 提供的 addEventListener）

-   本节内容的 DOM 事件，不属于JS 的功能。
    -   术语：本节内容是基于浏览器提供的 DOM 的功能 
    -   JS 是浏览器的功能之一。DOM 事件也是浏览器的功能之一（**二者是平行的关系，没有从属关系**）
    -   JS 里面没有 DOM 事件， JS 只是调用了 DOM 提供的 addEventListener 而已

>   因为 DOM 提供了 事件的功能，还提供了一整套完整的事件机制（捕获冒泡、默认动作、event 对象…）
>
>   所以 JS 才可以用

#### 追问

>   由于 JS 不支持事件，面试官可能问你「能不能手写出一个 JS 事件系统」

-   如何让JS支持事件？请手写一个事件系统。
-   目前大家的水平还写不出来，可以先思考一段时间。
    （可以搜一搜、实际上也不难，用一个「队列」就可以遭到了）



>   以上。本节就是对 **DOM事件（不是 JS 事件）** 的一个完整了解



## 参考

[阮一峰：事件模型](https://javascript.ruanyifeng.com/dom/event.html#toc10)

[深入理解DOM事件机制](https://juejin.im/post/6844903781969166349#heading-19)

[e = e || window.event](https://www.jianshu.com/p/e8a6fad0f7bc)


