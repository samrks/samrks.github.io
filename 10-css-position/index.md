# Re：从零开始の CSS 学习笔记——定位


复习盒模型、div 的分层、新属性 position 详解、层叠上下文 <!--more-->



​	

## 布局和定位的区别

区别大了

+   布局是**屏幕平面**上的
+   定位是**垂直于屏幕**的



​	

## CSS 定位

>   还得从**文档流**和**盒模型**说起（两个非常重要的概念）

​	

## 复习盒模型

margin 外边距、border 边框、padding 内边距、content 内容

background 背景

<img src="https://i.loli.net/2020/07/27/dZI4nGAVN93b62F.png" alt="image-20200727181035065" style="zoom: 67%;" />



### 问两个问题

#### 背景的范围是从哪到哪？

-   【A】border 内边沿围成的区域 ？【B】border 外边沿围成的区域 ？

+   如何验证自己的猜想？
    +   border 半透明试试，答案是【B】
    +   注意：在浏览器的元素样式中，可通过 【alt + 上下方向键】，对半透明数值进行 0.1 的 ± 调整

#### 从左边看一个 div，是什么样子？

>   元素是有层叠关系的，脑内模拟一个三维立体结构，从左边看一个 div，是什么样子？

-   background 在文字后面然后呢？
-   如何验证自己的猜想？用代码证明即可。

```html
<div class="demo">
  <span class=text>你好</span>
</div>

<style>
  .demo{
    box-sizing: border-box;
    border: 5px solid red;
    width: 100px;
    height: 100px;
    background: blue;
  }
  .text{
    font-size: 100px;  /* 让字足够大，测试字能否覆盖住border */
    color: green;
  }
</style>
```

<img src="https://i.loli.net/2020/07/28/Om9zTPDFLcgtYMh.png" alt="image-20200728012210256" style="zoom: 80%;" />

结论：

+   文字在最上层，中间是 border，最底层是 background 背景



​	

​	

## 代码验证：一个 div 的分层

>   三维立体模型 （SketchUp）

+   先给出结论（三维图），再在下面进行代码验证

<img src="https://i.loli.net/2020/07/28/U7B5VhYXxQMDtHm.png" alt="image-20200728012834456" style="zoom:67%;" />

<img src="https://i.loli.net/2020/07/28/bJfeVhgFSMXCpjv.png" alt="image-20200728012916794" style="zoom:67%;" />



​	

### 块级子元素与内联子元素的分层位置

>   让块级元素与内联元素，出现重叠，就可以判断哪个层级更高

```html
<style>
  .demo{
    background: rgb(120,184,211);
    width: 200px;
    height: 200px;
    border: 15px solid red;
    padding: 10px;
  }
  .childDiv{
    background: white;
    height: 50px;
    margin-top: -10px;  /* 测试块级元素与文字的分层位置，将div向上移动，与文字重叠，看是否会遮挡文字 */
  }
</style>

<div class="demo">
  文字内容
  <div class="childDiv"></div>
</div>
```

+   如上，`“文字内容”` 和 `div.childDiv` 属于同级关系
+   结论：
  
    +   块级元素位于文字下层
    
        <img src="https://i.loli.net/2020/07/28/AGs1BEK8aq2XQRr.png" alt="image-20200728013952071" style="zoom:50%;" />



​		

### 浮动子元素与内联子元素的分层位置

```html
<style>
  .demo{ ... }
  .float{
    float: left;
    background: white;
    height: 50px;
    width: 50px;
    margin-right: -10px;  
  	/* 测试浮动元素与文字的分层位置，将浮动元素向右移动，与文字重叠，看是否会遮挡文字 */
  }
</style>

<div class="demo">
  文字内容
  <div class="float"></div>
</div>
```

+   结论：
  
    +   浮动元素位于文字下层
    
        <img src="https://i.loli.net/2020/07/28/2VFuewkUgYM4atb.png" alt="image-20200728014640604" style="zoom:50%;" />



​		

### 浮动子元素与块级子元素的分层位置

```html
<style>
  .demo{...}
  .float{...}
  .childDiv{
    height: 50px;
    background: orange;
    margin-top: -10px;
  }
</style>

<div class="demo">
  文字内容
  <div class="float"></div>
  <div class="childDiv"></div>
</div>
```

+   结论：
    +   浮动内容（脱离文档流），高于 childDiv（普通文档流元素），低于文字
    +   所以，==脱离文档流==，实际上就是从普通文档流中跳出，**比文档流的层级稍微高一点**
    
        <img src="https://i.loli.net/2020/07/28/wXLSsQIEJGA8adZ.png" alt="image-20200728015413212" style="zoom:50%;" />



​	

### 结论

+   文字内容层级最高，代码写在下面的文字内容，会覆盖上面的
+   只有浮动元素会**脱离文档流**（从普通文档流中跳出），比普通文档流的层级稍高

    ![image-20200728012834456](https://i.loli.net/2020/07/28/U7B5VhYXxQMDtHm.png)

    ![image-20200728012916794](https://i.loli.net/2020/07/28/bJfeVhgFSMXCpjv.png)



​	

​	

## 新属性 position

### position 取值

-   static 默认值，当前元素待在文档流里

-   relative 相对定位，升起来，但不脱离文档流

-   absolute 绝对定位，定位基准是相对于祖先里的非 static 祖先进行定位

-   fixed 固定定位，定位基准是 viewport 视口

    -   有诈：后来 css 出的 transform 属性，使 fixed 无法相对 viewport 视口定位，这就是 CSS 的不正交

-   sticky 粘滞定位，不好描述直接举例 ↓↓
  
    -   特别适合做导航：正常状态会存在于文档流中，当向下滚动到 sticky 元素且 sticky 元素即将移出视口时，sticky 元素会始终保持在视口顶部，不会随文档流继续滚动。
      
    -   兼容性特别差，可以在 [caniuse](https://caniuse.com/#search=sticky) 中查询。 （黄色是需要加前缀or部分支持）
    
        

### 经验

-   如果你写了 absolute，一般都得补一个 relative
-   如果你写了 absolute 或 fixed，一定要补 top 和 left
-   sticky 兼容性很差，主要用于**面试装逼**



​	

### position: relative

#### 使用场景

-   用于做位移（很少用），将两个元素对齐
-   用于给 absolute 元素做爸爸

#### 配合 z-index

-   `z-index: auto` 默认值，不创建新[层叠上下文](#什么是层叠上下文)
-   `z-index: 0/1/2`
-   `z-index: -1/-2`

#### 经验

-   写 `z-index: 9999` 的都是 菜B
-   要学会管理 z-index



​	

​	

### position: absolute

####  使用场景

-   脱离原来的位置，另起一层，比如**对话框的关闭按钮**，通常是通过绝对定位实现的

-   鼠标提示

    ```html
    <div style="height:100px;"></div>
    <button>
      点击
      <span class="tips">提示内容</span>
    </button>
    
    <style>
      button{ position: relative; }
      button span{
        position: absolute;
        border: 1px solid red;
        white-space: nowrap;
        bottom: calc(100% + 10px);
        left: 50%;
        transform: translateX(-50%);
      }
      button span{ display: none; }
      button:hover span{ display: inline-block; }   /* 鼠标悬浮时，显示span */
    </style>
    ```

    

#### 配合 z-index

#### 经验

-   很多 菜B 都以为 absolute 是相对于 relative 定位的

    -   **absolute 是相对于 祖先元素中最近的一个定位元素**
        -   只要 position 属性值不是 static 的元素，就是定位元素

-   某些浏览器上，如果写了 absolute，不写 top / left / bottom / right 会位置错乱  （4个至少写2个）

    ```css
    .demo{
      position: absolute;
      top: 0;
      left: 0;
    }
    ```

-   善用 `left: 100%` （效果通常是该元素会出现在定位元素的最右边）

-   善用 `left: 50%;` + 加负 margin （负宽度的一半）：实现居中

    -   也可以用  `left: 50%;` + `transform: translateX(-50%);`  实现，优点是不需要自己计算宽度的一半



​	

​	

### position: fixed

>   fixed 是相对于【视口】定位的
>
>   +   视口，就是浏览器中普通用户能看到的部分（不包括滚动条）

#### 使用场景

-   烦人的广告
-   回到顶部按钮

#### 配合 z-index

#### 经验

+   如果 fixed 元素所在的容器，具有某些属性，可能会导致 fixed 元素不再相对于视口定位。

    +   ```css
        .container{ position: relative; }
        .container > .fixed{ position: fixed; left: 10px; bottom: 10px; }  
        .container{ transform: scale(0.9); } /* 此时fixed元素就不再相对于视口定位 */
        ```

    +   总结：**不要把 fixed 元素放到 具有 transform 属性的容器中**，可能会产生奇怪的bug（不正交：调这个元素却影响了另一个元素）

-   **手机上尽量不要用这个属性**，坑很多，无穷无尽的bug。不信你搜索一下「 移动端 fixed 」
    -   用了，可能就开始996了。一个bug可能引出10个bug





​	

​	

##  层叠上下文

### div分层 与 position

>   +   任意一个元素的 position 取值非 static 时，就成为了定位元素。定位元素的层级，立马超越内联文字，成为最上层元素（跑到所有元素最上层）
>   +   z-index，默认是 auto
>       +   所有定位元素，会以内联文字层为底层，随着 z-index 递增，层级也会递增
>       +   如果 z-index 为负值，则层级比 background 还低
>       +   （可以无限向下，但是不能低于层叠上下文，也就是不能低于 html ，低于html还有什么意义）

![image-20200728221925366](https://i.loli.net/2020/07/28/8kLeWB4txjPSdCJ.png)



### 什么是层叠上下文

>   也叫堆叠上下文
>
>   层叠上下文，会把所有元素包起来。
>
>   +   默认的层叠上下文，是 html 元素，html 就是会把所有元素包起来
>
>   +   其他元素，也会因为拥有某些属性而变成层叠上下文（导致 z-index 需要重新计算）

​	

#### 比喻

>   层叠上下文对 z-index 的影响

-   每个层叠上下文就是一个新的小世界（作用域）
-   这个小世界里面的 z-index 跟外界无关
-   **处在同一个小世界的 z-index 才能比较**
-   [JSBin 示例](http://js.jirengu.com/gewob/1/edit?html,css,output) 
    -   a 和 b2 处于同一个层叠小世界，由 container 创造的，如果 container 没有设定 z-index，那么 a 和 b2 就同处于 html 的作用域里面，是具有可比性的。



​	

#### 哪些不正交的属性可以创建层叠上下文

>    最简单的就是定位元素设置 z-index = 0 ，就会创建一个层叠上下文

-   [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)有写

    1.  文档根元素 html 自成一个层叠上下文
    2.  z-index 不为 auto 的定位元素，会开启一个小世界（作用域）
    3.  元素的 opacity 小于1，就会创建一个层叠上下文（因为 opacity 会影响内部所有元素）
    4.  `position: fixed;` 一定会创建层叠上下文
    5.  …

    +    需要记忆的有：
         +    z-index=0时会创建层叠上下文
         +    flex元素内部会形成层叠上下文
         +    opacity 不为1 ，就会创建层叠上下文
         +    具有 transform 的元素，会形成一个 层叠上下文

-   **知道这些知识的面试官也不太多，不用花时间背**

-   忘了就搜「 层叠上下文 MDN 」

-   你说 CSS 为什么不单独创建一个属性做这个事

    ```css
    例如： xxxdisabled: true; /* true就创建层叠上下文，false就不创建；css并没有这种直接功能开关*/
    ```

    +   这就是css的难学之处，互相交叉影响，错综复杂（不正交）



​	

​	

### 负 z-index 与层叠上下文的关系

>   [JSBin 示例](http://js.jirengu.com/modez/1/edit?html,css,output)
>
>   +   .demo 的 z-index = -1，会使 demo 出现在文档流的 bg 的后面
>   +   z-index 是默认 auto 时，不具有层叠上下文

>   记住 ：负 z-index 逃不出 层叠上下文的小世界



```html
<div class="container">
  <div class="demo"></div>
</div>
```

```css
.container{
  background: rgba(0,255,255,0.5);
  height: 200px;
  position: relative;
  /* z-index 是默认 auto 时，不具有层叠上下文，所以添加z-index=0，创建层叠上下文 */
  z-index: 0; /* 注释这行看看 */    
}
.container > .demo{
  width: 50px;
  height: 50px;
  background: red;
  position: absolute;
  z-index: -1; /* 因为上面z-index:0创建了层叠上下文，所以demo无法逃出这个小世界，不可能比container低 */
}
```

<img src="https://i.loli.net/2020/07/28/M9O8q7XEantTBUQ.png" alt="image-20200728231218006" style="zoom:50%;" /><img src="https://i.loli.net/2020/07/28/aeQfNxSnglBwEkM.png" alt="image-20200728231201250" style="zoom:50%;" />

图一：当 container 不是层叠上下文（也就是没有 z-index: 0; ）时，demo 是 `z-index = -1` 存在于容器bg之下

图二：当 container 创建层叠上下文，demo 就无法跳出容器之外

​	

总结：

>   z-index = -1  不一定存在于容器的背景之下。当这个容器是一个层叠上下文时，就不可能存在于容器之外




























































