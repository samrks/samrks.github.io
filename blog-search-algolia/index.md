# Hugo 集成 Algolia 搜索


hugo 搜索功能 Algolia  <!--more-->

## 前言

[Hugo](https://gohugo.io/)是由 Steve Francis 大神(http://spf13.com/)基于`Go`语言开发的静态网站构建工具。没错你现在看到的本博客就是基于`Hugo`的，使用 Hugo 创建一个网站是非常简单的，基本上没有什么门槛，官方还提供了大量的主题供你选择，你只需要专心写的文章就行。不过有个问题是搜索，我们知道搜索属于动态行为了，如何给静态网站增加搜索功能呢？当然我们可以使用`Google`的站内搜索功能，Hugo 官方也提供了一些开源的和商业的解决方案，今天我们要介绍的就是一个非常优秀的商业解决方案：[Algolia](https://www.algolia.com/)。

## 注册

1.  前往官方网站https://www.algolia.com/ 使用 GitHub 或 Google 帐号登录。

2.  登录完成后根据提示信息填写一些基本的信息即可。

3.  注册完成后前往 [Dashboard](https://www.algolia.com/dashboard)，我们可以发现 Algolia 会默认给我们生成一个 app。

    +   默认的 app 可以在 settings → Applications 中重命名

4.  选择 Indices，添加一个新的索引，我这里命名为`blog`，创建成功后，我们可以看到提示中还没有任何记录。

    <img src="https://i.loli.net/2020/09/08/zJXo4yl8RYpWLm1.png" alt="image-20200908231415773" style="zoom:50%;" />

5.  Algolia 为我们提供了三种方式来增加记录：手动添加、上传 json 文件、API。

## 修改 config.toml 配置

以下内容，基于 Loveit 主题测试生效

```toml
[params.algolia]
  appId = "你的Application ID"
  indexName = "你的索引名字"
  searchOnlyKey = "你的Search-Only API Key"
```

执行 hugo，会自动生成 `public/index.json` 用于索引

##  上传索引文件

生成索引文件之后，我们需要上传到 Algolia 的服务器。

### 手动上传

这一步是可选的，不过还是建议跟着做一下。

点击侧栏 `Indices` ，点击 `Upload record(s)` 按钮上传上一步生成的 `index.json` 文件。
<img src="https://i.loli.net/2020/09/08/RWTrjFHQPBSwLU5.png" alt="img" style="zoom:50%;" />

上传成功之后，我们就可以马上尝试搜索了： 

<img src="https://i.loli.net/2020/02/23/yGsPjqCQBeZVA8U.png" alt="img" style="zoom: 33%;" />

可以看到搜索的关键词有相应的匹配结果，说明我们生成的索引文件是正确的。

### 自动上传

每次写完博文都手动上传索引文件无疑是痛苦的、无意义的重复劳动。

因此我们需要把上传索引文件的操作自动化，在自动部署的时候顺便完成即可。

这里我们采用npm包 [atomic-algolia](https://www.npmjs.com/package/atomic-algolia) 来完成上传操作。

-   安装 atomic-algolia 包

    ```js
    npm install atomic-algolia --save
    ```

-   修改根目录下的 `package.json` 文件（没有就新建），在 `scripts` 下添加 `"algolia": "atomic-algolia"`

    ```json
    {
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "algolia": "atomic-algolia"
      }
    }
    ```

    注意 `"test"` 那一行末尾有个英文逗号，不要漏了。

-   根目录下新建 `.env` 文件，内容如下：

    ```js
    ALGOLIA_APP_ID=你的Application ID
    ALGOLIA_INDEX_NAME=你的索引名字
    ALGOLIA_INDEX_FILE=public/index.json
    ALGOLIA_ADMIN_KEY=你的Admin API Key
    ```

    注意替换你自己 Algolia 索引的信息。
    另外特别注意 `ALGOLIA_ADMIN_KEY` 可以用来管理你的索引，所以尽量不要提交到公共仓库。可以添加到 `.gitignore` 中

-   上传索引的命令
    你可以本地执行 `npm run algolia` 查看运行效果：
    [![img](https://i.loli.net/2020/02/23/C6u38blAWG7Xnke.png)](https://i.loli.net/2020/02/23/C6u38blAWG7Xnke.png)
    可以看到我们成功添加了记录。

-   后续，可以把下面的命令加到你的部署脚本中：

    ```js
    npm run algolia // 在hugo命令后面执行
    ```
    
    <img src="https://i.loli.net/2020/09/09/W7uLBFGHP2d1Oae.png" alt="image-20200909172756935" style="zoom:50%;" />

## 参考资料

1.  [Hugo添加Algolia搜索支持](https://edward852.github.io/post/hugo%E6%B7%BB%E5%8A%A0algolia%E6%90%9C%E7%B4%A2%E6%94%AF%E6%8C%81/#%E4%B8%8A%E4%BC%A0%E7%B4%A2%E5%BC%95%E6%96%87%E4%BB%B6)
2.  [atomic-algolia 插件](https://github.com/chrisdmacrae/atomic-algolia)
3.  [Hugo 集成 Algolia 搜索](https://www.qikqiak.com/post/hugo-integrated-algolia-search/)
