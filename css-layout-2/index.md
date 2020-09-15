# CSS 学习笔记——布局（下）


CSS 布局篇（下）：Grid 布局。  <!--more-->

​	

##  如何在两套布局中切换

>   一个css中，写两种布局，使用 @media 媒体查询来切换

​		

## Grid 布局

>   Grid 布局兼容性并不好，在一两年内都可能不会非常广泛的使用（现在可以不用学的那么仔细）
>
>   但功能确实非常强大

+   flex 更擅长 一维布局，要么横着布局，要么竖着布局
+   一维布局用 Flex



​	

## 二维布局用 Grid

>   以下内容主要来自 CSS Tricks 的一篇文章  [A Complete Guide to Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

+   查看本地代码



​	

### Grid 也分 container 和 items

>   分别记忆

### 成为 container

```css
.container{
	display: grid | inline-grid;
}
```

### 行和列

>   类似 table 表格（grid 被称为新时代的表格）

```css
.container{
	display: grid;
  grid-tamplate-columns: 40px 50px auto 50px 40px;   /* 每列宽度（有n个值对应生成n列） */
    			         /*  第1列 第2列 第3列 第4列 第5列  */
  
  grid-tamplate-rows: 25% 100px auto;   /* 每行高度（有n个值对应生成n行） */
                 /*  第1行 第2行 第3行  */
}
```

<img src="https://i.loli.net/2020/07/24/G3hdyrkeIbHioF6.png" alt="image-20200724111926579" style="zoom:67%;" />

>   [1]  [2]  [3]  …  [6]   指的是线的序号。用于划分 items  的范围（[具体使用见 items 部分 ](#第二步)）

​	

#### 缩写  grid-template

>   `grid-template`是`grid-template-rows`（行高）和`grid-template-columns`（列宽）的缩写形式。
>
>   比如说，`grid-template: 50% 50% / 200px;`将创建一个具有两行的网格，每一行占据50%，以及一个200像素宽的列。

```css
/* 需要包括你的花园上部的60%，以及左侧的200像素。 */
#garden {
  display: grid;
  grid-template: 60% 40% / 200px calc(100% - 200px) ;
  			 	/*	行高↑↑       列宽↑↑   */
}

#water {
  grid-column: 1;
  grid-row: 1;
}
```

<img src="https://i.loli.net/2020/07/26/yaqIGO7DmrYiSR8.png" alt="image-20200726232019709" style="zoom: 33%;" />

```css
/* 你的花园看起来很棒。现在，你在花园的底部留下了50像素的小路，其他的空间用来种植胡萝卜。
不幸的是，胡萝卜地的20%已经杂草丛生了，最后一次用CSS网格布局来规划你的花园吧！ */
#garden {
  display: grid;
  grid-template: calc(100% - 50px) 50px/20% 80%
}
```

<img src="https://i.loli.net/2020/07/26/A5mpIzgOEK4Q3HZ.png" alt="image-20200726232555266" style="zoom:33%;" />



​	

### 指定 items 布局

#### 第一步

>   +   搭出大框架：几行几列
>
>   +   有几个区域，就建立几个 items

```html
<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .container {
    display: grid;
    grid-template-columns: 40px 50px auto 50px 40px;   /* 5列（auto会撑满该行余下空间） */
    grid-template-rows: 100px 300px 100px;  /* 3行（每行高度累加，成为container的总高度） */
    border: 3px solid red;
  }
  .a, .b, .c, .d, .e {
    border: 3px solid black;
  }
</style>

<div class="container">
  <!-- emmet快捷键: 键入.a+.b+.c+.d ——> tab 创建出5个div，class分别为abcde -->
  <div class="a"></div>  <!--第1行, 第1列-->
  <div class="b"></div>  <!--第1行, 第2列-->
  <div class="c"></div>  <!--第1行, 第3列-->
  <div class="d"></div>  <!--第1行, 第4列-->
  <div class="e"></div>  <!--第1行, 第5列-->
</div>
```

+   container 中的子元素，**与列数一一对应**。且默认只占据第一行。所以添加 border 后 效果如下图所示

<img src="https://i.loli.net/2020/07/24/Cde3yXrMSaN7pBI.png" alt="image-20200724115703077" style="zoom: 33%;" />

#### 第二步

>   给每个 item 指定区域、设置范围

##### grid-xxx-start / grid-xxx-end

>   grid-row-start、grid-row-end
>
>   grid-column-start、grid-column-end

```css
.a{
  grid-row-start: 1;  /* a从哪条线开始 */     
  grid-row-end: 3;    /* a到哪条线结束 */         
  /* 指定的两条【线】的中间区域，就是a所占的【行】 */

  grid-column-start: 1;   
  grid-column-end: 2;
  /* 指定的两条【线】的中间区域，就是a所占的【列】 */
}
```

##### grid-row / grid-column

>   grid-row：grid-row-start / grid-row-end;
>
>   grid-column:  grid-column-start / grid-column-end;

```css
/* 上面写法，可以缩写成 ↓ */
.a{
  grid-row: 1/3 ;
  grid-column: 1/2 ;   
}
```

##### grid-area

>   grid-area属性接受4个由'/'分开的值：grid-row-start, grid-column-start, grid-row-end, 最后是 grid-column-end。
>
>   ​																					起始行 ↑↑            起始列 ↑↑           终止行 ↑↑                      终止列 ↑↑

```css
/* 再进一步缩写 */
.a{
  grid-area: 1/1/3/2 ;
}
```

+   最终效果

    <img src="https://i.loli.net/2020/07/24/TmcNryw4v21Qsbh.png" alt="image-20200724164701012" style="zoom:50%;" />



​	

#### 可以给线起名字

>   但没必要，代码太麻烦。就按照序号就很好

![image-20200724161013733](https://i.loli.net/2020/07/24/MpyEAgk7OU2RDfV.png)

取名结果：如图↘

<img src="https://i.loli.net/2020/07/24/boOXmVrD7AEGZTM.png" alt="image-20200724161116535" style="zoom: 50%;" />

item可以设置范围，直接用线的名称

```css
.item-a{ 
  grid-column-start: 2; 
  grid-column-end: five;
  grid-row-start: rowl-start; 
  grid-row-end: 3;
}
```

---



​	

​	

### fr ：单位【份】

>   容器属性

>   全称  free space   自由空间

```html
<div class="container">
  <div class="a">a</div>
  <div class="b">b</div>
  <div class="c">c</div>
  <div class="d">d</div>
  <div class="e">e</div>
  <!-- <div class="f">f</div> -->
</div>
```

```css
<style>
* {margin: 0;padding: 0;box-sizing: border-box;}
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;   /* fr=份，与flex的份类似：共3列每列占1份，也就是均分 */
  grid-template-rows: 1fr 1fr;  /* 共2行，每行占1份，均分 */
  border: 3px solid red;
  min-height: 500px;
}
.a, .b, .c, .d, .e {
  border: 3px solid black;
}
.e{
  grid-column-start: 2;    /* 再单独定义e版块占两列空间 */
  grid-column-end: 4;
  background-color: #ccc;
}
</style>
```

<img src="https://i.loli.net/2020/07/24/pI3EUHFDSj6Ld4z.png" alt="image-20200724170500772" style="zoom: 50%;" />

​	

#### 变形

```css
.container {
  display: grid;
  grid-template-rows: 1fr 3fr;   /* 2行：第2行占3份 */
  grid-template-columns: 1fr 2fr 1fr;   /* 3列：第2列占2份 */
  ...
}
```

<img src="https://i.loli.net/2020/07/24/NxoGF8VMJu9Styh.png" alt="image-20200724171954273" style="zoom:50%;" />

​	

#### 平均布局 grid-gap

>   fr 主要用于实现平均布局

>   容器属性：grid-gap  用于控制间距

```html
<div class="container">
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <!-- 应用下面grid布局样式，可实现任意添加.image，都可以实现一排4个、间距12px的布局 -->
</div>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .container{
    margin-right: auto;
    margin-left: auto;
    margin-top: 30px;
    outline: 5px solid red;
    width: 800px;
    /* 开启grid布局 */
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;  /* 格子布局：1排4个 */
    grid-template-rows: 1fr 1fr;   
    grid-gap: 12px;  /* 计算每项边距，自动计算位置，四边上的会紧贴 */
  }
  .container>div{
    border: 2px solid black;
    background-color: #ccc;
    height: 191px; /* 宽800px，1排4个产品，计算得出：每个产品可占宽191px，则间距为12px */
    width: 191px;
  }
</style>
```

<img src="https://i.loli.net/2020/07/25/3ruQI8siWBp1oqZ.png" alt="image-20200725210126270" style="zoom: 33%;" />



​	

###  分区 grid template-areas

>   名字很长，但非常好用

```html
<div class="container">
  <header>header</header>
  <aside>aside</aside>
  <main>main</main>
  <div class="ad">ad</div>
  <footer>footer</footer>
</div>

<style>
  .container {
    min-height: 100vh;   /* 最小高度为占满整个屏幕 */
    display: grid;
    grid-template-rows: 60px auto 60px;  /* 行高 */
    grid-template-columns: 190px auto 100px;    /* 列宽 */
    /* 布局：以版块名称直接写出布局、位置 */
    grid-template-areas:  
      "header header header"
      "aside main ad"
      ". footer .";  /* 空格通常会写一个不存在的名称，如【.】点 */
    grid-gap: 10px;  /* 设置空隙 */
  }
  .container > * {
    border: 2px solid red
  }
  .container > header {grid-area: header;}   /* 版块命名 */
  .container > aside {grid-area: aside;}
  .container > aside {grid-area: aside;}
  .container > main {grid-area: main;}
  .container > .ad {grid-area: ad;}
  .container > footer {grid-area: footer;}
</style>
```

<img src="https://i.loli.net/2020/07/26/I97wMn3iQWstkNm.png" alt="image-20200726161506848" style="zoom: 50%;" />

#### grid-gap 设置间距

>   grid-gap：通用间距
>
>   grid-row-gap :   行间距
>
>   grid-column-gap： 列间距

```css
.container{
  ...
  grid-gap: 10px;
}
```

<img src="https://i.loli.net/2020/08/02/5HCdtYMlTjknNRF.png" alt="image-20200726204956518" style="zoom:40%;" />

```css
.container{
  ...
  grid-row-gap: 20px;  /* 行间距 */
  grid-column-gap: 10px;   /* 列间距 */
}
```

<img src="https://i.loli.net/2020/08/02/XDZlTBKA7d2uUWq.png" alt="image-20200726205139866" style="zoom:40%;" />



​	

​	

## 实践

### 布局

>   Grid 尤其适合不规则布局

### 经验

>   +   等到 Grid 普及了，前端对CSS的要求会进一步降低
>   +   目前你简单尝试一下 Grid 就可以了

### 示例

#### 第一步：根据设计图，划分行列需求，给每块区域命名

<img src="https://i.loli.net/2020/07/26/n2FzpfyMVqdKmOH.png" alt="image-20200726211309111" style="zoom: 50%;" /><img src="https://i.loli.net/2020/07/26/PJKiFsmcovwNyZI.png" alt="image-20200726212409197" style="zoom:50%;" />



#### 第二步：书写【容器】样式

```html
<div class="container">
  <header>header</header>
  <div class="image bigImage">big</div>
  <div class="image smallImage">small</div>
  <div class="image smallImage">small</div>
  <div class="image smallImage">small</div>
  <div class="image middleImage">middle</div>
  <div class="image middleImage">middle</div>
  <div class="image middleImage">middle</div>
</div>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .container {
    display: grid;
    grid-template-rows: 50px 200px repeat(4, 100px); /* 重复4个120px，可以使用repeat()缩写语法 */
    grid-template-columns: 240px 240px;
    grid-template-areas:
      "header header"
      "big mid1"
      "big mid2"
      "sm1 mid2"
      "sm2 mid3"
      "sm3 mid3";
  }
</style>
```



#### 第三步：指定每项的名称（所属）

```css
.container > * {
  border: 2px solid black;
}
.container > header {grid-area: header;}
.container > .image:nth-child(2) {grid-area: big;}
.container > .image:nth-child(3) {grid-area: sm2;}
.container > .image:nth-child(4) {grid-area: sm3;}
.container > .image:nth-child(5) {grid-area: mid1;}
.container > .image:nth-child(6) {grid-area: mid2;}
.container > .image:nth-child(7) {grid-area: mid3;}
```

<img src="https://i.loli.net/2020/07/26/RYzNrquPyQ2F46X.png" alt="image-20200726221716959" style="zoom:50%;" />





​	

​	

### 游戏

https://cssgridgarden.com/#zh-cn

忘记用法时，可以通过游戏来快速回忆

​	

#### grid-area

>   grid-area属性接受4个由'/'分开的值：grid-row-start, grid-column-start, grid-row-end, 最后是 grid-column-end。
>
>   ​																					起始行 ↑↑            起始列 ↑↑           终止行 ↑↑                      终止列 ↑↑

![image-20200726223805829](https://i.loli.net/2020/07/26/7hbIdo6j42kNEZg.png)

```css
#garden {
  display: grid;
  grid-template-columns: 20% 20% 20% 20% 20%;
  grid-template-rows: 20% 20% 20% 20% 20%;
}

#water-1 {
  grid-area: 1 / 4 / 6 / 5;
}
#water-2 {
  grid-area: 2 / 3 / 5 / 6;
}
```

​	

#### order

>   如果网格项不是以`grid-area`、`grid-column`、`grid-row` 等显示的，它们会自动按照它们在源程序中出现的位置摆放。同样我们也可以使用`order`属性来重写它的顺序，这也是网格布局优于表格布局的好处之一。
>
>   默认情况下，所有的网格项的`order`都是0，但是顺序也可以被任意设置为正数或者负数，就像`z-index`一样。

​	

####  fr 与 px 结合

![image-20200726230822478](https://i.loli.net/2020/07/26/Db7YzmR6fUieIxF.png)

```css
#garden {
  display: grid;
  grid-template-columns: 50px 1fr 1fr 1fr 50px;
  grid-template-rows: 20% 20% 20% 20% 20%;
}

#water {
  grid-area: 1 / 1 / 6 / 2;
}

#poison {
  grid-area: 1 / 5 / 6 / 6;
}
```

![image-20200726231600839](https://i.loli.net/2020/07/26/pH6gQE2tsYkz1uw.png)

```css
#garden {
  display: grid;
  grid-template-columns: 20% 20% 20% 20% 20%;
  grid-template-rows: 1fr 1fr 1fr 1fr calc(100% - 50px);
}

#water {
  grid-column: 1 / 6;
  grid-row: 5 / 6;
}
```




