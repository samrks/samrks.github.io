# HTML 标签の简单记录🌈🏳️‍🌈


❤️ 🧡 💛 💚 💙 💜 记录几个标签的简单用法，用于 emmm .. 忘掉  🤪 <!--more-->

​	

## a 标签

>   特别常用，但很多人不会用 :flushed::flushed:

+   属性 
    +   [href](#href)
    +   [target](#target)
    +   [download](#download)
    +   [rel=noopener](#rel=noopener)   面试可能会问
+   作用
    +   跳转外部页面
    +   跳转内部锚点
    +   跳转到邮箱或电话等

### href

>   href 是 **H**ypertext **REF**erence 超链接
>
>   [发音](https://zh.forvo.com/word/href/) ：应该读 H-Ref `/ˈeit∫.rɛf/`

#### href 的取值

##### 网址

+   https://google.com
+   http://google.com
+   [//google.com](//google.com)   【推荐写这种形式】 
    +   network 会先请求 http://google.com → http://www.google.com → https://www.google.com 结束
##### 路径

>   使用 hs 开启服务的目录，就是 /a/b/c  的根目录
>
>   双击打开文件，访问的路径是硬盘的根目录 file
>

+   /a/b/c  以及 a/b/c

+   index.html 以及 ./index.html

    ```html
    <a href="/a/b/c.html">c.html</a>
    <a href="./index.html">index.html</a>
    ```

##### 伪协议

+   javascript:代码;    

    ```html
    <a href="javascript:alert(1);"></a>   <!--执行js操作：弹出1-->
    <a href="javascript:;"></a>   <!--只有这种是什么都不做-->
    <a href="">href为空，页面会刷新</a>   <!--如果有input会被清空-->
    <a href="#">#不刷新，但页面会滚动到顶部</a>
    ```

    补充：编译器中 `p{$}*30` + tab ，会生成 30 个 p 标签，内容 1-30

+   mailto:邮箱

    +   移动端，通常会直接呼出发邮件的界面，并自动填写收件人邮箱

    ```html
    <a href="mailto:ryuukousen@gmail.com">给他发邮件</a>
    ```

+   tel:手机号

    +   移动端，直接呼出拨号盘，并自动填写号码
    
    ```html
    <a href="tel:13912345678"></a>
    ```

##### id  锚点

>   跳转到指定标签      href=#xxx

```html
<a href="#xxx">查看aaa</a>
...
<p id='xxx'>aaa</p>
...
```

​	



### target

>   在哪个页面打开窗口

#### 内置名字

+   _blank ：新窗口
+   _top ：顶级窗口（结合 iframe）
+   _parent  ：父级窗口（结合 iframe）
+   _self ：当前窗口（默认值）

####  程序员命名

##### window 的 name

```html
<a href="//google.com" target="asdf">google</a>
<a href="//baidu.com" target="asdf">baidu</a>
```

+   `target=asdf`，表示如果有这个`asdf`窗口就在这个窗口打开google，如果没有就创建一个新的窗口命名为`asdf`
+   控制台输出 window.name  可知当前窗口的名称

##### iframe 的 name

>   可以写一个搜索引擎的集合或切换，goodbai
>
>   谷歌已经禁用了在 iframe 中引入

```html
<a href="//google.com" target="asd">google</a>
<a href="//baidu.com" target="qwe">baidu</a>
<iframe src="" name=asd></iframe>
<iframe src="" name=qwe></iframe>
```

​	

### download

>   绝大多数浏览器不支持，chrome 不支持

```html
<a href="//google.com" download>下载页面</a>
```

#### 作用

不是打开页面，而是下载页面

#### 问题

不是所有浏览器都支持，尤其是手机浏览器可能不支持



​	

## iframe

>   内嵌窗口
>
>   已经很少使用了，还有些老系统再用。新的方式是通过 ajax 实现

```html
<iframe name="a"  src="a-target-iframe.html" frameborder="0"></iframe>
```



​	

## table 

### 相关标签

>   thead、tbody、tfoot，即使开发时换了顺序，浏览器识别时仍会按照 head  body  foot 的顺序解析渲染

+   table
+   thead
+   tbody
+   tfoot

+   tr
+   td
+   th



#### 一个表头的表格

{{<figure src="https://i.loli.net/2020/07/18/wZLvm3BSFY8IXt6.png">}}

```html
  <table>
    <thead>
      <!-- tr: table row 行-->
      <tr>
        <!-- th: table head 表头-->
        <th>英语</th>
        <th>翻译</th>
      </tr>
    </thead>
    <tbody>
      <tr>
         <!-- td: table data 数据 -->
        <td>hyper</td>
        <td>超级</td>
      </tr>
      <tr>
        <td>target</td>
        <td>目标</td>
      </tr>
      <tr>
        <td>reference</td>
        <td>引用、链接</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td>空</td>
        <td>空</td>
      </tr>
    </tfoot>
  </table>
```





#### 两个表头的表格

{{<figure src="https://i.loli.net/2020/07/18/dQVqa5kzSXlmMxf.png">}}

```html
<table>
  <thead>
    <tr>
      <th></th>
      <th>小红</th>
      <th>小明</th>
      <th>小颖</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>数学</th>
      <td>61</td>
      <td>85</td>
      <td>92</td>
    </tr>
    <tr>
      <th>语文</th>
      <td>61</td>
      <td>85</td>
      <td>92</td>
    </tr>
    <tr>
      <th>英语</th>
      <td>61</td>
      <td>85</td>
      <td>92</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th>总分</th>
      <td>200</td>
      <td>200</td>
      <td>200</td>
    </tr>
  </tfoot>
</table>
```



### 相关样式

#### table-layout

+   auto【默认】：根据单元格中的内容数量，计算单元格宽度  
+   fixed：单元格宽度尽量平均
+   inherit
+   initial
+   unset

#### border-spacing

>   设定每个单元格的间隙

```html
<style>
  table{
    width: 600px;
    table-layout: auto;
    border-spaceing: 10px;
  }
  td,th{
    border: 1px solid blue;
  }
</style>
```



#### border-collapse

>   通常用于 去掉单元格间隙

```html
<style>
  table{
    width: 600px;
    table-layout: auto;
    border-collapse: collapse;   /* 去掉单元格间隙 */
    border-spaceing: 0;   /* 去掉单元格间隙 */
  }
  td,th{
    border: 1px solid blue;
  }
</style>
```





>   下面这两句常被写进 reset.css 中

```css
border-collapse: collapse;   /* 去掉单元格间隙 */
border-spaceing: 0;   /* 去掉单元格间隙 */
```



​	

## img 标签

### 作用

发出 get 请求，展示一张图片

### 属性

+   alt：全称是可替换的，当图片加载失败，会显示 alt 的值{{<figure src="https://i.loli.net/2020/07/18/YP6MvrgmWnJpaTE.png">}}

+   height：只设置 height  ，宽度自适应

+   width：只设置 width  ，高度自适应

    +   同时设定，height 和 width，图片可能变形

        >   永远不要让图片变形

+   src：全称 source

### 事件

+   onload/onerror

```html
<img id="xxx" width="400" src="dog.jpg" alt="一只狗子">
<script>
	xxx.onload = function(){
    console.log("图片加载成功");
  }
  xxx.onerror=function(){
    console.log("图片加载失败");
    xxx.src = "/404.jpg";  // 挽救：图片加载失败，展示404图，放在项目中
  }
</script>
```



### 响应式

+   max-width:100%

```html
<style>
  *{
    margin:0;
    padding:0;
    box-sizing: border-box;
  }
  img{
    max-width: 100%;   /* 图片是响应式的，尺寸永远满足各种窗口大小 */
  }
</style>
```



### 可替换元素

https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element

考试可能会问，被问概率30%



​	

## form 标签

### 作用

发 get 或 post 请求，然后刷新页面 

### 属性

```html
<form action="/xxx" method="POST">
  <input type=text />
  <input type=submit />
</form>
```

#### action

>   +   默认需要给一个 action ，就是发送请求的目标地址（HTTP相关）、请求某一个页面的页面地址
>
>   +   需要有后台给我们提供这个地址

#### method

>   method 属性控制用 GET 还是 POST 来发送请求

#### autocomplete

>   自动填写
>
>   +   autocomplete: on / off

#### target

>   提交到哪个页面：新开页面、当前页面、iframe
>
>   +   与 a 标签写法一致
>       +   `_blank `
>       +   `top `：顶级窗口（结合 iframe）
>       +   `_parent ` ：父级窗口（结合 iframe）
>       +   `_self` ：当前窗口（默认值）
>       +   `window.name`
>       +   `<iframe name=xxx></iframe>`

```html
<form action="/xxx" method="POST" autocomplete="off" target="a">
  <input type=text />
  <input type=submit />
</form>
<iframe name="a"  src="a-target-iframe.html" frameborder="0"></iframe>
```

​	

### 事件

onsubmit：用户点击提交会触发这个事件



​	

### type=submit

>   form 中的 button 如果没有指定 type 类型，默认是 type=submit 的，点击 button 就会提交 form 表单。如果指定了其他类型，例如 type=button，按钮就不能提交表单。
>
>   +   一个 form 表单中，必须要有一个 type=submit 的 input 或 button ，才能提交表单。

​	

#### 修改submit按钮文字

```html
<input type="submit" />
```

```html
<input type="submit" value="搞起" />
```

{{<figure src="https://i.loli.net/2020/07/18/NdceT5iEHoJn1FY.png">}}



​	

#### submit 按钮的 input 和 button 的区别

```html
<input type="submit" value="搞起" />
<button type="submit">搞起<button>
```

+   区别是：

    +   input按钮中，不能再添加内容

    +   button按钮中还可以再添加标签内容

        ```html
        <button>
          <strong>搞起</strong>
          <img src="dog.jpg" alt="狗子">
        </button>
        ```

        {{<figure src="https://i.loli.net/2020/07/18/UZXwoxYvqCtlLkI.png">}}





​	

​	

## input 标签

### 作用 

让用户输入内容 

### 属性

#### 类型

>   type: button/checkbox/email/file/hidden/ number/password/radio/ search/submit/tel/text 

{{<figure src="https://i.loli.net/2020/07/18/la24J7UpIfZqc3e.png">}}

```html
<input type="text" />

<input type="color" />

<input type="password" />

<input type="radio" name="gender" />男        <!--同一组，取相同的name-->
<input type="radio" name="gender" />女

<input type="checkbox" name="hobby" />唱      <!--同一组，取相同的name-->
<input type="checkbox" name="hobby" />跳
<input type="checkbox" name="hobby" />rap
<input type="checkbox" name="hobby" />篮球

<input type="file" multiple/>

看不见我吧：<input type="hidden">    <!-- 通常用于js自动获取并填入 -->

<textarea style="resize: none;width:50%;height:50px"></textarea>   <!--禁止改变大小-->

<select name="" id="">
  <option value="">-请选择-</option>
  <option value="1">星期一</option>
  <option value="2">星期二</option>
</select>
```



#### 其他

（js内容）

>   name/autofocus/checked/disabled/ maxlength/pattern/value/placeholder 



### 事件

（js内容）

onchange ：用户改变内容触发

onfocus：用户聚焦时触发

onblur ：用户失焦时触发



### 验证器

 （js内容）

HTML5新增功能：自带验证 	

例：

```html
<input type="text" require/>
<input type="password" require/>
```

{{<figure src="https://i.loli.net/2020/07/18/6o7mS4wF1qWygBC.png">}}





​	

​	

## 其他输入标签

### 标签

 （js内容）

+   select + option
+   textarea
+   label

### 注意事项

+   一般不监听 input 的 click 事件
+   form 里面的 input 都要有 name
+   form 里要放一个 type=submit 才能触发 submit 事件



​	

​	

##  补充工具：开启 http 服务

### http-server

1.  安装

    ```js
    yarn global add http-server
    ```

2.  使用

    ```js
    http-server . -c-1
    http-server -c-1
    hs -c-1
    ```

    -c 缓存，-c-1 不要缓存

    . 可以省略

    http-server 可缩写成 hs

    {{<figure src="https://i.loli.net/2020/07/18/4srOcgJFBWioCQ2.png">}}

3.  按住ctrl + 点击打开任意链接。地址栏追加 html 文件名，即可浏览

4.  ctrl + C 中断服务器



​	

### parcel

>   操作更简便
>
>   好像有bug ：ctrl + c 无法中断

```js
yarn global add parcel // 安装
parcel a-href.html // 使用
```

{{<figure src="https://i.loli.net/2020/07/18/sG1uWVYadSIA7Ky.png">}}


