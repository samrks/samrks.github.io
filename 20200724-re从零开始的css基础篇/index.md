# Re：从零开始の CSS 学习笔记——基础篇


熬夜总结 – CSS 基础篇 。🤞🏻 Nobody knows CSS than me !! 🤗  <!--more-->

![Family Guy Css GIF - FamilyGuy Css OpenWindow - Discover & Share GIFs](https://i.loli.net/2020/07/22/Nue4qvXzsQpbcAF.gif)

 		

  	

## CSS 的历史

### CSS 是谁发明的

>   李爵士的挪威同事赖先生，首先[提出 CSS](https://www.w3.org/People/howcome/p/cascade.html)
>
>   Håkon Wium Lie （挪威语）

### 赖先生的生平

#### 生平

+   1991年获得MIT媒体实验室视觉研究理学硕士学位

+   1994年提出CSS概念而闻名

+   1999年任Opera的CTO（首席技术官，不分前端后端、只要是技术就负责）

+   2005年他写公开信给比尔盖茨问为什么IE不支持Web标准，盖茨说IE 7马上发布，他写了 Acid2 用来验证

+   2006年通过了博士论文答辩

+   2006年他倡议 Web 网页应使用通用字体格式

+   2007年他倡议浏览器可以支持video标签

#### 观点

+   微软的IE常常成为他的批评对象

+   他也是Web打印概念的倡导者，用HTML和CSS写书



​			

​			

### CSS 的牛 X 之处在哪？

>   CSS（Cascading Style Sheets）：==层叠==样式表

#### 样式层叠

可以多次对同一选择器进行样式声明

#### 选择器层叠

可以用不同选择器对同一个元素进行样式声明

#### 文件层叠

可以用多个文件进行层叠

#### 这些特性使得 CSS 极度灵活

这也为 CSS 后来被吐槽留下了隐患



​				

​				

###  CSS 的版本

>   ⭐目前使用最广泛的css版本：css 2.1 版本
>
>   +   2004-2011年间不断更新，没有具体发布时间

| **版本**      | **时间**       | **重点**                           |
| ------------- | -------------- | ---------------------------------- |
| CSS  1        | 1996年         | 不用管                             |
| CSS  2        | 1998年         | 添加定位、z-index、media，不用管   |
| **CSS  2.1 ** | 2004~2011年    | 使用最广泛的版本（IE支持）         |
| CSS  3        | 1999年开始起草 | 现代版本，分模块（IE  8 部分支持） |
| CSS  4*       | 分模块升级     |                                    |

​				

​				

### 浏览器对CSS的支持（兼容性）

>   怎么知道，哪些浏览器，支持哪些特性？
>
>   +   方法一：几十种浏览器全部跑一遍
>   +   方法二：使用 [caniuse.com](https://www.caniuse.com/#search=flex)

![image-20200720145452024](https://i.loli.net/2020/07/20/1E2eSlgzD5cIrC3.png)

+   红色：不支持flex
+   黄色：部分支持
+   绿色：完全支持

#### caniuse.com 使用方法

##### 请收藏此网站

+   输入你关心的样式，比如 border-radius 或 filter

+   查看大部分浏览器的支持情况

+   如果想看更多，点击 Show All

+   下方会详细说明兼容 bug 有哪些（翻译成中文）

##### 这个网站是怎么运作的

+   网站主一开始[自己测试](http://tests.caniuse.com/)了一部分浏览器

+   社区的前端工程师帮助测试各种各样的浏览器

+   UC 浏览器和 QQ 浏览器当然主要是中国开发者测试

+   大家把测试结果提交到 GitHub 上

+   这就是<u>开发者社区</u>的力量、

    +   什么是社区？

        社区不是一个准确存在的概念，大家你帮我我帮你，互相学习，互相分享，就形成了社区。

        是一种弱联系。

        社区通过汇集的力量将事情给解决好



​				

​			

## CSS 是艺术

>   就像画画、折纸
>
>   你需要用感性思维来理解 CSS

###  不要用理性思维

+   即不要问「为什么会这样」，而是说「原来是这样」
+   浏览器说是怎么样，就是怎么样

+   当然有极少情况是浏览器出错了

### 为什么 color: red 能让字变红

+   不要问「为什么」

+   要说「原来是这样」

+   所见即所学



​					

​				

## 体系化学习

>   +   有生之年都不太可能把css所有知识学完，因为它太、多、了。只文档可能就有几百页，目录全看一遍都很累：[css标准文档](https://www.w3.org/Style/CSS/specs.en.html)（google 搜 css spec）
>   +   不是把所有知识都学完，而是在一个新知识点到来的时候，马上学会它。这才是学习CSS体系化最重要的手段

>   体系化学习 CSS 和 HTML 的过程完全一致

### 学一门语言必须学会什么

+   [语法](#语法)（怎么写代码）

+   css 语法非常简单

+   [如何调试](#如何调试 CSS)（怎么知道自己代码写错了）

    +   ```js
        node-w3c-validator -i index.html    // 查错
        ```

+   [在哪查资料](#在哪查资料)（其实就是为了抄代码）

+   MDN

+   标准制定者是谁

    +   尝试所有方法仍不能解决问题的话，就必须看[标准文档](https://www.w3.org/Style/CSS/specs.en.html)了

### 如何学

+   Copy - 抄文档、抄老师

+   Run - 放在自己的机器上运行成功

+   Modify - 加入一点自己的想法，然后重新运行



​				

​			

### 语法

>   超级简单

#### 语法一：样式语法

```css
选择器 {
  属性名: 属性值;
  /*注释*/
}
```

##### 注意事项

>   不学常态，学变态

+   所有符号都是英文符号，如果写错了，浏览器会警告（样式会被~~划掉~~）

+   区分大小写，a 和 A 是不同的东西（特别是大小写很相似的字母，很容易错）

+   没有 // 注释，只有 `/* 注释 */`  这种形式

+   最后一个分号可以省略，但建议不要省略

+   任何地方写错了，都不会报错，浏览器会直接忽略

+   那我怎么知道自己写没写错呢？一会说



#### 语法二：[at ](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule)[语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule)

```css
@charset "UTF-8";    /* 声明字符编码，必须放在第一行 */
@import url(2.css);   /* 导入一个额外的css文件 */
@media (min-width: 100px) and (max-width: 200px) {    /* 媒体查询 */
  语法一
}
```

##### 注意事项

+   @charset 必须放在第一行

+   前两个 at 语法必须以分号 ; 结尾

    ```css
    @charset "UTF-8";
    @import url(2.css);
    ```

+   @media 语法会单独教学

+   charset 是字符集的意思，但 UTF-8 是字符编码 encoding（并不是字符集），这是历史遗留问题

    +   最早只有 ASCII 这一种字符集（英文），这个字符集的编码形式就是它自己
    +   后来中国人发现 ASCII 不能表示中文，所以就发明了 GB2312，由中国国家标准局（国标局）发行，GB 就是国家标准的缩写。
        +   GB2312 是一个支持中文简体汉字的字符集。它对应的编码形式，是它自己
    +   后来日本、韩国的文字，我们的 GB2312 也表示不了。国标局还没开始动作，微软先一步发明 GBK，k表示扩展。GBK 可以同时表示  中文、日文、韩文（CJK）
    +   后来中国发现还需要和更多国家建立联系，国标库根本就搞不定，就有一个叫 Unicode 的组织，给全球制作了一个字符集，叫做 Unicode（万国码）。这个码包含非常多国家，特别长，所以必须采用不同的编码形式。
        +   编码形式包含：UTF8/UTF16/UTF32 （选择任意一个即可）
        +   从 Unicode开始字符集和编码形式就不统一了，之前都是统一的
    +   html 1993年左右发明，css 发明于1996年左右，Unicode在1996年之后发明。
    +   charset 原本后面写一个【字符集的名字】，写 GBK/GB2312 ….这种，对应编码形式也是同名的。但是Unicode发明后，字符集名称和编码形式不统一了，所以我们为了表明自己使用的是 Unicode 的那种编码形式，就只能将Unicode的编码形式，写在表示字符集的 charset 的位置。
    +   ==【这就是这个历史问题的发展经过】==

| 字符集  | 编码形式         |
| ------- | ---------------- |
| ASCII   | ASCII            |
| GB2312  | GB2312           |
| GBK     | GBK              |
| Unicode | UTF8/UTF16/UTF32 |

+   问： @charset “UTF-8”  指的是什么？

    >   +   回答 ：字符集（charset） 是错误的
    >
    >   +   **“UTF-8”**指定的不是字符集 charset，而是**指定的【 文件编码 】**
    >       +   虽然charset本身是字符集的意思，但后面的 utf-8 指的是  Unicode字符集的**【编码形式】**的一种。这是一个历史遗留问题



​				

​			

### 如何调试 CSS

#### 方法

+   使用 W3C 验证器（[在线](https://jigsaw.w3.org/css-validator/) / 命令行工具）不用试了

    ```js
    node-w3c-validator -i index.html    // 可能需要先安装java环境，才能运行 node-w3c-validator
    ```

    +   [Java环境安装、配置](https://www.cnblogs.com/SNNA/p/12531886.html)

+   使用 VSCode 看颜色

+   使用 WebStorm 看颜色

+   使用开发者工具看警告

    +   浏览器预览页面，右键检查，查看Element中，一般会给出问题警告。有问题的css会被~~划删除线~~

#### 如何使用开发者工具

+   找到你脑中的标签

+   看它是否有选择器

+   看它的样式是否被划掉

+   看它的样式是否有警告

#### Border 调试法

##### 步骤

-   怀疑某个元素有问题
-   就给这个元素加 border
-   border 展示效果没出现？说明选择器错了或者语法错了
    -   通常是 border 上面的代码有问题（因为上面代码执行遇到问题，就会影响下面样式的输出）
-   border 出现了？看看边界是否符合预期
    -   说明选择器没有错、border以上的代码没有错
    -   可以继续往下测试、查找错误
-   bug 解决了才可以把 border 删掉

##### 记住

-   CSS 的 border 调试法
-   就相当于 JS 的 log 调试法
-   我会再每节课重复这个调试法



​	

#### 新人常见错误

##### 低级错误

-   选择器拼写错误
-   属性名拼写错误
-   属性值拼写错误
-   大小写错误
-   没写分号
-   中文冒号
-   没写反花括号
-   没加单位

##### 非低级错误

-   没有非低级错误





​			

​			

### 在哪查资料

#### 网站推荐

-   Google 搜索关键词时加 MDN
-   CSS tricks（英文），专门收集 css 小技巧的网站
    -   使用方式：在google 搜  `[技术名词] css tricks`    https://css-tricks.com/snippets/css/a-guide-to-flexbox/
-   张鑫旭的博客：可能是中国花时间最多来学css的人
    -   使用方式：搜  [技术名词] 张鑫旭

#### 书籍推荐

-   不推荐买任何书
-   CSS 和 HTML 一样，以练为主



​				

​				

### 在哪搜练习素材

#### PSD

-   Freepik 搜索 [PSD web](https://www.freepik.com/search?query=web&type=psd)  英文（下载免费的 psd）
    -   如果下载慢，就把域名加入翻墙插件
-   中文免费 PSD 网站较少，需要多搜一下
-   365PSD 里的 [UI ](https://cn.365psd.com/free-psd/ui-kits)[套件](https://cn.365psd.com/free-psd/ui-kits)还行

#### 效果图（不提供下载）

-   dribbble.com 顶级设计师社区
-   可以用肉眼模仿它

#### 商业网站

-   直接模仿你常去的网站

​			

### 不要沉迷临摹

>   每个类型的临摹一两个即可
>
>   +   PC 网站、手机网站、UI 套件
>
>   +   再多无益

​					

### 遇到查不到的问题，怎么办

>   遇到查不到的问题，应该去哪里查文档？  ——找标准制定者

>   李爵士、赖先生

-   W3C
-   搜索 CSS spec 可以找到 [CSS 最新标准](https://www.w3.org/Style/CSS/specs.en.html)
    -   没人能看完它
    -   你可以看看 CSS 2.1 标准的[中文版](http://www.ayqy.net/doc/css2-1/cover.html)
    -   正确学 css ，应该是遇到问题，再查资料



​	

​	

## 基本概念

### 要理解几个重要的概念

-   [文档流](#文档流) Normal Flow
-   [块、内联、内联块](#块、内联、内联块)
-   [margin 合并](#margin合并)
-   [两种盒模型](#盒模型)（border-box 更符合人类思维）



​			

### 文档流

>   文档流的英文名称叫做  Normal Flow
>
>   文档流方向：默认，内联元素从左到右，块级元素从上到下

![文档流动方向](https://i.loli.net/2020/07/21/DM2CyaNgR7bGQUs.png)

![emmet](https://i.loli.net/2020/07/21/X13ZmegalSqOA6y.png)

emmet:  `span{第$个span元素}*6` + tab

​	

### 块、内联、内联块

>   +   将元素分为：内联元素、块级元素。是一种已经过时的分法。
>       +   在新的 HTML5 标准中，元素不分内联、块级。所有元素都可以是内联元素，所有元素都可以是块级元素。
>   +   看样式：如果 **`display:inline`**， 就是内联元素。如果 **`display: block`**  就是块级元素。
>   +   内联块：不会出现如【内联元素首尾可以在不同行】的情况，内联块会同行展示，但是又保有 block 的一个特点：不会跨两行（这就是inline和inline-block在流动方向上相似又稍微不同的点）

#### 流动方向

-   inline 元素从左到右，到达最右边才会换行
-   block 元素从上到下，每一个都另起一行
-   inline-block 也是从左到右

#### 宽度

##### **inline 元素**

1.  宽度为 内部 inline 元素的和。
2.  不接受用 width 指定宽度。加了也不变化
3.  **不要**在 inline元素内部，再写 block 元素。它的计算可能乱套的，出现后果自负

##### **block 元素**

1.   默认自动计算宽度，可用 width 指定

     -   `width: 200px; `（200像素）、`width: 20em;` （20个字宽）

2.   不指定宽度，宽度默认是 **`width: auto`**。不是 width: 100%，**是能有多宽就占多宽**，最大可以占 100%

     -   如下图，每个div元素都有边框，【div默认宽度】=100%-边框宽度。如果【设置div宽度为100%】，那么相当于，整行宽度= div宽度100%+边框宽度，所以会多出边框宽度的一小块位置
     -   ==经验之谈==：给任何元素都尽量，**不要设置 width: 100% 的样式**。大部分情况写了就等于 bug。

     ![image-20200721223815884](https://i.loli.net/2020/07/21/s3miyMWE9YgqzGd.png)

     

##### **inline-block 元素**

1.  结合前两者特点，可用 width

    +   默认情况，与 inline 一致

    +   但是又可以用 width 指定宽度，这一点又与 block 一致



#### 高度

##### inline 高度

1.   由 **line-height（行高）** **间接**确定，跟 height 无关 （设置height无效）

     +   如下图，为什么外层的 绿div 没有包住 红span、被撑高呢？

         +   因为 span 的高度，不是【内容高+padding】决定的。**padding 改变的不是 span 的实际高度**，只是改变了可视的高度。div 框的高度才是 span 的**实际高度**。

         ![image-20200721232421441](https://i.loli.net/2020/07/21/7WNZPex21wKfihL.png)

         ---

         +   span 的实际高度，由 **行高** 间接确定。如下图，虽然span的红框仍然没变，但是 div高度变化了。而div包裹的高度才是 span 的**实际高度**（只需关注外层div高度即可，红框的高度并不是span实际高度）

         ![image-20200721233215790](https://i.loli.net/2020/07/21/IzaWv3XVfF8kEeh.png)

     +   “间接” 怎么理解？

         >   即使设置行高100px，span的高度也有可能不是100px高

         +   设置不同的**字体**，会改变 span 的实际高度。经测试：↓↓

             +   默认，微软雅黑字体、行高是100px的话，实际高度就是100px。
             +   改成宋体、行高是100px，span 的实际高度变成 101px。
             +   改成 monospace(等宽字体)、行高是100px。 实际高度变成 102px

         +   具体知识点叫做：**行盒** 

             （可以看[文章](https://zhuanlan.zhihu.com/p/25808995)，能看懂就看，面试官可能都看不懂，前期可以不用研究这么深）

##### block 高度

+   默认高度，由内部所有 **文档流元素** 的**高度总和** 决定的。（由默认行高间接决定）
    <img src="https://i.loli.net/2020/07/22/vNcFKe6A4CdalfE.png" alt="image-20200722140905258" style="zoom:67%;" />

    +   内层div是脱离文档流的元素，外层div就无法计算上它的高度

        +   后面会介绍：如何**脱离文档流**、脱离后去哪里了

            <img src="https://i.loli.net/2020/07/22/n3IB9TphDwejt27.png" alt="image-20200722141229968" style="zoom: 67%;" />

+   同时也可以自定义设置 height 高度。这种情况，就忽略内部元素高度。

+   如果div中没有任何内容，高度为0。区别于span，span中没有内容，也有高度，由 默认的 line-height 决定。

    <img src="https://i.loli.net/2020/07/22/cDILgRMKz9avYEV.png" alt="image-20200722143215855" style="zoom:67%;" />

    

     

##### inline-block 高度

+   默认高度的计算，跟 block 一样。
+   也可以自定义设置 height。



### overflow 溢出

>   当内容大于容器

-   当内容的宽度或高度大于容器的，会溢出
    -   可用 overflow 来设置是否显示滚动条
    -   auto 是灵活设置
    -   scroll 是永远显示（基本不用）
    -   hidden 是直接隐藏溢出部分
    -   visible 是直接显示溢出部分（默认值）
    -   overflow 可以分为 overflow-x 和 overflow-y，通常只用overflow。x和y可能最终效果会与实际需求有差

+   示例

    ```html
    <div style="width: 10em; height: 200px;">内容......</div>
    ```

    ```css
    overflow: visible;   /* 默认 */
    overflow: hidden;   /* 超出部分隐藏 */
    overflow: scroll;   /* 超出部分可滚动预览，很少有人用。因为内容没溢出时，仍显示滚动条，非常丑 */
    overflow: auto; 		/* 超出时显示滚动条，不超出不显示 */
    ```

    <img src="https://i.loli.net/2020/07/22/QE6qVilvTftODRu.png" alt="image-20200722142201945" style="zoom:50%;" />

+   横向滚动条

    ```html
    <div style="width: 10em; height: 200px;">
      内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容
      <div style="width:1000px;">
        内容
      </div>
    </div>
    
    <style>
    div{
      border: 1px solid green;
      overflow: auto;
    }
    span{
      border: 1px solid red;
    }
    </style>
    ```

    +   内部文档流元素宽度超出外层父元素，就会出现横向滚动条

    +   同时原本的内容，不会因为宽度被内部元素撑开而平铺显示，仍会只显示在第一屏中

        ![overflow](https://i.loli.net/2020/07/22/r6Ay1XQhxjT3ftL.gif)



​	

### 脱离文档流

#### 回忆一下

+   block 高度由内部文档流元素决定，可以设 height

+   这句话的意思是不是说，有些元素可以不在文档流中

#### 哪些元素脱离文档流

>   脱离文档流的元素，那么它所在的容器，就不会把它计算进高度中
>
>   +   **脱离文档流**，就是从普通文档流中跳出，比普通文档流的层级稍高

+   float

+   position: absolute / fixed

####  怎么让元素不脱离文档流

+   不要用上面属性不就不脱离了



​	

### 盒模型

>   content box & border box
>
>   css 盒模型分为两种：一种是 content-box ，一种是 border-box 
>
>   二者的区别是：
>
>   +   content-box 的宽度，只包含 content 部分
>   +   border-box 的宽度，包含 border、padding 和 content

![image-20200722150554444](https://i.loli.net/2020/07/22/39hdpUSl4Zy1eDa.png)

#### 分别是

-   content-box 内容盒 - 内容，就是盒子的边界
-   border-box 边框盒 - 边框，才是盒子的边界

#### 公式

-   content-box width = 内容宽度
-   border-box width = 内容宽度 + padding + border

#### 哪个好用

-   border-box 好用
-   同时指定 padding、width、border 就知道为什么了



​					

### margin合并

#### 哪些情况会合并

-   兄弟 margin 合并

    -   上方元素的 margin-bottom，会和下方元素的 margin-top 重合

-   父子 margin 合并

    -   给子元素添加 margin-top，效果会作用在父元素上（bf：嵌套崩塌）
    -   第一个子元素的 margin-top，会和 最后一个子元素的 margin-bottom，效果会作用在父元素的 margin-top/bottom 或 和父元素的 margin-top/bottom 重合

    >   记住：只有上下会重叠，左右从来不重叠

#### 如何阻止合并

>   不要问为什么，CSS 很多效果是试出来了，无法解释清楚

-   父子合并用 padding / border 挡住
    -   可以用很小的 padding/border 来挡住，让父子的margin无法穿透并重叠
-   父子合并用 overflow: hidden 挡住
-   父子合并用 display: flex，不知道为什么，试出来的
-   兄弟合并是符合预期的
-   兄弟合并可以用 `display: inline-block` 消除

总之要一条一条死记。

而且 CSS 的属性逐年增多，每年都可能有新的

>   为什么css 难学？
>
>   +   不正交
>       +   因为很多没有道理，需要死记硬背的点。
>       +   为什么 `display: inline-block` 可以消除 margin 合并。
>       +   并没有 `enable-margin-callapse: false`  这种选项，翻译一下就知道它是用来单独控制margin合并的。
>       +   无法知道 现在写的这句 css，到底会影响哪些属性 
>   +   什么叫正交？
>       +   当我们调节显示器的亮度时，显示器的对比度不会改变。
>       +   而 css 是不正交的，因为我们在写某一个属性时，可能就会改变其他属性的状态、效果

![Family Guy Css GIF - FamilyGuy Css OpenWindow - Discover & Share GIFs](https://i.loli.net/2020/07/22/Nue4qvXzsQpbcAF.gif)

​				

​				

### 基本单位

#### 长度单位

-   px 像素
-   em 相对于自身 font-size 的倍数
-   百分数
-   整数
-   rem：等你把 em 滚瓜烂熟了再问 rem
-   vw 和 vh
-   其他长度单位都用得很少，不用了解

#### 颜色

-   十六进制 `#FF6600` 或者 `#F60` `#000`
    -   也支持最后添加 alpha。 `#FF660000` 全透明、 `#FF6600FF` 不透、 `#FF660088` 半透。但**兼容性不确定**
-   RGBA 颜色 `rgb(0,0,0)` 或者` rgba(0,0,0,1)`
-   hsl 颜色 `hsl(360,100%,100%)`：色相、饱和度、明度
    -    `hsla(360,100%,100%, 0.5)`



​						

​			

##  实践：做一个彩虹

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
<div class="rainbow">
  <div>
    <div>
      <div>
        <div>
          <div>
            <div>
              <div>
                <div>
                  
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</body>
</html>
```

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body{
  background: white;
}
.rainbow {
  height: 200px;
  overflow: hidden;
}
.rainbow div {
  overflow: hidden;
}
.rainbow > div {
  width: 400px;
  height: 400px;
  background: red;
  border-radius: 50%;
}
.rainbow > div> div {
  background: hsl(60, 80%, 50%);
  height: 380px;
  margin: 10px;
  border-radius: 50%;
}
.rainbow > div> div > div {
  background: hsl(120, 80%, 50%);
  height: 360px;
  margin: 10px;
  border-radius: 50%;
}
.rainbow > div> div > div > div {
  background: hsl(180, 80%, 50%);
  height: 340px;
  margin: 10px;
  border-radius: 50%;
}
.rainbow > div> div > div > div >div {
  background: hsl(240, 80%, 50%);
  height: 320px;
  margin: 10px;
  border-radius: 50%;
}
.rainbow > div> div > div > div >div > div {
  background: hsl(300, 80%, 50%);
  height: 300px;
  margin: 10px;
  border-radius: 50%;
}
.rainbow > div> div > div > div >div > div > div {
  background: hsl(330, 80%, 50%);
  height: 280px;
  margin: 10px;
  border-radius: 50%;
}
.rainbow > div> div > div > div >div > div > div > div {
  background: hsl(330, 80%, 100%);
  height: 260px;
  margin: 10px;
  border-radius: 50%;
}
```

<img src="https://i.loli.net/2020/07/22/28UTue5DspJ6gGt.png" alt="image-20200722160523027" style="zoom:50%;" />




