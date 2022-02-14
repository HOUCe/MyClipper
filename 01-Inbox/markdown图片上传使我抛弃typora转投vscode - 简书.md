# [markdown图片上传使我抛弃typora转投vscode - 简书](https://www.jianshu.com/p/868b3a2028f8)

[![](https://upload.jianshu.io/users/upload_avatars/8746907/94f03975-c583-4050-a41e-19d1f8e2ed2b.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/jpg)](https://www.jianshu.com/u/ed7ec68c9e0e)

142019.10.21 11:12:05字数 942阅读 10,515

使用markdown语法写文章真的很省事，再也不用关心文章的格式问题，专心以内容为中心写作。

我用的是windows系统，以前使用的是typora软件写markdown文章，用了大概也有一年左右的时间。不得不说，typora得所见即所得的设计令人非常舒适，不用再使用单独的一个窗口来预览markdown格式了。

然而最令人头疼的是文件中如果粘贴了图片，typora是直接引用的本地地址，例如`E:\path\to\img\file.png`，如下图所示。这时你把markdown文件发送给同事，或者将markdown文章复制下来粘贴到博客平台上，是无法正常显示图片的。这是typora的硬伤，因为我要发布文章到两个简书和掘金两个平台。  

![](https://upload-images.jianshu.io/upload_images/8746907-2ca22a6fd2461946.png?imageMogr2/auto-orient/strip|imageView2/2/w/892/format/png)

20191021104750.png

鉴于此，我上网搜索了很多解决方案。了解到`图床`的功能可以解决这个问题。

> 图床功能就是可以将你本地粘贴的图片上传到网络(所谓的图床)，并给你返回该图片的地址。这样无论你的文章发给谁、发在哪个平台，只要有网络，都是能正常访问到这个图片的。

windows版本的typora原生并不支持该功能，同时typora也不支持安装插件，无疑给typora判了死刑，曾经一度放弃了图床这个想法。后来还是不死心，继续上网搜索，了解到了vscode以及其强大的插件，重要的事情说三遍，**强大的插件！强大的插件！！强大的插件！！！** 下载试用，感觉比typora更舒适，遂决定放弃了typora转投vscode。

## vscode写markdown

vscode虽然也是text文本编辑器，但其是原生支持写markdown的。将你的文件保存为.md结尾的问题，在编辑器的右上角会出现预览按钮，点击即可预览markdown，如下图。  

![](https://upload-images.jianshu.io/upload_images/8746907-27bb62570213deb1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/png)

20191021105800.png

## vscode添加图床插件PicGo

我最需要的功能来了，打开vscode插件搜索框（crtl + shift + x），输入PicGo，下载安装该插件，重启vscode。划重点：

**该插件会自动将你需要的图片上传到SM.MS服务器，完全免费，无需任何配置，安装插件即可使用。我就用的这个默认服务器。**

当然你也可以配置使用你自己的图床。如微博、阿里云等其他主机。  

![](https://upload-images.jianshu.io/upload_images/8746907-697f8bf5edbbd4b5.png)

20191021110226.png

## 粘贴图片

PicGo支持两种方式在vscode上粘贴图片：

-   `ctrl+alt+u`：上传剪贴板中的图片到服务器。
-   `ctrl+alt+e`：打开文件浏览器选择图片上传。

因为我们知道，vscode是文本编辑器，如果你直接在vscode中使用ctrl+v粘贴图片，是无法粘贴成功的。正确的姿势是，我们使用截图工具，如微信的截图工具，截图成功后会保存在剪贴板中，这时按`ctrl+alt+u`就开始上传图片了。或者，按`ctrl+alt+e`，选择一张图片上传即可。

OK，强大的插件使得vscode支持上传图片，可以更加愉快的使用markdown写作了，一次写作，处处粘贴，舒爽。

以上。

喜欢我的文章的小伙伴欢迎关注我的公众号“东瓜东瓜”哦，自己上微信搜一下。

更多精彩内容，就在简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![  ](https://upload.jianshu.io/users/upload_avatars/8746907/94f03975-c583-4050-a41e-19d1f8e2ed2b.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/jpg)](https://www.jianshu.com/u/ed7ec68c9e0e)

[快给我饭吃](https://www.jianshu.com/u/ed7ec68c9e0e "快给我饭吃")热爱生活积极向上且乐于分享的Java程序猿，分享自己的生活和技术，欢迎关注我。我的博客地址，和...

总资产4共写了9.1W字获得411个赞共123个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   目录 第一章 初次接触vscode第二章 vscode快捷键的使用第三章 vscode的界面配置第四章 vscod...
    
    [![](https://upload.jianshu.io/users/upload_avatars/14616778/7125eba0-a2b1-4a0c-85cf-8e98f4e90f86?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/jpg)小宫同学\_](https://www.jianshu.com/u/44f676e24c73)阅读 25,546评论 3赞 30
    
    [![](https://upload-images.jianshu.io/upload_images/14616778-fe3bdc1ab34eeba3.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/cb8d2194d5ef)
-   时间：2018-03-24 作者：魏文应 一、说明 Atom 这个软件是开源免费的，由业界良心 Github 推出...
    
    [![](https://upload.jianshu.io/users/upload_avatars/9181524/e0ff4dde-4746-4de1-8ea4-dfa65fdf70cb.png?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/png)秋的懵懂](https://www.jianshu.com/u/04b6bd32b52b)阅读 3,564评论 5赞 10
    
    [![](https://upload-images.jianshu.io/upload_images/9181524-0683505376d9e82d.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/6b54e2eb9ae2)

-   出发点 我是一个比较懒的人，平时看书看了也就是看了，并没有把自己看书的心得体会做一个总结并记录下来，然而岁月就像一...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/1-04bbeead395d74921af6a4e8214b4f61.jpg)小云柳](https://www.jianshu.com/u/39e5303c0491)阅读 7,499评论 0赞 9
    
-   Typora for Markdown 语法 ==说明，本文档转自该博客，结合自身稍作修改，有些效果没有显示出来，...
    
-   选择适合自己的Markdown编辑器 博客地址：http://www.cnblogs.com/gibbonnet/...
    
    [![](https://upload-images.jianshu.io/upload_images/1737690-f354540a97bd8b4b.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/bc505dfae8d9)
-   中午以为舍友要睡觉，衣服就赶紧洗了洗，结果出来就剩下一个舍友了，我说你们都不睡了？舍友说睡不着，不睡了。我说哦那你...
    

-   在离婚率极高，小三遍地的年代，我来讲诉一对年龄差21岁夫妻的幸福生活。他们既不是再婚，也不是有钱人或高官娶了小...
    
    [![](https://upload.jianshu.io/users/upload_avatars/5507090/8d94d6bf-1cae-41ad-aeda-7f18d0cbbb73.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/jpg)禅风墨羽](https://www.jianshu.com/u/e234912d52d3)阅读 1,447评论 27赞 16
    
    [![](https://upload-images.jianshu.io/upload_images/5507090-5781fce4c05baba8.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/b5b79a1ede4e)
-   初秋，瓢泼大雨，打在湖面溅起小小的水花和相互碰撞后消融的阵阵涟漪，湖的尽头升起层层水雾，一幢木屋的轮廓若隐若现。木...
    
-   春天 羌笛何须怨杨柳，春风不度玉门关。春暖花开、春意盎然、春色满园、春色撩人、春树幕云、春满人间、春风化雨、春风满...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/11-4d7c6ca89f439111aff57b23be1c73ba.jpg)凤辉](https://www.jianshu.com/u/1d460727a51d)阅读 136评论 0赞 0
    
    [![](https://upload-images.jianshu.io/upload_images/11486341-272a376ce261cfd3.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/55e67a173f00)
-   阿志，我的小学同学，和我一样出生于90年，虽然贴着90后的标，却因从小的生活环境，过着和80后相差无几的生活。 其...
