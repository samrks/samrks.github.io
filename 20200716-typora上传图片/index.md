# Typora 如何上传图片？（图床功能）


<!--more-->

​	

Typora 是一款简单、高效而且优雅的 Markdown 编辑器，它提供了一种所见即所得的全新的 Markdown 写作体验。它把**源码编辑和效果预览**两者合二为一，在输入 Markdown 代码的时候即时生成预览效果。Typora 的一切都围绕纯粹的生产效率而设计。

Typora一直是我最喜欢的 markdown 编辑器，写博客、记笔记的首选软件。相信大部分朋友也在使用 Typora 这款软件。

但是 Typora 之前是不支持图床功能的，只能通过第三方插件实现图片自动上传的功能。不过最近 Typora 有了更新，终于支持图床功能了，新增了原生对 PicGo-Core 的调用，可以自动上传 markdown 内的图片，赶紧尝鲜一波。

​	

## 1. 安装 PicGo-Core

因为Typora已经原生支持PicGo-Core, 所以只需要在软件内下载一下就可以了(PS: 下面这张图就是用的自动上传, 很方便)

{{<image src="https://i.loli.net/2020/07/15/Cc4bTxA9mEaojZr.png" >}}

首先点击上面红色1的位置，选择 PicGo-Core，再点3（不要问我2去哪儿了....就当他不存在），并等待下载完成。

​	

## 2. 安装 smms 插件

直接点击红色5的位置，根据文本内容找到 PicGo-Core 的程序目录

{{<image src="https://i.loli.net/2020/07/15/mfJ7g9GLxShQcPE.png" >}}

**注意，上图中我们只需关心红框中的路径下的 picgo.exe 文件，其他信息都不管**

找到目录后，我们**在picgo的目录下启动命令行**，执行如下命令，并等待安装成功

```powershell
.\picgo.exe install smms-user
```

​	

## 3. 配置 PicGo-Core

安装完成之后点击红色4的位置（红标顺序不代表点击顺序），打开 PicGo-Core 的配置文件，按照下面的格式无脑全选替换就行

```json
{
  "picBed": {
    "current": "smms-user",
    "uploader": "smms-user",
    "smms-user": {
      "Authorization": "这里替换成你自己的"
    },
    "transformer": "path"
  },
  "picgoPlugins": {
    "picgo-plugin-smms-user": true
  }
}
```

没有 Authorization 的自己去这里申请一个： [https://sm.ms/home/apitoken](https://links.jianshu.com/go?to=https%3A%2F%2Fsm.ms%2Fhome%2Fapitoken)

申请前需要注册一个账号，这个没啥难度，就不细说了。

​	

## 4. 体验效果

保存配置之后，我们直接在 Typora 内粘贴一张图片，就会自动提示上传中

{{<image src="https://i.loli.net/2020/07/15/davDboUKiGqzMpP.gif">}}

或者在已有的本地图片上面按右键，也可以弹出上传图片的按钮，整个操作非常便捷。


