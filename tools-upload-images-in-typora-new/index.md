# Typora 如何上传图片？（最新超详细图解）


📢 采用阿里云 OSS，更稳定、上传加载速度更快<!--more-->

​	

## 1. 安装 PicGo-Core

因为 Typora 已经原生支持 PicGo-Core，所以只需要在软件内下载一下就可以了（PS: 下面这张图就是用的自动上传，很方便）

![0](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/0.JPG)

## 2. 开通阿里云 OSS

>   打开[阿里云](https://www.aliyun.com/)官网，进入控制台，打开最左侧菜单，找到「对象存储 OSS」

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/1..JPG" alt="1." style="zoom: 67%;" />

（第一次进入都会提示需要开通 👇）

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/1.JPG" alt="1" style="zoom:50%;" />

（立即开通）

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/2.JPG" alt="2" style="zoom:50%;" />

>   进入控制台，创建 Bucket

![3](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/3.JPG)

>   起个名字，配置基本采用默认，可参考下图

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/4.JPG" alt="4" style="zoom: 67%;" />

![5](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/5.JPG)

>   创建完就是👇这样事儿的，然后进入「资源包管理——购买资源包」，准备掏银子

![6](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/6.JPG)

>   一般买个最便宜的配置就行

![7](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/7.JPG)

>   购买完成，我们去找到自己的 AccessKey，后面配置会用到

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/10.1.5.JPG" alt="10.1.5" style="zoom:50%;" />

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/10.2.5.JPG" alt="10.2.5" style="zoom:67%;" />

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/10.2.6.JPG" alt="10.2.6" style="zoom: 50%;" />

>   这里的 AccessKey ID 和 AccessKey Secret 可以保存一下（总之不要关闭）
>
>   然后，打开 OSS 控制台，就可以对照控制台信息，配置 Typora 啦

![10.3](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/10.3.JPG)



## 3. 配置 PicGo-Core

>   点击红框 1 ——【打开配置文件】

![10.0](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/10.0.JPG)

>   代码按照下面的格式无脑全选替换就行
>
>   +   我们都可以在前面的打开的页面上查看到（oss 控制台中，我应该都用红框标注了）

```json
{
  "picBed": {
    "uploader": "aliyun",
    "aliyun": {
      "accessKeyId": "",
      "accessKeySecret": "",
      "bucket": "",      // 存储空间名
      "area": "",        // 存储区域代号
      "path": "img/",    // 自定义存储路径
      "customUrl": "",   // 自定义域名，注意要加 http:// 或者 https://
      "options": ""      // 针对图片的一些后缀处理参数 PicGo 2.2.0+ PicGo-Core 1.4.0+
    }
  },
  "picgoPlugins": {}
}
```

下面是我的配置

![10.4](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/10.4.JPG)

>   写完配置，保存关闭，来验证一下
>
>   +   点击 Typora 配置图中标出的红框 2 —— 【验证图片上传选项】

<img src="https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/11.JPG" alt="11" style="zoom:80%;" />



​	

## 4. 体验效果

>   保存配置之后，我们直接在 Typora 内粘贴一张图片，就会自动提示上传中
>
>   或者在已有的本地图片上面按右键，也可以看到【上传图片】的按钮，整个操作非常便捷。

复制图片，粘贴到 Typora，效果如下

![typora upload pic](https://imgsubmit.oss-cn-beijing.aliyuncs.com/img/typora upload pic.gif)














