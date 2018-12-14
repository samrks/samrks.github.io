# 个人博客添加评论系统 Valine + LeanCloud


hugo 博客添加评论系统 Valine  <!--more-->



>   Valine - 一款快速、简洁且高效的无后端评论系统。
>
>   Valine 诞生于2017年8月7日，是一款基于Leancloud的快速、简洁且高效的无后端评论系统。
>   理论上支持但不限于静态博客，目前已有Hexo、Jekyll、Typecho等博客程序在使用Valine。

所以，理论上它也是支持 **Hugo** 的， 实践证明，确实如此。其特性如下：

-   快速
-   安全
-   Emoji 😉
-   无后端实现
-   MarkDown 全语法支持
-   轻量易用(~15kb gzipped)
-   文章阅读量统计 v1.2.0-beta1+

下面就讲一下如何一步步添加 [Valine](https://valine.js.org/) 支持的。

**Tips:**

-   整个过程，是以Loveit主题为例的，其它主题操作大同小异。
-   配置之前应该先阅读[Valine快速开始](https://valine.js.org/quickstart.html)



​	

## Leancloud相关配置

评论系统依赖于 leancloud，所以需要先在leancloud中进行相关的准备工作。

-   登录 或 注册 [LeanCloud（国际版）](https://leancloud.app/)
    必须验证邮箱和手机号

-   成功后，进入后台点击左上角的创建应用：

    <img src="https://pichome-1254392422.cos.ap-chengdu.myqcloud.com/20180708153104380829479.png" alt="img" style="zoom: 33%;" />

-   创建好应用，进入应用，点击【组件】

    <img src="https://i.loli.net/2020/09/10/OsA25v7fpRh8X9G.png" alt="image-20200909000346264" style="zoom: 50%;" />

-   左边栏找到【设置】，然后点击【应用Key】，此时记录出现的 App ID 和 App Key，后面配置文件中会用到：

    <img src="https://pichome-1254392422.cos.ap-chengdu.myqcloud.com/20180708153104457148134.png" alt="img" style="zoom: 25%;" />

-   因为评论和文章阅读数统计依赖于存储，所以还需要建立两个新的存储 Class
    左边栏找到并点击【存储】，点击【创建Class】
    创建两个存储Class，分别命名为: `Counter` 和 `Comment`；

    **仅修改 ACL 的 write 权限**

    <img src="https://i.loli.net/2020/09/09/q2D9dCorHQ8tx3w.png" alt="image-20200909004906488" style="zoom: 50%;" />

-   还需要为应用添加安全域名，左边栏点击【设置】，找到【安全中心】，点击后会看到【安全域名】设置框，输入博客使用的域名，点击保存即可：

    <img src="https://pichome-1254392422.cos.ap-chengdu.myqcloud.com/20180708153104592457270.png" alt="img" style="zoom: 25%;" />



​	

## config.toml 添加参数

添加 **Valine** 参数项：

```toml
[params.page.comment.valine]
  enable = true
  appId = "xxxxxxxxxxxxxxxxxxxxxx"
  appKey = "xxxxxxxxxxx"
  placeholder = "说点什么吧...（提醒：填写邮箱，若有人回复您，您将及时收到提醒邮件！）"
  avatar = "mp"
  meta= ""
  pageSize = 10
  lang = ""
  visitor = false
  recordIP = true
  highlight = true
  enableQQ = false
  serverURLs = ""
  # LoveIt 新增 | 0.2.6 emoji 数据文件名称, 默认是 "google.yml"
  # ("apple.yml", "google.yml", "facebook.yml", "twitter.yml")
  # 位于 "themes/LoveIt/assets/data/emoji/" 目录
  # 可以在你的项目下相同路径存放你自己的数据文件:
  # "assets/data/emoji/"
  emoji = "apple.yml"
```

上面几项内容的含义，这里简单一说，具体还是要看 [Valine官网中配置相关的内容](https://valine.js.org/configuration.html)：

| 参数       | 用途                                                         |
| ---------- | ------------------------------------------------------------ |
| enable     | 这是用于主题中配置的，不是官方Valine的参数，**true**时控制开启此评论系统 |
| appId      | 这是在 [leancloud](https://leancloud.cn/) 后台应用中获取的，也就是上面提到的 **App ID** |
| appKey     | 这是在 [leancloud](https://leancloud.cn/) 后台应用中获取的，也就是上面提到的 **App Key** |
| notify     | 用于控制是否开启邮件通知功能，具体参考[邮件提醒配置](https://github.com/xCss/Valine/wiki/Valine-评论系统中的邮件提醒设置) |
| verify     | 用于控制是否开启评论验证码功能                               |
| avatar     | 用于配置评论项中用户头像样式，有多种选择：mm, identicon, monsterid, wavatar, retro, hide。详细参考：[头像配置](https://valine.js.org/avatar.html) |
| placehoder | 评论框的提示符                                               |
| visitor    | 控制是否开启文章阅读数的统计功能, 详情阅读[文章阅读数统计](https://valine.js.org/visitor.html) |



## 完善评论通知  ⭐️⭐️

**Valine** 评论邮件提醒功能不太健全，通知邮件中没有文章直达链接，**Valine** 官网中提供了评论系统第三方功能扩展[Valine](https://github.com/zhaojun1998/Valine-Admin)链接，按照链接中的说明，非常详细的步骤，一步步很容易实现完备的评论系统后台管理以及邮件提醒功能，部分高级配置[点我](https://github.com/zhaojun1998/Valine-Admin/blob/master/高级配置.md#自定义邮件服务器)了解。这里简单列举步骤如下：

### 步骤

1.  进入leancloud，【**云引擎**】【**部署项目**】【**git**】

    填写仓库地址： https://github.com/zhaojun1998/Valine-Admin

    填写分支： master 

    <img src="https://i.loli.net/2020/09/09/1dgcOzHKBvJV5EP.png" alt="image-20200909010214580" style="zoom:50%;" />

2.  此外，你需要设置云引擎的环境变量以提供必要的信息，点击云引擎的设置页，设置如下信息：

    **必选参数**

    -   `SITE_NAME` : 网站名称。
    -   `SITE_URL` : 网站地址, **最后不要加 `/` 。**
    -   `SMTP_USER` : SMTP 服务用户名，一般为邮箱地址。
    -   `SMTP_PASS` : SMTP 密码，一般为授权码，而不是邮箱的登陆密码，请自行查询对应邮件服务商的获取方式
    -   `SMTP_SERVICE` : 邮件服务提供商，支持 `QQ`、`163`、`126`、`Gmail`、`"Yahoo"`、`......` ，全部支持请参考 : [Nodemailer Supported services](https://nodemailer.com/smtp/well-known/#supported-services)。 --- *如这里没有你使用的邮件提供商，请查看[自定义邮件服务器](https://github.com/zhaojun1998/Valine-Admin/blob/master/高级配置.md#自定义邮件服务器)*
    -   `SENDER_NAME` : 寄件人名称。

        （图略）

3.  设置完环境变量，必须**重新部署**，邮件提醒功能才会生效

4.  **云引擎** —— **设置** —— **云引擎域名**（如：jackma），保存

    1.   然后进入 **存储** —— **_User** 添加一个用户，只需 User，password，email 三个信息即可。（为了安全考虑，此 `email` 必须为配置中的 `SMTP_USER` 或 `TO_EMAIL`， 否则不允许登录）
    2.   此时可以使用定义的主机域名登录**后台管理系统**了，地址为：[[云引擎域名jackma].avosapps.us]()，用户名为刚设置的邮箱。

5.  **LeanCloud 休眠策略**

    1.   首先需要添加环境变量，`ADMIN_URL : 云引擎域名`，如：https://jackma.avosapps.us（重启生效）

    2.   然后点击【云引擎】【定时任务】【创建定时任务】，按照图片上填写：`0 0/20 7-23 * * ?`

         <img src="https://i.loli.net/2020/09/09/UwKDZ2XPcdWa7oE.png" alt="image-20200909014721962" style="zoom: 33%;" />

    3.  添加后，要记得**点击启用**

        启用成功后，每 20 分钟在【云引擎】的 - 应用【日志】中可以看到提示

        <img src="https://i.loli.net/2020/09/09/fQkMTSKFp5z1mGd.png" alt="image-20200909020148474" style="zoom:67%;" />

6.  登录上面主机域名进入后台瞅一瞅：

    <img src="https://i.loli.net/2020/09/09/SE4c8jlLFw6tZDU.png" alt="img" style="zoom: 33%;" />

7.  我自己沙发了一条评论

    1.  日志提示：

        ![image-20200909020718733](https://i.loli.net/2020/09/09/HBxUEh2rif6eCyt.png)

    2.  进入后台后可以看到：

        <img src="https://i.loli.net/2020/09/09/5b8Y1fzHWp7DCyc.png" alt="image-20200909171608041.png" style="zoom:50%;" />

    3.  同时，我也收到了通知邮件：

        <img src="https://i.loli.net/2020/09/09/DKmHWPE46Z8sFCf.png" alt="image-20200909012309644" style="zoom:50%;" />

至此完成了 **Valine** 评论系统的添加和完善，喝杯咖啡☕️庆祝一下！

​	

​	

## 解决：自动唤醒失败

>   免费版的 LeanCloud 容器，是有强制性休眠策略的，不能 24 小时运行：
>
>   -   每天必须休眠 6 个小时
>   -   30 分钟内没有外部请求，则休眠。
>   -   休眠后如果有新的外部请求实例则马上启动（但激活时此次发送邮件会失败）。
>
>   分析了一下上方的策略，如果不想付费的话，最佳使用方案就设置定时器，每天 7 - 23 点每 20 分钟访问一次，这样可以保持每天的绝大多数时间邮件服务是正常的。

>   使用 cron-job 解决 Valine-admin 因流控原因自动唤醒失败的问题

免费的体验版，基本崩了。

评论提醒功能，可能必须经常手动部署。阅读量仍正常记录，不受牵连。

![image-20200910183223427](https://i.loli.net/2020/09/16/nxOv8uGZkzfTHos.png)

>   Valine-admin由于Leancloud流控原因，自动唤醒任务可能会失败
>
>   所以这里介绍一个使用第三方计划任务网站进行定时唤醒 Valine-admin 的方法。

### 注册 cron-job 帐号

注册地址：https://cron-job.org/en/signup/

>   注册时的时区请选择  `Asia/Shanghai`

### 添加一个计划任务

1.  登陆之后依次点击 `Members`，`cronjobs`，`Create cronjob`

2.  Title, Address
    +   Title 可以随便填一个
    +   Address 填写你的云引擎环境变量的 ADMIN_URL，也就是Leancloud的Web 主机域名。

3.  Schedule

    选择 User-defined 进行自定义设置（按住 Ctrl  可多选）

    -   Days of month: 全选
    -   Days of week: 全选
    -   Months: 全选
    -   Hours: 你需要在哪个时间段唤醒就选择什么  （每天强制休眠 6 小时，推荐选 7-23-0 唤醒）
    -   Minutes: 选择 0 , 20 , 40

4.  Notifications

    可以不用修改，也可以根据自己的需要修改

5.  Common

    勾选Save responses, 保存唤醒日志

6.  点击Create cronjob

​	

## 参考

-   [Valine](https://valine.js.org/)
-   [Valine-Admin](https://github.com/zhaojun1998/Valine-Admin)
-   [leancloud-休眠策略](https://github.com/zhaojun1998/Valine-Admin/blob/master/高级配置.md#leancloud-休眠策略)
-   [hugo博客添加评论系统Valine](https://www.smslit.top/2018/07/08/hugo-valine/)
-   [将 Valine 切换至 leancloud 国际版](https://co5.me/2019/190818-valine.html)
-   https://juejin.im/post/6844904175298428941






