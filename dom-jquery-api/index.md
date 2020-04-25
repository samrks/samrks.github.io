#  jQuery 快速上手


<!--more-->

## 参考

-   《[阮一峰：jQuery设计思想](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)》

-   《 [jQuery 中文文档](https://www.jquery123.com/)》

​		

## 如何获取和使用  jQuery

-    jQuery 的官方网址是：http://jQuery.com/，从这里可以获取  jQuery  的最新版本。
     -    jQuery中文文档：https://www.bootcdn.cn/jquery/  
-    使用的话，就是导入这份 js 文件。
-    导入方式是在页面，通过`<script>`标签导入

```html
<script type="text/javascript" src="jQuery.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
```

导入之后，就可以使用  jQuery  的语法了。

​	

##  jQuery  版本

1.x：兼容IE678,使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用1.x版本就可以了，最终版本：1.12.4 (2016年5月20日)

2.x：不兼容IE678，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，最终版本：2.2.4 (2016年5月20日)

3.x：不兼容IE678，只支持最新的浏览器。需要注意的是很多老的 jQuery 插件不支持3.x版。目前该版本是官方主要更新维护的版本。

>   维护IE678是一件让人头疼的事情，一般我们都会额外加载一个CSS和JS单独处理。值得庆幸的是使用这些浏览器的人也逐步减少，PC端用户已经逐步被移动端用户所取代，如果没有特殊要求的话，一般都会选择放弃对678的支持。

​	

##  jQuery 对象

-    jQuery 对象就是通过 jQuery 包装DOM对象后产生的对象。
-    jQuery 对象是  jQuery 独有的。
-   如果一个对象是  jQuery 对象，那么它就可以使用  jQuery 里的方法：例如 `$('#i1').html()`  。

```js
$("#i1").html()  // 意思是: 获取id值为 i1 的元素的 html 代码。其中 html() 是  jQuery  里的方法。 
// 相当于： 
document.getElementById("i1").innerHTML;
```

+   虽然  jQuery 对象是包装 DOM 对象后产生的，但是  jQuery 对象无法使用 DOM 对象的任何方法，同理 DOM对象也没不能使用  jQuery 里的方法。

>   一个约定，我们在声明一个 jQuery 对象变量的时候在变量名前面加上$：

```js
let $variable =  jQuery 对像
let variable = DOM对象
$variable[0]  //  jQuery 对象转成DOM对象
```

拿上面那个例子举例， jQuery 对象和 DOM 对象的使用：

```js
$("#i1").html();       //  jQuery 对象可以使用 jQuery 的方法
$("#i1")[0].innerHTML  // DOM对象使用DOM的方法
```

​	

​	

## 一、选择网页元素

>   jQuery 的基本设计思想和主要用法，就是"选择某个网页元素，然后对其进行某种操作"。这是它区别于其他Javascript库的根本特点。

>   使用 jQuery 的第一步，往往就是将一个选择表达式，放进构造函数 jQuery ()（简写为$），然后得到被选中的元素。

选择表达式可以是CSS选择器：

```js
$(document) //选择整个文档对象

$('#myId') //选择ID为myId的网页元素

$('div.myClass') // 选择class为myClass的div元素

$('input[name=first]') // 选择name属性等于first的input元素
```

​	

也可以是 jQuery 特有的表达式：

```js
$('a:first') //选择网页中第一个a元素

$('tr:odd') //选择表格的奇数行

$('#myForm :input') // 选择表单中的input元素

$('div:visible') //选择可见的div元素

$('div:gt(2)') // 选择所有的div元素，除了前三个

$('div:animated') // 选择当前处于动画状态的div元素
```

​	

## 二、改变结果集

 jQuery 设计思想之二，就是提供各种强大的过滤器，对结果集进行筛选，缩小选择结果。

```js
$('div').has('p'); // 选择包含p元素的div元素

$('div').not('.myClass'); //选择class不等于myClass的div元素

$('div').filter('.myClass'); //选择class等于myClass的div元素

$('div').first(); //选择第1个div元素

$('div').eq(5); //选择第6个div元素
```

​	

有时候，我们需要从结果集出发，移动到附近的相关元素， jQuery 也提供了在DOM树上的移动方法：

```js
$('div').next('p'); //选择div元素后面的第一个p元素

$('div').parent(); //选择div元素的父元素

$('div').closest('form'); //选择离div最近的那个form父元素

$('div').children(); //选择div的所有子元素

$('div').siblings(); //选择div的同级元素
```

​	

## 三、链式操作

 jQuery 设计思想之三，就是最终选中网页元素以后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来，比如：

```js
$('div').find('h3').eq(2).html('Hello');
```

​	

分解开来，就是下面这样：

```js
$('div') //找到div元素

  .find('h3') //选择其中的h3元素

  .eq(2) //选择第3个h3元素

  .html('Hello'); //将它的内容改为Hello
```

这是 jQuery 最令人称道、最方便的特点。它的原理在于每一步的 jQuery 操作，返回的都是一个 jQuery 对象，所以不同操作可以连在一起。

 jQuery 还提供了.end()方法，使得结果集可以后退一步：

```js
$('div')

  .find('h3')

  .eq(2)

  .html('Hello')

  .end() //退回到选中所有的h3元素的那一步

  .eq(0) //选中第一个h3元素

  .html('World'); //将它的内容改为World
```

​	

## 四、元素的操作：取值和赋值

>   操作网页元素，最常见的需求是取得它们的值，或者对它们进行赋值。

>   jQuery 设计思想之四，就是使用同一个函数，来完成取值（getter）和赋值（setter），即"取值器"与"赋值器"合一。到底是取值还是赋值，由函数的参数决定。

```js
$('h1').html(); //html()没有参数，表示取出h1的值

$('h1').html('Hello'); //html()有参数Hello，表示对h1进行赋值
```

​		

常见的取值和赋值函数如下：

```js
.html() // 取出或设置html内容

.text() // 取出或设置text内容

.attr() // 取出或设置某个属性的值

.width() // 取出或设置某个元素的宽度

.height() // 取出或设置某个元素的高度

.val() // 取出某个表单元素的值
```

需要注意的是，如果结果集包含多个元素，那么赋值的时候，将对其中所有的元素赋值；取值的时候，则是只取出第一个元素的值（.text()例外，它取出所有元素的text内容）。

​	

## 五、元素的操作：移动

>    jQuery 设计思想之五，就是提供两组方法，来操作元素在网页中的位置移动。一组方法是直接移动该元素，另一组方法是移动其他元素，使得目标元素达到我们想要的位置。
>
>   假定我们选中了一个div元素，需要把它移动到p元素后面。

第一种方法是使用 .insertAfter()，把div元素移动p元素后面：

```js
$('div').insertAfter($('p'));
```

第二种方法是使用.after()，把p元素加到div元素前面：

```js
$('p').after($('div'));
```

​	

表面上看，这两种方法的效果是一样的，唯一的不同似乎只是操作视角的不同。但是实际上，它们有一个重大差别，那就是返回的元素不一样。第一种方法返回div元素，第二种方法返回p元素。你可以根据需要，选择到底使用哪一种方法。

使用这种模式的操作方法，一共有四对：

```js
.insertAfter()  和 .after()   ：在现存元素的外部，从后面插入元素

.insertBefore() 和 .before()  ：在现存元素的外部，从前面插入元素

.appendTo()     和 .append()  ：在现存元素的内部，从后面插入元素

.prependTo()    和 .prepend() ：在现存元素的内部，从前面插入元素
```

​	

## 六、元素的操作：复制、删除和创建

除了元素的位置移动之外， jQuery 还提供其他几种操作元素的重要方法。

复制元素使用 .clone()。

删除元素使用.remove()和.detach()。两者的区别在于，前者不保留被删除元素的事件，后者保留，有利于重新插入文档时使用。

清空元素内容（但是不删除该元素）使用.empty()。

创建新元素的方法非常简单，只要把新元素直接传入 jQuery 的构造函数就行了：

```js
$('<p>Hello</p>');

$('<li class="new">new list item</li>');

$('ul').append('<li>list item</li>');
```

​	

​	

## 汇总

###  jQuery  选择器 ———

>   选择器通过标签名、属性名或内容对DOM元素进行快速、准确的定位。根据所获取页面中元素的不同，可以将选择器分为：基本选择器、层次选择器、过滤选择器和表单选择器。

### 1、基本选择器

>   使用最频繁的选择器，包括元素 ID、Class 名、元素名等。

id选择器：

```js
$('#element-id')
```

class选择器：

```js
$('.class-name')
```

元素选择器：

```js
$('element-name')
```

​	

### 2、层次选择器

>   通过DOM元素间的层次关系获取元素，主要层次关系包括后代、父子、相邻、兄弟关系等。

根据祖先元素匹配所有后代元素：

```js
$('ancestor descendant')
```

根据父元素匹配所有的子元素：

```js
$('parent > child')
```

匹配所有紧接在prev元素后的相邻元素：

```js
$('prev + next')
```

匹配prev元素之后的所有兄弟元素：

```js
$('prev ~ siblings')
```

​	

### 3、过滤选择器

>   过滤选择器根据某类过滤规则进行元素的匹配，以:开头。过滤选择器又分为：简单过滤选择器、内容过滤选择器、可见性过滤选择器、属性过滤选择器、子元素过滤选择器和表单对象属性过滤选择器。

#### 3.1 简单过滤选择器

获取页面第一个和最后一个X元素：

```js
$('element:first')
$('element:last')
```

获取所有索引值为偶数和奇数的元素，索引值从0开始：

```js
$('element:even')
$('element:odd')
```

获取等于、大于和小于索引值的元素：

```js
$('element:eq(index)')
$('element:gt(index)')
$('element:lt(index)')
```

获取除给定的选择器外的元素：

```js
$('element:not(selector)')
```

#### 3.2 内容过滤选择器

获取包含给定文本的元素：

```js
$('element:contains(text)')
```

获取所有不包含子元素或者文本的空元素：

```js
$('element:empty')
```

获取含有选择器所匹配的元素的元素：

```js
$('element:has(selector)')
```

获取含有子元素或文本的元素：

```js
$('element:parent')
```

#### 3.3 可见性过滤选择器

获取所有不可见的元素，或者type为hidden的元素：

```js
$('element:hidden')
```

获取所有可见的元素：

```js
$('element:visible')
```

#### 3.4 属性过滤选择器

获取包含给定属性的元素：

```js
$('element[attribute]')
```

获取属性是给定值的元素：

```js
$('element[attribute=value]')
```

获取属性不是给定值的元素：

```js
$('element[attribute!=value]')
```

获取属性是以给定值开始的元素：

```js
$('element[attribute^=value]')
```

获取属性是以给定值结束的元素：

```js
$('element[attribute$=value]')
```

获取属性是包含给定值的元素：

```js
$('element[attribute*=value]')
```

#### 3.5 子元素过滤选择器

获取父元素下的第一个、最后一个、唯一一个子元素：

```js
$('parent:first-child')
$('parent:last-child')
$('parent:only-child')
```

获取父元素下的特定位置的元素，索引值从1开始：

```js
$('parent:nth-child(eq|even|odd|index)')
```

#### 3.6 表单对象属性过滤选择器

获取表单中所有属性为可用的元素：

```js
$('element:enabled')
```

获取表单中所有属性为不可用的元素：

```js
$('element:disabled')
```

获取表单中所有被选中的元素：

```js
$('element:checked')
```

获取表单中所有被选中option的元素：

```js
$('element:selected')
```

​	

### 4、表单选择器

>   通过它可以在页面中快速定位某表单对象。

获取所有input、textarea、select等input元素：

```js
$('form:input')
```

获取所有单行文本框：

```js
$('form:text')
```

获取所有密码框：

```js
$('form:password')
```

获取所有单项按钮：

```js
$('form:radio')
```

获取所有复选框：

```js
$('form:checkbox')
```

获取所有提交按钮：

```js
$('form:submit')
```

获取所有图像域：

```js
$('form:image')
```

获取所有重置按钮：

```js
$('form:reset')
```

获取所有按钮：

```js
$('form:button')
```

获取所有文件域：

```js
$('form:file')
```

​	

​	

### DOM 操作 ———

>   在与页面中的元素进行交互式的操作中，主要包括对元素属性、内容、值、CSS等的操作。同时，还有对页面节点的操作，包括节点元素的创建、插入、复制、替换、删除等操作。

### 1、元素属性操作

在  jQuery  中，可以对元素属性进行获取、设置、删除等操作。

获取指定属性名的元素属性：

```js
$(selector).attr(name)
```

设置元素属性值，key为属性名称，value为属性值：

```js
$(selector).attr(key, value)
```

设置多个属性值：

```js
$(selector).attr({keyN:valueN})
```

删除指定属性名的元素属性：

```js
$(selector).removeAttr(name)
```

​		

### 2、元素内容操作

>   在 jQuery 中，可以获取和设置元素的HTML或文本内容。

获取元素的HTML/文本内容：

```js
$(selector).html()
$(selector).text()
```

设置元素的HTML/文本内容：

```js
$(selector).html(value)
$(selector).text(value)
```

>   两者的区别是，html() 方法仅支持 HTML 类型的文档，不支持 XML。而 text() 方法不仅支持 HTML 类型，也支持 XML 类型。

​	

### 3、元素值操作

>   在 jQuery 中，可以获取和设置元素的值。

获取元素的值：

```js
$(selector).val()
```

设置元素的值：

```js
$(selector).val(value)
```

>   Tips：通过 val().join(",") 获取 select 标签中的多个选项值。

​	

### 4、元素样式操作

>   在 jQuery 中，可以直接设置样式、增加CSS类别、类别切换、删除类别等操作。

为指定name的样式设置值：

```js
$(selector).css(name, value)
```

为元素增加样式类：

```js
$(selector).addClass(class)
```

切换不同的样式类：

```js
$(selector).toggleClass(class)
```

删除元素的样式类：

```js
$(selector).removeClass(class)
```

​	

### 5、创建节点元素

>   如果要在页面中添加某个元素，需要先通过构造函数创建节点元素：

```js
$(html)
```

​	

### 6、插入节点元素

>   按照插入元素的位置区分，可以分为内部和外部两种插入方法。

#### 6.1 内部插入节点

向所选择的元素内部追加/前置内容：

```js
$(selector).append(content)
$(selector).prepend(content)
```

向所选择的元素内部追加/前置function方法所返回的内容：

```js
$(selector).append(function())
$(selector).prepend(function())
```

把所选择的元素追加/前置到另一个指定的元素集合中：

```js
$(selector).appendTo(content)
$(selector).prependTo(content)
```

#### 6.2 外部插入节点

向所选择的元素外部追加/前置内容：

```js
$(selector).after(content)
$(selector).before(content)
```

向所选择的元素外部追加/前置function方法所返回的内容：

```js
$(selector).after(function())
$(selector).before(function())
```

把所选择的元素追加/前置到另一个指定的元素：

```js
$(selector).insertAfter(content)
$(selector).insertBefore(content)
```

​	

### 7、复制节点元素

>   将某个元素节点复制到另一个节点之后。

复制匹配的DOM元素并且选中复制成功的元素：

```js
$(selector).clone()
```

在复制时将该元素的所有行为也进行复制：

```js
$(selector).clone(true)
```

​	

### 8、替换节点元素

将所有选择的元素替换成指定的HTML或DOM元素：

```js
$(selector).replaceWith(content)
```

将所有选择的元素替换成指定selector的元素：

```js
$(selector).replaceAll(selector)
```

一旦完成替换，被替换元素中的全部事件将会消失。

​	

### 9、删除节点元素

删除指定的元素：

```js
$(selector).remove()
```

删除指定的元素，但保留被移除元素的事件：

```js
$(selector).detach()
```

清空所选择的页面元素的内容，但不移除该元素：

```js
$(selector).empty()
```

​	

### 10、通用操作

>   这类操作不需要选择元素就可以直接使用。

```js
$.trim()           // 去除字符串两端的空格。
$.each()           // 遍历一个数组或对象。
$.inArray()        // 返回一个值在数组中的索引位置。如果该值不在数组中，则返回-1。
$.grep()           // 返回数组中符合某种标准的元素。
$.extend()         // 将多个对象，合并到第一个对象。
$.makeArray()      // 将对象转化为数组。
$.type()           // 判断对象的类别（函数对象、日期对象、数组对象、正则对象等等）。
$.isArray()        // 判断某个参数是否为数组。
$.isEmptyObject()  // 判断某个对象是否为空（不含有任何属性）。
$.isFunction()     // 判断某个参数是否为函数。
$.isPlainObject()  // 判断某个参数是否为用"{}"或"new Object"建立的对象。
$.support()        // 判断浏览器是否支持某个特性。
```



