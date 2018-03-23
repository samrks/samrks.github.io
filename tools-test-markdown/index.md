# 测试 Markdown 语法


 <!--more-->

本文用于测试一些 Markdown 语法和样式展现

​	

## 文章摘要

```
 <!--more-->
```

[more注释]之前的内容，会作为文章摘要显示在主页。尽量不要包含代码块、图片、表格等模块。如果 [more注释] 之前的内容为空，则将自动添加 description 内容为文章摘要，显示在主页中。

​	

## 文章置顶

```js
weight: 1   // Front-matter
```

​	

## 常用语法

>   [主题文档链接](https://hugoloveit.com/zh-cn/theme-documentation-content/#3-%E5%86%85%E5%AE%B9%E6%91%98%E8%A6%81)

```html
<font color="red">red</font>
<font color="violet">violet</font>
<font color="orange">orange</font>
<font color="blue">blue</font>
<font color="green">green</font>
```

<font color="red">red</font>、<font color="violet">violet</font>、<font color="orange">orange</font>、<font color="blue">blue</font>、<font color="green">green</font>

```
`反引号`
```

`反引号` 		

```
<table><tr><td bgcolor=orange>背景色</td></tr></table>
```

<table><tr><td bgcolor=orange>背景色</td></tr></table>

```
**加粗**
```

**加粗**   

```
_斜体_
*斜体*
```

*斜体*

```
~~删除线~~
```

~~删除线~~

```
<u>下划线</u>
```

<u>下划线</u>

```
[链接](https://gohugo.io/getting-started/quick-start/)
```

[链接](https://gohugo.io/getting-started/quick-start/)

```
[Hugo] ^ (一个开源的静态网站生成工具)
```

[Hugo]^(一个开源的静态网站生成工具)

​	

​	

## markdown 支持 emoji

在 ` config.toml` 中开启/关闭 emoji 支持

[表情包大全](https://hugoloveit.com/zh-cn/emoji-support/)

```markdown
冒号joy冒号  冒号jack_o_lantern冒号  冒号heart冒号
```

:joy:  :jack_o_lantern:  :heart:

​	

## 有序列表

1.  有序列表
    +    无序列表
    +    无序列表
2.  有序列表
3.  有序列表




​	

## 无序列表

+   无序列表

    +   无序列表
        +   无序列表
        +   在这里添加一个脚注 [^1]
+   无序列表

    

[^1]: 这是一段脚注 https://xxx 这是一段脚注

​	



## 复选框

```markdown
- [x] Write the press release
- [x] Update the website
- [ ] Contact the media
```

- [x] Write the press release
- [x] Update the website
- [ ] Contact the media



​	

## 代码块


```js
console.log("test")
```

```html
<div class="box" style="color:red;"></div>
```

```css
body{
  font-size: 20px;
}
```



​	

## 内置 Shortcodes 语法支持

### 横幅

{{< admonition type=danger title="This is a tip" >}}
一个 danger 横幅
{{< /admonition >}}

{{< admonition type=note title="This is a tip" open=false >}}
一个 note 横幅
{{< /admonition >}}

{{< admonition tip "This is a tip" true>}}
一个 tip 横幅
{{< /admonition >}}

​	

### 图片

{{< figure src="https://i.loli.net/2020/07/15/jPkMZdGYfaoB7Ne.jpg" title="Iceland (figure)" >}}

​	

点击图片放大，点击空白缩小↘

{{< image src="https://i.loli.net/2020/07/15/Q2dnHSgxCcbfhZW.jpg" caption="Iceland (`image`)"   >}}



​	

##  二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

​	
