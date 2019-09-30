# HTML 入门笔记


HTML (HyperText Markup Language) 不是一门编程语言，而是一种用来告知浏览器如何组织页面的**标记语言**。😎💋<!--more-->

HTML 可复杂、可简单，一切取决于开发者。

它由一系列的**元素**（[elements](https://developer.mozilla.org/zh-CN/docs/Glossary/元素)）组成，这些元素可以用来包围不同部分的内容，使其以某种方式呈现或者工作。 一对标签（ [tags](https://developer.mozilla.org/zh-CN/docs/Glossary/Tag)）可以为一段文字或者一张图片添加超链接，将文字设置为斜体，改变字号，等等。

​	



## HTML 是谁发明的

>   HTML 之父

### 1990年左右诞生

+   Tim Berners-Lee，称之为**李爵士**
+   2004年，英女皇他颁发大英帝国爵级司令勋章
+   2017年，被颁发图灵奖

### 李爵士做了啥？

+   自己写了第一个浏览器
+   自己写了第一个服务器
+   用自己写的浏览器访问了自己写的服务器
+   发明了WWW，同时发明了HTML、HTTP和URL



​	

## HTML 起手式怎么写

>   快捷键：感叹号 ! + tab
>

```html
<!DOCTYPE html>   ← 文档类型              
<html lang="en">	← 页面根标签

<head>
  <meta charset="UTF-8">    ← 文件的字符编码
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> ← 防止页面缩放
  <meta http-equiv"X-UA-Compatible" content="ie=edge"> ← 如果在IE打开，告诉IE使用最新内核（IE11）
  <title>Document</title>
</head>

<body>

</body>
</html>
```

+   DOCTYPE：文档类型
    +   浏览器支持很多种文档类型（HTML、XHTML…）。
    +   `<!DOCTYPE html>` 表示告诉浏览器开始写HTML了。
+   html 标签是根标签
    +   必须要写（如果没写也会自动加上）
    +   可以把 lang 的属性值改为 zh-CN
+   head 和 body 标签，虽然是 html 的子元素，但一般格式上不缩进
+   head 标签里面存放看不见的元素
+   viewport 视口、视窗



​	

## 章节标签

>   章节标签，通常用于表示文章、书的层级（就是内容框架）

+   标题 h1~h6

+   章节 section

+   文章 article

+   段落 p

+   头部 header

+   脚部 footer

+   主要内容 main

+   旁支内容 aside

+   划分 div

```html
<body>
  <header>顶部广告</header>
  <div>
    <main>
      <h1>文章标题</h1>
      <section>
        <h2>第一章</h2>
        <p>
          一段话一段话一段话一段话一段话一段话一段话一段话一段话一段话一段话
        </p>
        <section>
          <h3>1.1 节</h3>
          <p>一段话</p>
        </section>
        <section>
          <h3>1.2 节</h3>
          <p>一段话</p>
        </section>
      </section>
    </main>
    <aside>
      参考资料 1 2 3
    </aside>
  </div>
  <footer>&copy; xxx版权所有</footer>
</body>
```



​	

​	

## 全局属性

>   任何标签都可以有的属性

​	

### class ： 类名

```html
<style>   ← style标签也有contenteditable 属性，可以被编辑
  [class=middle]{  /* 早期写法--缺点：当一个标签存在多个类名，就无法通过匹配单个类名来找到这个标签 */
    background: black;
    color: white;
  }
  .middle{  /* 简写 */
    background: black;
    color: white;
  }
</style>
```

​	

### contenteditable ：让标签可以被编辑

+   style标签，也有 contenteditable 属性，可以被编辑
    +   可以将 style 标签放到 body里面，然后添加 **`style { display : block; }`** ，style 标签中的样式内容就会显示在浏览器中，然后给style标签添加contenteditable 属性，就可以在浏览器总直接修改样式，并实时刷新

```html
<body>
  <style contenteditable>
    style{
      display: block;
    }
    .xxx{
      border: 10px solid orange;
    }
  </style>
  <p class="xxx" contenteditable>这段话可以直接在浏览器中编辑修改内容</p>
</body>
```

{{<image src="https://i.loli.net/2020/07/17/4PDCUkJ8R15VBjz.gif" caption="contenteditable">}}



​	

### hidden ：让标签隐藏

```html
<h1 class="title" hidden></h1>
.title{
	display: block   /* 直接添加属性hidden而隐藏的元素，可以通过设置样式 display: block，再显示回来 */
}
```

​	

### id ：全局唯一

+   id 用来表示【全局唯一的标签】

+   id 的全局唯一性没有保障，就算有两个重复的 id，HTML 也不会提示我写错了
    +   class足够了，不到万不得已不要用 id ，因为 id 重复使用也不会报错，可能误导开发者

    ```html
    <header id="xxx"></header>
    <div id="xxx"></div>
    #xxx{
    	/* 此时设置的样式会同时对两个id=xxx元素生效，id被重复使用不会报错，那id的唯一性就毫无意义了 */
    }
    ```

+   id 中有很多不能使用的词：parent、self、top …   控制台输入 **`window.`**  弹出属性列表中所有的词都不能用，因为这些词是 window 已经有的全局属性

    ```html
    <header id="top"></header>
    <script>
      top.style.border="10px solid red"  // 直接调用不能获取到top元素，与window对象中的全局属性重名
      var ele = document.getElementById("top") // 通过这种方式可以获取到 top 元素，但单词略复杂
    </script>
    ```



​	

### style ：行内样式

```html
<div style="border: 10px solid red" id="a"></div>
```

+   js 中写 `a.style.border = "100px solid green"`  会覆盖 div元素中的行内样式



​	

### tabindex ：顺序

+   在浏览页面时，网页中按钮都只通过键盘 tab 控制切换选中。而这里的 tabindex 属性就是控制，切换的顺序

    +   被选中的会有一圈不明显的蓝色边框

    ```html
    <a tabindex=1>首页</a>
    <p tabindex=3>一段话</p>
    <footer tabindex=2>&copy;版权所有</footer>
    ```

+   tabindex 可以是正数，不必是连续的

+   tabindex 可以是 0，表示最后才被 tab 访问

+   tabindex 可以是 -1，表示不可被 tab 访问



​	

### title ：鼠标指向时显示的内容

+   单行文字溢出，使用省略号

    +   将鼠标移动到这段文字上时，应该能展示出全部文字内容，包括溢出隐藏的部分

    ```html
    <p class="xxx" title="显示一段超长的话">一段超长的话</p>
    <style>
      .xxx {
        text-overflow: ellipsis;   /* 超出部分用省略号 */
        overflow: hidden;     /* 超出隐藏 */
        white-space: nowrap;   /* 不换行 */
      }
    </style>
    ```
    



​	

​	

## 内容标签

### ol+li

>   ordered list + list item

+   ol ：有序列表
+   ol 不能含有 li 之外的任何元素、字符

    {{<figure src="https://i.loli.net/2020/07/17/sbHl2ftTAOdVcZn.png" >}}

### ul+li

>   unordered list + list item

+   ul ：无序列表

    {{<figure src="https://i.loli.net/2020/07/17/7PwEHXYce3y5vFL.png" >}}

### dl+dt+dd

>   description list + term + data

+   dl ：description list —— 描述列表

+   dt ：description term —— 描述项

+   dd ：猜测是 description data —— 描述信息

    {{<figure src="https://i.loli.net/2020/07/17/ZngFrGO1fwaBW8M.png" >}}	

###  pre

>   preview 的缩写

+   用的较少

+   html 特点：HTML 代码里的多处空格、回车、tab 等内容，默认会被转化为一个空格

+   pre 的作用是，可以保留开发者键入的空白位置（pre 有默认样式，很丑，如下）

    {{<image src="https://i.loli.net/2020/07/17/wymCdUX19EaAojn.png" caption="pre">}}

### code 

>   等宽字体

+   用 code 标签包裹的内容，字符是等宽的。但默认内容在同行展示，无法换行

+   使用 pre 可以让 code 的内容换行

    {{<image src="https://i.loli.net/2020/07/17/NwKaXSdYMHJgqs8.png" caption="code & pre+code">}}

### hr 

>   水平分割线

+   horizontal

    {{<figure src="https://i.loli.net/2020/07/17/u6BMqOD92pSowFI.png">}}

### br

>   break 的缩写

+   换行



​	

### a

>   anchor 的缩写

+   超链接
    +   href 
    +   target
        +   国内开发习惯使用 _blank 在新标签打开链接。
        +   国外开发通常不写 target，他们觉得会让浏览器网页越开越多（他们习惯：通过鼠标中间点击打开新标签页，默认左键点击同屏跳转）

### em

>   emphasis 的缩写

+   emphasis 意为强调，效果字体斜体



​	

### strong

+   字体加粗
+   表示重要



​	

###  q

>   quote 的缩写

+   quote 意为引用
+   quote标签：内联，行内，不换行



​	

### blockquote

+   引用
+   块级，换行

    {{<image src="https://i.loli.net/2020/07/17/wYSfjvtP7DTgmzC.png" caption="quote & blockquote">}}
