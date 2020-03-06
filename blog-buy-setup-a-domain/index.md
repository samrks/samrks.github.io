# 个人博客：💰购买并配置域名


买个域名玩玩！  <!--more-->



## 个人博客绑定域名

### 购买域名

>    namesilo （方便、无需填写个人真实信息）和阿里云（中文、可能需要实名填写详细信息）

#### [namesilo](https://www.namesilo.com/)

+   进入[官网](https://www.namesilo.com)，搜索并选中需求的域名（domain），进行注册

    <img src="https://i.loli.net/2020/07/15/dyfIo3bi6pnH78U.png" alt="image-1" style="zoom:80%;" />

    <img src="https://i.loli.net/2020/07/15/4DgPxfCA2FK5O8k.png" alt="image-2"  />



+   支付，使用支付宝，需设置[支付宝邮箱](https://custweb.alipay.com/account/index.htm)
+   购买成功后，等待跳转。邮箱也会收到购买成功的邮件。

    +   每年需要续费，不续费，会有一个保护期，保护期过了，域名就重新开放购买
+   点击 **`Manage my Domains `**【管理我的域名】。

    +   初次进入可能需要填写基本信息。点击 create my new account。只要保证当前账户邮箱是真实的即可
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

+   添加域名 

    +   找到 github pages 中的 custom domain ，添加域名，SAVE 保存
    +   仓库会多出一个 CNAME 文件，记录配置的域名

+   注意：不勾选 Enforce HTTPS。现在不用开启 HTTPS（不开启比较方便测试）

    +   勾选后，所有与当前仓库相关的页面，可能都需要变成 https，可能还需要申请免费证书之类的，hin麻烦




​	

### 配置 DNS

>   最终效果是，让 4个A记录出现在域名的DNS管理页面中，就搞定了

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

    +   ![image-20200714152032229](https://i.loli.net/2020/09/15/4zCaRt7fprTy1xl.png)

    +   会生成一个配置，然后依次将4个IP 多填入提交，生成4条配置。只保留这4条配置，将其他默认存在的配置删除即可，默认提供的配置都是没用的

        ![image-20200714152421752](https://i.loli.net/2020/09/15/CMc7yOmNpe3lU9q.png)

##### 阿里云 配置 A 记录

>   基本同 namesilo
>
>   +   在域名DNS解析中，添加 4 条 A 记录 IP
>   +   下拉框选择 @ 

​	

#### 测试DNS是否配置成功

>   打开命令行，运行`nslookup liubingxuan.xyz`
>
>   +   命令行能打印出4条A记录的IP，就说明配置的DNS生效了
>   +   刚配置完可能没法立即生效，需要等待（可能半小时、一天或更久，只能等，听天由命:cry: ）

+   Windows 用户：nslookup 域名
+   Mac 用户：dig + noall + answer 域名
+   A 记录可能要很久才会生效，等就好了




​	

### tip

>   如果要放弃域名方案 :sob:
>
>   +   把仓库中的 CNAME文件（自定义域名） 删掉。settings 中 custom domain 也删掉。




