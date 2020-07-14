# Markdown 语法测试


more 之前的内容，会作为文章摘要显示在主页。尽量不要包含代码块、图片、表格等模块。如果 more 之前的内容为空，则将自动添加 description 内容为文章摘要，显示在主页中。 <!--more-->

## 常用md语法测试

>   [主题文档链接](https://hugoloveit.com/zh-cn/theme-documentation-content/#3-%E5%86%85%E5%AE%B9%E6%91%98%E8%A6%81)

  ​	

## markdown 支持 emoji

在 ` config.toml` 中开启/关闭 emoji 支持

表情包大全：https://hugoloveit.com/zh-cn/emoji-support/

```
:joy:  :jack_o_lantern:  :heart:
```
:joy:  :jack_o_lantern:  :heart:





##  二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

​	

>   引用
>
>   #### 四级标题

​	

`反引号`   ==高亮==   **加粗**   *斜体*    <u>下划线</u>

这是一个[链接](https://gohugo.io/getting-started/quick-start/)哦。
​	  

+   无序列表
+   无序列表
    +   无序列表
        +   无序列表
        +   无序列表 [^1]
+   无序列表
+   无序列表

[^1]: 这是一段脚注 https://xxx 这是一段脚注 


​	

1.  有序列表1
    +    无序列表
    +    无序列表
2.  有序列表2
3.  有序列表3


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



## 内置 Shortcodes 语法支持
### 横幅

​	```markdown
{{< admonition type=danger title="This is a tip" >}}
一个 danger 横幅
{{< /admonition >}}

{{< admonition type=note title="This is a tip" open=false >}}
一个 note 横幅
{{< /admonition >}}

{{< admonition tip "This is a tip" true>}}
一个 tip 横幅
{{< /admonition >}}
```

{{< admonition type=danger title="This is a tip" >}}
一个 danger 横幅
{{< /admonition >}}

​	

{{< admonition type=note title="This is a tip" open=false >}}
一个 note 横幅
{{< /admonition >}}

​	

{{< admonition tip "This is a tip" true>}}
一个 tip 横幅
{{< /admonition >}}


​	### 图片

​```markdown
{{< figure src="https://hbimg.huabanimg.com/c9cf14bc56fa58565aae5d7ca5fa568e9c791b572e18a-AWOkh2" title="Lighthouse (figure)" >}}
```
{{< figure src="https://hbimg.huabanimg.com/c9cf14bc56fa58565aae5d7ca5fa568e9c791b572e18a-AWOkh2" title="Lighthouse (figure)" >}}

​	
```markdown
{{< image src="https://hbimg.huabanimg.com/20c21bc3f0f4c0f9cb10844c3411270bb890f2cc2e76c-pQpnYD" caption="Iceland (`image`)"   >}}
```
{{< image src="https://hbimg.huabanimg.com/20c21bc3f0f4c0f9cb10844c3411270bb890f2cc2e76c-pQpnYD" caption="Iceland (`image`)"   >}}

​	

```markdown
<img src="https://images.newscientist.com/wp-content/uploads/2019/09/02140648/1-vatnajokull-ice-cave-150131_mg_3748.jpg?width=1200" alt="image-20200714000241909" style="zoom: 80%;" />
```
<img src="https://images.newscientist.com/wp-content/uploads/2019/09/02140648/1-vatnajokull-ice-cave-150131_mg_3748.jpg?width=1200" alt="image-20200714000241909" style="zoom: 80%;" />

​	


​	



