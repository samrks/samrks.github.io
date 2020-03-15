# 博客之路1：如何用 hugo 搭建个人博客


第一篇正式博文，我想给大家分享下我的博客的创建过程吧！:1st_place_medal:<!--more--> 

​	

>   Hugo 是由 Go 语言实现的静态网站生成器。简单、易用、高效、易扩展、快速部署。

​	

## 安装 Hugo

+   [官方教程](https://gohugo.io/getting-started/installing)  英文

+   Mac 安装方式
    +   brew install hugo 

    +   hugo version 

+   Windows 安装方式 
    +   去 [Hugo releases 页面](https://github.com/gohugoio/hugo/releases) 下载 hugo_xxx_Windows- 64bit.zip 
    +   解压，把 hugo.exe 放到 D:\Software\hugo\hugo.exe 
    +   把 D:\Software\hugo\ 加到 PATH
    +   重启终端，运行 **`hugo version`** 查看版本



​	

## 快速搭建博客

>   [官方文档教程](https://gohugo.io/getting-started/quick-start/)，必看！ 

### 准备、提交

1.  新建 blog 目录，运行 **`hugo new site xxx.github.io-generator `**  ， xxx 为 github 用户名 。会在当前目录中创建 xxx.github.io-generator 文件夹（博客生成器）

2.  进入博客生成器目录，**`git init`**

3.  选择并下载[主题](https://themes.gohugo.io/) ，放到 themes 目录下 **`git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt`**

    +   遇到报错 ↘

        ```js
        fatal: unable to access 'https://github.com/dillonzq/LoveIt.git/': error setting certificate verify locations:
          CAfile: D:/Software/Git/mingw64/ssl/certs/ca-bundle.crt
          CApath: none
        ```

    +   解决办法：使用 git clone 出现 `fatal: unable to access 'https://github.com/...'` ，执行代码 ​↓

        ```js
        git config --system http.sslverify false   // 把证书校验禁用 
        ```

        

4.  然后，将主题添加到站点配置中：**`echo 'theme = "LoveIt"' >> config.toml`**   // 主题目录的名称

5.  创建新文章：**`hugo new  posts/first_post.md `**

    `D:\blog\xxx.github.io-generator\content\posts\first_post.md` created

6.  编辑文章后，修改 **`draft: false`**。draft : true 表示处于草稿状态，此时Hugo不会真正发布它

7.  初次创建博客或修改主题，需将主题文档中给出的配置，粘贴到 **`config.toml`** 文件中。

    baseURL 配置成 **`http://[用户名].github.io/`**

8.  **`hugo server -D`**  建立本地访问 https://localhost:1313 预览博客 

9.  **`hugo`**   创建一个新的目录  public/，这就是需要提交到 github，最终生成线上博客的目录

10.  根目录下，新建 **`.gitignore`** 文件，添加 **`/public/`**。使得 /public 可以自成一个仓库

11.  进入public **`cd public`**，**`git init`**   **`git add .`**    **`git commit`**



​	

### 第一次部署

1.  登录 github，创建博客专用仓库，仓库名必须为 ： **`[用户名].github.io`** 。
2.  进入 public 目录，**`git remote add origin xxx`**
3.  **`git push -u origin master`**   
4.  进入 github 博客仓库的 [Settings](https://github.com/xxx/xxx.github.io/settings)，找到 GitHub Pages ，选择 master ，保存
5.  通过 [http://[用户名].github.io](http://xxx.github.io) 就能访问博客 



​	

### 以后的部署

1.  在 xxx.github.io-creator 目录（注意确保自己不在 public 目录）里运行 `hugo new posts/第二篇博客.md`
2.  运行 `code posts/第二篇博客.md` 对文件进行编辑，注意不要把文件原本的内容 front matter 给删了，直接在后面另起一行写新内容。
3.  **`hugo server -D`**  建立本地访问 https://localhost:1313 预览博客 
4.  运行 **`hugo -D`**，得到新的 public 目录
5.  进入 public 目录 **`cd public`**，执行一下操作
    1.  **`git add .`** 注意有一个点
    2.  **`git commit -m update`**
    3.  **`git push -f`** 其中 -f 是强制上传的意思
6.  等待几分钟后，你的博客就会出现第二篇文章了！
    +   通过 [http://[用户名].github.io](http://xxx.github.io) 访问博客 



​	

## 备份博客生成器 generator

>   程序员永远都会留备份

+   新建仓库 xxx.github.io-generator

+   将本地 xxx.github.io-generator 目录，`git init`，`git add .` ，`git commit -m backup`，`git remote add origin xxx`，`git push -u origin master`  即可
    +   如果在执行 add 时，提示我们需要执行 rm 操作，可能是因为主题目录下已经存在 .git 文件，主题目录本身就是一个本地仓库了，那和 generator 目录会形成一个嵌套子目录的关系，that's not good . 我们需要把主题目录下的 .git 文件删除








