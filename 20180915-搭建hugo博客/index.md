# 如何用 hugo 搭建个人博客


<!--more-->

## 安装 Hugo

+   [官方教程](https://gohugo.io/getting-started/installing)  英文

+   Mac 安装方式
    +   brew install hugo 

    +   hugo version 

+   Windows 安装方式 
    +   去 [Hugo releases 页面](https://github.com/gohugoio/hugo/releases) 下载 hugo_xxx_Windows- 64bit.zip 
    +   解压，把 hugo.exe 放到 D:\Softwarelhugolhugo.exe 
    +   把 D:\Software\hugo\ 加到 PATH
    +   重启终端，运行 **`hugo version`** 查看版本



​	

## 快速搭建博客

>   [官方文档教程](https://gohugo.io/getting-started/quick-start/)，必看！ 

### 准备、提交

1.  新建 blog 目录，运行 **`hugo new site xxx.github.io-generator `**  ， xxx 为 github 用户名 。会在当前目录中创建 xxx.github.io-generator 文件夹（博客生成器）

2.  进入博客生成器目录，**`git init`**

3.  下载[主题](https://themes.gohugo.io/)文件，放到 themes 目录下 **`git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt`**

    遇到报错

    ![image-20200713192539437](https://gitee.com/samrks/PicGo-Img/raw/master/img/image-20200713192539437.png)

    ```js
    fatal: unable to access 'https://github.com/dillonzq/LoveIt.git/': error setting certificate verify locations:
      CAfile: D:/Software/Git/mingw64/ssl/certs/ca-bundle.crt
      CApath: none
    ```

    使用 git clone 出现 `fatal: unable to access 'https://github.com/...'` 的解决办法

    ```js
    git config --system http.sslverify false   // 把证书校验禁用 
    ```

4.  然后，将主题添加到站点配置中：**`echo 'theme = "LoveIt"' >> config.toml`**   // 主题目录的名称

5.  创建新文章：**`hugo new  posts/开博第一篇文章纪念.md `**

    D:\blog\samrks.github.io-generator\content\posts\开博第一篇文章纪念.md created

6.  编辑文章后，修改 **`draft: false`**。draft 表示处于草稿状态，Hugo不会真正发布它

7.  初次创建博客或修改主题，需将主题文档中给出的配置，粘贴到 **`config.toml`** 文件中。

    baseURL 配置成 **`http://[用户名].github.io/`**

8.  **`hugo server -D`**  建立本地访问 https://localhost:1313 预览博客 

9.  **`hugo`**   创建一个新的目录  public/，这就是需要提交到 github，最终生成线上博客的目录

10.  根目录下，新建 **`.gitignore`** 文件，添加 **`/public/`**。使得 /public 可以自成一个仓库

11.  进入public **`cd public`**，**`git init`**   **`git add .`**    **`git commit`**



​	

### 第一次部署

1.  登录 github，创建博客专用仓库，仓库名必须为 ： **`[用户名].github.io`** 。以我为例：samrks.github.io
2.  进入 public 目录，**`git remote add origin git@github.com:samrks/samrks.github.io.git`**
3.  **`git push -u origin master`**   
4.  进入 github 博客仓库的 [Settings](https://github.com/samrks/samrks.github.io/settings)，找到 GitHub Pages ，选择 master ，保存



​	

### 以后的部署

1.  在 xxx.github.io-creator 目录（注意确保自己不在 public 目录）里运行 `hugo new posts/第二篇博客.md`
2.  运行 `code posts/第二篇博客.md` 对文件进行编辑，注意不要把文件原本的内容 front matter 给删了，直接在后面另起一行写新内容。
3.  运行 **`hugo -D`**，得到新的 public 目录
4.  进入 public 目录 **`cd public`**，执行一下操作
    1.  **`git add .`** 注意有一个点
    2.  **`git commit -m update`**
    3.  **`git push -f`** 其中 -f 是强制上传的意思
5.  等待几分钟后，你的博客就会出现第二篇文章了！



​	

## 域名

### 购买域名

>    namesilo （方便、无需填写个人真实信息）和阿里云（中文、可能需要实名填写详细信息）

#### [namesilo](https://www.namesilo.com/)

+   进入[官网](https://www.namesilo.com)，搜索并选中需求的域名（domain），进行注册

    ![image-20200714135525837](https://gitee.com/samrks/PicGo-Img/raw/master/img/image-20200714135525837.png)

    <img src="https://gitee.com/samrks/PicGo-Img/raw/master/img/image-20200714135819333.png" alt="image-20200714135819333" style="zoom:50%;" />

+   支付，使用支付宝，需设置[支付宝邮箱](https://custweb.alipay.com/account/index.htm)

+   购买成功后，不要关闭页面等待跳转。邮箱也会收到购买成功的邮件。

    +   每年需要续费，不续费，会有一个保护期，保护期过了，别人也可以购买了

+   点击 **`Manage my Domains `**【管理我的域名】。

    初次进入，可能需要填写账户基本信息，点击create my new account。只要保证当前账户邮箱是真实的即可

+   在域名管理页面，点击蓝色圆形按钮，可以进入 【Manage DNS】 管理DNS页面



​	

#### [阿里云](https://www.aliyun.com)

1.  进入[官网](https://www.aliyun.com/)，注册一个账户，国内账户通常需要提供真实的手机号/姓名/身份证等。
2.  在【域名与网站】选项卡中，选择【域名注册】；或者直接在<u>搜索框</u>进入【域名 控制台】选择【域名注册】
3.  搜索域名，加入清单，结算
    +   个人 or 企业
    +   填写 [个人] 信息
    +   支付



​	

### 配置 GitHub Pages

+   添加域名 （仓库会多出一个 CNAME 文件，记录配置的域名）

    +   找到 github pages 中的 custom domain ，添加域名，SAVE 保存

+   不勾选 Enforce HTTPS。现在不用开启 HTTPS（不开启比较方便测试）

    +   勾选后，所有与当前仓库相关的页面，可能都需要变成 https，可能还需要申请免费证书之类的，hin麻烦




​	

### 配置 DNS

>   最终效果就是，让 4个A记录出现在域名的DNS管理页面中，就搞定了

#### 配置四条 A 记录

1.  找到 github pages 中的 custom domain ，点击 **`Learn more`**  ，找到【[配置 apex 域](https://docs.github.com/cn/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)】

2.  找到 4 个IP，配置到域名中

    ```js
    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153
    ```

##### namesilo 配置 A 记录

+   （点击蓝色圆形按钮）进入某个域名的管理页面，点击选择 A

    +   ![image-20200714152032229](https://gitee.com/samrks/PicGo-Img/raw/master/img/image-20200714152032229.png)

    +   会生成一个配置，然后依次将4个IP 多填入提交，生成4条配置。只保留这4条配置，将其他默认存在的配置删除即可，默认提供的配置都是没用的

        ![image-20200714152421752](https://gitee.com/samrks/PicGo-Img/raw/master/img/image-20200714152421752.png)

##### 阿里云 配置 A 记录

>   基本同 namesilo
>
>   +   在域名DNS解析中，添加 4 条 A 记录 IP
>   +   下拉框选择 @ 

​	

#### 测试DNS是否配置成功

>   打开命令行，运行`nslookup liubingxuan.xyz`
>
>   +   只有命令行能打印出4条A记录的IP，就说明配置的DNS生效了
>   +   刚配置完可能没法立即生效，需要等待（可能半小时、一天或更久，只能等，听天由命:cry: ）

+   Windows 用户：nslookup 域名
+   Mac 用户：dig + noall + answer 域名
+   A 记录可能要很久才会生效，等就好了




​	

### 无法访问域名

>   如果全部配置成功，但就是访问域名无法打开博客或者乱码，那就把仓库中的 CNAME（自定义域名） 删掉。settings中也 custom domain 也删掉。
>
>   只能放弃域名方案。。。



​	

## 备份博客生成器 generator

>   程序员永远都会留备份

+   新建仓库 xxx.github.io-generator

+   将本地 xxx.github.io-generator 目录，`git init`，`git add .` ，`git commit -m backup`，`git remote add origin xxx`，`git push -u origin master`  即可
    +   如果在 add 时提示执行  rm 操作，可能是主题目录下已经存在 .git 文件，主题目录本身就是一个本地仓库了，那和 generator 目录会形成一个嵌套子目录的关系，这个是非常不好的。需要把主题目录下的 .git 文件删除








