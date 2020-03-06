# 前端也得懂点儿 HTTP (下) 




🤞🏻 “Nobody knows HTTP better than me !! ” 🤥<!--more-->

​	

## 先导

-   安装 Node.js 8+
-   理解 IP 和 端口
-   理解 URL 路径和查询参数



​	

## 演示 Node.js Server ⭐

### 请求与响应模型

<img src="https://i.loli.net/2020/08/05/u2Av5ORN6mdkGLE.png" alt="image-20200805141128921" style="zoom: 67%;" />

+   服务器，就是没有显示器的电脑



​	

### 如何发请求

方法

-   用 Chrome 地址栏
-   用 curl 命令

概念

-   帮你发请求的工具叫做「用户代理」
    -   如果使用 Chrome 地址栏发送请求，那么 Chrome 就是我们的「用户代理」
    -   如果使用 curl 命令发送请求，那么 curl 就是我们的「用户代理」
-   「用户代理」 英文名 User Agent



​	

### 如何做出一个响应 ⭐

>   用 Chrome 地址栏 或 用 curl 命令 ，可以发出一个请求
>
>   +   那么如何做出一个响应呢？（演示自己发请求-自己响应的过程）

需要用到编程

-   Node.js 有一个 http 模块可以做到

-   新建项目目录 node-demo / server.js，将下面代码粘入  ⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐

    >   代码细节先不管，直接复制使用。（注意这块目的不是学 nodejs，而是搞清楚 http 的请求和响应）

    ```js
    var http = require('http')
    var fs = require('fs')
    var url = require('url')
    var port = process.argv[2]
    
    if (!port) {
      console.log('请指定端口号好不啦？\nnode server.js 8888 这样不会吗？')
      process.exit(1)
    }
    
    var server = http.createServer(function (request, response) {
      var parsedUrl = url.parse(request.url, true)
      var pathWithQuery = request.url
      var queryString = ''
      if (pathWithQuery.indexOf('?') >= 0) { queryString = pathWithQuery.substring(pathWithQuery.indexOf('?')) }
      var path = parsedUrl.pathname
      var query = parsedUrl.query
      var method = request.method
    
      /******** 从这里开始看，上面不要看 ************/
    
      console.log('有个傻子发请求过来啦！路径（带查询参数）为：' + pathWithQuery)
    
      if (path === '/') {
        response.statusCode = 200
        response.setHeader('Content-Type', 'text/html;charset=utf-8')
        response.write(`二哈`)
        response.end()
      } else if (path === '/x') {
        response.statusCode = 200
        response.setHeader('Content-Type', 'text/css;charset=utf-8')
        response.write(`body{color: red;}`)
        response.end()
      } else {
        response.statusCode = 404
        response.setHeader('Content-Type', 'text/html;charset=utf-8')
        response.write(`你输入的路径不存在对应的内容`)
        response.end()
      }
    
      /******** 代码结束，下面不要看 ************/
    })
    
    server.listen(port)
    console.log('监听 ' + port + ' 成功\n请在空中转体720度然后用电饭煲打开 http://localhost:' + port)
    ```


注意事项

-   这些代码就是服务器代码，一般放在服务器上
-   path 是不带查询参数的路径 /x
-   query 是查询参数的对象形式 {a:1}
-   queryString 是查询参数的字符串形式 ?a=1
-   pathWithQuery 是带查询参数的路径，一般不用
-   request 是请求对象
-   response 是响应对象
-   \n 表示回车



​	

#### 运行机制⭐

1.  在终端中启动应用：

    1.  运行 `node server.js`  未指定**端口号**，会有提示
    2.  按照提示执行即可  `node server.js 8888`  或者  `node server 8888`  这句话就意味着我们的电脑会开一个端口 8888，这个端口被 server.js 监听着
    3.  这时候只要有人请求了8888 端口，就会走入 server.js 的代码中，我们注释的那段代码就会运行一遍。每接收到一次请求，就运行一遍

2.  用 curl 发出请求 `curl http://127.0.0.1:8080/xxxx`。server.js 接收到请求，会做出响应。

    <img src="https://i.loli.net/2020/08/05/E4yJm2lQgW5MPja.png" alt="image-20200805172004902" style="zoom: 80%;" />

    <img src="https://i.loli.net/2020/08/05/GKM71s6Xjf5xSQk.png" alt="image-20200805171047473" style="zoom: 80%;" />

    +   如果响应内容乱码，可能是 windows 系统的关系

3.  添加路由

    1.  编辑 server.js 文件，添加 if else（限定条件，访问某个路径，响应对应内容）

    2.  重新运行 node server.js 8888（修改服务代码，需要重启）

        ```js
        if (path === '/') {
          console.log('有人访问/了')
          response.end('这就是访问/，响应的内容\n')   // 回车\n
        } 
        ```

         <img src="https://i.loli.net/2020/08/05/QwYFABoug47XR12.png" alt="image-20200805182322737" style="zoom:80%;" />

        下面是，server.js 监听到 curl 命令 请求根路径时 执行的 console.log(…)

         <img src="https://i.loli.net/2020/08/05/pYz1dFxSNVyanDf.png" alt="image-20200805182425895" style="zoom:80%;" />

        

4.  后台启动应用：  

    1.  `touch log `  创建一个 log 文件

    2.  `node server.js 8888 >log log 2>&1 &`

        返回的数字1144就是这个**进程的 ID**（目前这个进程已经在后台运行了）

    <img src="https://i.loli.net/2020/08/05/qer1vxujZEUwhdb.png" alt="image-20200805184943729" style="zoom:80%;" />

    +   运行后，服务器在后台启动，不占用当前终端
    +   怎么关闭这个后台进程呢？
        +   执行 `kill -9 xxxx`   xxxx为后台进程的 id数字
        +   kill -9 是最厉害的杀进程的方法



​	

#### 代码逻辑

语法

```js
`这种字符串` 里面可以回车
'这种字符串' 里面要回车，只能用 \n 表示
```

逻辑

1.  每次收到请求都会把中间的代码执行一遍

2.  用 if else 判断路径，并返回响应

3.  如果是已知路径，一律返回 200

4.  如果是未知路径，一律返回 404

5.  Content-Type 表示内容的「类型/语法」（省略后缀，程序员从来不看后缀😎，后缀只是用来告诉计算机要用什么软件打开文件）

    ```js
    `text/html`、`text/css`
    path 不写 /x.css 而写 /x，因为 content-type 中已经声明了类型/语法，所以可省略后缀 .css
    ```

6.  response.write() 可以填写返回的内容（写入响应内容）

7.  调用 response.end() 表示响应可以发给用户了（调用 response.end() ，就会将响应发送给浏览器）

    （ 以前不写 end 就会一直等待，现在可能优化了可以不写 end，严谨起见还是都写完整，明确告知可以响应给用户了 ）

    ```js
     if (path === '/') {
        response.statusCode = 200
        response.write(`
    	    <!DOCTYPE html>
    	    <head>
    				<meta charset="UTF-8">
    	    	<link rel="stylesheet" href="/x">  // <<<<<<<<<<<<<<<<<< 将 html 和 css 结合起来
    	    </head>
    	    <body>
    				<h1>标题</h1>
    			</body>
        `)
        response.end()
      } else if (path === '/x') {
        response.statusCode = 200
        response.setHeader('Content-Type', 'text/css;charset=utf-8')
        response.write(`body{color: red;}`)
        response.end()
      }
    ```

     <img src="https://i.loli.net/2020/08/05/hUuzjymwO9W4lT3.png" alt="image-20200805184247079" style="zoom: 25%;" />

    +   如上图就是，通过 link 把 html 和 css 联系起来，成为一个网页，把这个网页通过 http 传送到浏览器上的整个过程：一个路径返回 html 字符串，一个路径响应 css  字符串。

    +   这就是李爵士发明的 URL + HTTP + HTML

​	

#### 注意符号 ``

>   反引号 ``  可以识别回车、语法
>
>   单引号 ‘’   不能识别回车语法，仅作为字符串 

<img src="https://i.loli.net/2020/08/05/MgYEQFiGpqZrXo2.png" alt="image-20200805145039135" style="zoom: 25%;" />

​	

​	

### 遥想当年李爵士

**世界上第一个服务器程序**

+   我们也写一个服务器程序

**世界上第一个网页**

-   我们在 / 路径返回一个 HTML 内容

-   然后在 /x 路径返回一个 CSS 内容

-   然后再 /y 路径返回一个 JS 内容

    ```js
     if (path === '/') {
        response.statusCode = 200
        response.write(`
    	    <!DOCTYPE html>
    	    <head>
    				<meta charset="UTF-8">
    	    	<link rel="stylesheet" href="/x">   ←←←←←←←←←←←←←← 引入 css
    	    </head>
    	    <body>
    				<h1>标题</h1>
    				<script src="/y"><script>   ←←←←←←←←←←←←←←← 引入 js
    			</body>
        `)
        response.end()
      } else if (path === '/x') {
        response.statusCode = 200
        response.setHeader('Content-Type', 'text/css;charset=utf-8')
        response.write(`body{color: red;}`)
        response.end()
      } else if(path === "/y"){
        response.statusCode = 200;
        response.setHeader("Content-Type", "text/javascript;charset=utf-8");
        response.write(`console.log('这是JS内容')`;
        response.end();
      } else {
        response.statusCode = 404;
     		response.setHeader('Content-Type', 'text/html;charset=utf-8')
        response.write(`你输入的路径不存在对应的内容`)
        response.end()
      }
    ```

**注意事项**

-   关于后缀
    -   即使写成 `path === "/y.css"` ，但如果在 Content-Type 中规定是 js 类型，就会按照 js 解析。所以，URL里的css 后缀卵用没有
    -   **Content-Type 才是决定文件类型的关键**



​	

## 体系化学习 HTTP

必须学会什么

-   基础概念（有哪些是必会的）
    -   请求、响应
-   如何调试（用的是 Node.js，可以用 log / debugger )
    -   本质是学习 HTTP，所以不要在 Nodejs 花费太多时间，只需要搞懂 nodejs 怎么调试即可
-   在哪查资料（用的是 Node.js，所以看 Node.js 文档）
-   标准制定者是谁（ HTTP 规格文档：[RFC2616](https://tools.ietf.org/html/rfc2616)（[中文](https://cloud.tencent.com/developer/chapter/13544)）等）

如何学

-   Copy-抄文档、抄老师
-   Run-放在自己的机器上运行成功
-   Modify-加入一点自己的想法，然后重新运行



​	

## HTTP 基础概念

>   +   必须点击 view source ，才能看到完整的请求、响应
>
>       <img src="https://i.loli.net/2020/08/07/ouEW2X9brp4stVj.png" alt="image-20200807182319962" style="zoom:67%;" />



### 请求

 <img src="https://i.loli.net/2020/08/07/pRfrLUAzuWgPZie.png" alt="image-20200807182834947" style="zoom: 50%;" />

+   <font color="redorage">**请求动词 路径加查询参数 协议名/版本**</font>（所有请求都按照这个格式，简化版）例：`GET   /x?wd=hi   HTTP/1.1`

-   <font color="darkorange">Host: 就是域名或  IP</font>（包括端口号）

-   <font color="darkorange">Accept: text/html</font>（表示浏览器想接收什么内容）

    -   测：根据 accept 返回不同内容

        ```js
        var accept = request.headers['accept'];  // 获取请求头中的 accept 的值，赋给变量
        if(accept.indexOf('xml')){
          response.write('我知道你想访问 XML 内容')
        }else{
          response.write(`<!DOCTYPE html><html>...</html>`)
        }
        ```

    -   浏览器可接收的内容：accept。（大多来说浏览器就是接收 html）

        默认先找 html，如果没有 html，可以接收 xhtml、xml、webp、png、igned-exchange（如下）

        ```
        Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.
        ```

-   <font color="darkorange">Content-Type: 表示请求体的格式</font>（例：text/javascript;charset=utf-8）

-   <font color="grey">回车</font>（划分请求头、请求体的界线）

-   <font color="redr">请求体（也就是上传内容）</font>



#### 细节

-   请求有三部分，用回车隔开，分别是：（与 ↑↑ 对应颜色）

    -   <font color="redorage">请求行</font>（因为只有一行，所以叫请求行）`GET   /x?wd=hi   HTTP/1.1`
    -   <font color="darkorange">请求头</font>
    -   <font color="redr">请求体</font>（请求体的格式，是在 content-type 中指定的）

-   请求动词有 GET / POST / PUT / PATCH / DELETE 等

    -   GET 用于获取内容

    -   POST 用于上传内容

        ```
        发送post请求：curl -v -X POST --data '上传内容' http://localhost:8888/
        ```

         <img src="https://i.loli.net/2020/08/07/Dn8rwhtmPEaquzO.png" alt="image-20200807181116577" style="zoom:5%;" />

-   请求体在 GET 请求中一般为空

    -   因为get请求通常用于获取内容，而请求体表示要上传的内容，所以GET请求一般没有请求体

-   文档位于 [RFC2616 第五章](https://tools.ietf.org/html/rfc2616#page-35)

-   大小写不敏感（随意），最好照着我的写



​	

### 响应

 <img src="https://i.loli.net/2020/08/07/lzRIUVeBYrWJFEd.png" alt="image-20200807182931196" style="zoom: 80%;" />

-   **<font color="redorage">协议名/版本 状态码 状态字符串</font>**
-   <font color="darkorange">Content-Type: 响应体的格式</font>
-   <font color="gray">回车</font>
-   <font color="redr">响应体</font>（也就是下载内容）

#### 细节

-   响应有三部分，用回车隔开，分别是：
    -   <font color="redorage">状态行</font>（Status LIne）
    -   <font color="darkorange">响应头</font>
    -   <font color="redr">响应体</font>（响应体的格式，在Content-Type中指定）
-   常见的状态码是**考点**
    -   200 成功
    -   404 找不到
-   文档位于 [RFC2616 第六章](https://tools.ietf.org/html/rfc2616#page-39)



​	

## 用 curl 构造请求

### curl 用法

>   curl 可以用来改请求动词、查询参数，还可以改第二部分的请求头、第三部分的请求内容…
>
>   +   什么都可以改，请求的东西都可以由自己觉得
>   +   只不过需要按照 http 的标准来写

>   前提条件：server 要处于开启的状态：node server.js 8888

例：**`curl -v http://localhost:8888`**

>   设置请求动词

-   `-X POST`
-   例：**`curl -v -X POST http://localhost:8888`**  设置为post请求
-   注意大小写

>   设置路径和查询参数

-   直接在 url 后面加
-   例：**`curl -v -X POST http://localhost:8888/xxx?id=xxx`**  
-   注：在 url 后添加 # 锚点是不会发送到服务器的

>   设置请求头

-   `-H 'Name: Value' ` 或者  `--header 'Name: Value'`
-   例：**`curl -v -X POST -H 'Accept: text/html' http://localhost:8888`**  

>   设置请求体

-   `-d '内容'`  或者  `--data '内容'`

+   **`curl -v -X POST -H 'ABC: abc' -H 'Content-Type: text/plain;charset=utf-8' -d '请求体内容' http://localhost:8888`**  

    在请求体中设置一个 ABC: abc，没有实际意义但是成立的。

    text/plain 表示上传的内容是纯文本，编码是 utf-8（中文），内容是'请求体内容'这5个字（每个字占2bytes）

    ![image-20200807224709482](https://i.loli.net/2020/08/07/VqsBNJFWxRMX4CT.png)

>   总结：可以使用 curl 随心所欲的构造一个请求

​	

### 用 Node.js 读取请求

读取请求动词

-   **`request.method`**

    ```js
    console.log('method:', request.method)   // method: GET ...
    ```

读取路径

-   **`request.url`** 路径，带查询参数

    ```js
    console.log('路径：', request.url) // 路径：/xxxx?wd=hihihi
    ```

-   **`path`** 纯路径，不带查询参数

-   **`query`** 只有查询参数

读取请求头

-   **`request.headers['Accept']` **    读取请求头中的 Accept 值

    ```js
    console.log("请求头：", request.headers)  // 请求头：{ host:xxx, ???:???, ... }
    ```

读取请求体

-   比较复杂，先不讲



​	

### 用 Node.js 设置响应

设置响应状态码 

+   **`response.statusCode = 200 `**
+   状态码可以任意设置，状态字符串会根据设置的状态码自动改变
+   但是状态码是有统一的使用规则的，如 200 规定就是表示请求成功时返回的状态码，所以不要随意改变
+   404 表示请求的网页不存在 。404 页面就是一个普通页面，是 Chrome 提供的，当访问页面不存在时提醒用户

设置响应头 

+   **`response.setHeader("Content-Type", "text/html"); `**

设置响应体 

+   **`response.write("内容"） `**

+   可追加内容



​	

### curl 的作用是什么

>   不单单是用来测试 http 的请求和响应。
>
>   curl 可以完成 Chrome 的大多基本功能，但 curl 通过命令行执行，所以不具有可视化能力

1.  下载图片

    `curl 图片路径.jpg > xxx.jpg`  （ 在命令行开启的目录中，下载图片并重命名为 xxx ）

    ```html
    curl https://i.loli.net/2020/07/15/Q2dnHSgxCcbfhZW.jpg > 3.jpg
    ```

    ![image-20200808132904053](https://i.loli.net/2020/08/08/RHsF2Ow6uKfEj51.png)

2.  测试 请求和响应

3.  … 

    curl 功能很强大，Chrome 的基本功能都可以实现，但不具有可视化



​	

## HTML / CSS / JS 的本质都是字符串

>   HTML / CSS / JS 的**本质都是字符串，不是文件**
>
>   +   本质上我们看到的网页，都是通过 html字符串 渲染的，html字符串 里面请求了 css字符串、js字符串
>   +   从演示的 server.js 中就能体现这一点

​	

​	

## console.log 调试大法

>   console.log（打印）这种调试方法，是在所有编程语言中都适用的
>
>   JS（console.log）、Java（print）、Python（print）、PHP（echo）…  语言/写法不一样，但原理相同

-   把可能有问题的代码，打印看看
-   debug 就是在不断质疑自己的过程
-   不要过分相信自己，而要相信 console.log() 可以告诉你对错
-   程序员每天都在问自己错在哪里








