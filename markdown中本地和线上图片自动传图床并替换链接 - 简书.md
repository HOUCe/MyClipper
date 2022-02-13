# [markdown中本地和线上图片自动传图床并替换链接 - 简书](https://www.jianshu.com/p/7a90b7d07455)

[![](https://cdn2.jianshu.io/assets/default_avatar/4-3397163ecdb3855a0a4139c34a695885.jpg)](https://www.jianshu.com/u/1d6fc4e89a52)

0.2182017.11.20 23:47:09字数 1,171阅读 5,404

我根据网上现有的两个Python脚本（[《markdown文件中图片到图床的转换》](https://link.jianshu.com/?t=https://github.com/JyHu/useful_script/blob/master/Scripts/md%E6%96%87%E4%BB%B6%E5%9B%BE%E7%89%87%E5%9B%BE%E5%BA%8A%E8%BD%AC%E6%8D%A2/%E8%87%AA%E5%8A%A8%E8%BD%AC%E6%8D%A2markdown%E6%96%87%E4%BB%B6%E4%B8%AD%E5%9B%BE%E7%89%87%E5%88%B0%E5%9B%BE%E5%BA%8A.md/)、[hxzqlh/qiniu-markdown-pics](https://link.jianshu.com/?t=http://hxzqlh.com/2016/04/26/%E6%AD%A4%E5%9B%BE%E7%89%87%E6%9D%A5%E8%87%AA%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%B9%B3%E5%8F%B0-%E6%9C%AA%E7%BB%8F%E5%85%81%E8%AE%B8%E4%B8%8D%E5%8F%AF%E5%BC%95%E7%94%A8/))以及七牛官方的[Python SDK](https://link.jianshu.com/?t=https://developer.qiniu.com/kodo/sdk/1242/python)，优化改编了一个新的**Python 3脚本**，能够自动将markdown中的本地和线上图片（如微信公众号中的图片）批量上传到[图床七牛的Bucket](https://link.jianshu.com/?t=https://portal.qiniu.com/signup?code=3loze0od2v8eq)，并自动替换markdown文件中的链接。

## 写作作用流程

在markdown软件（如Typora)中专心进行写作，随意插入本地及网络图片，不需要中断下来传个图。

本地图片可以是电脑中任意位置的文件（使用绝对路径），也可以MD同名文件夹下的图片（使用相对路径）。插入的网络图片也不用担心出现“`此图片来自微信公众平台,未经允许不可引用`”之类问题，因为脚本处理之后会直接转到到[个人的七牛空间](https://link.jianshu.com/?t=https://portal.qiniu.com/signup?code=3loze0od2v8eq)。插入网络图片如果是自己的七牛空间里面的图片，脚本会自动忽略，避免重复上传。

插完图之后也可以随时修改，不用担心图片管理混乱的问题（上传的图床的图片不方便管理）。

写作完成，浏览修改，运行

```
python 脚本路径 MD文件路径 [是否图片压缩]
```

-   0 - 不需要压缩
-   1 - 需要压缩，注意`tiniPNG`的`key`

上传之后的七牛中的图片名称为`上传日期-MD文件名/image序号.png or jpg`，[方便后期管理和备份](https://link.jianshu.com/?t=https://loomob.com/95.html)

发表markdown文件到网络，删除备份的MD.bak和清理本地图片。

![](https://upload-images.jianshu.io/upload_images/2232435-8319aa1d2e1a76ce.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/png)

mark

## 需要用到的工具

### 环境

这是一个Python 3 脚本，在Microsoft Windows 10 （Home Insider Preview China 10.0.17025）、Anaconda 4.3.27、python 3.6.2、PyCharm 2017.2.4 环境调试完成。

运行条件：

```
import re
import os
import sys
import time
import math
import imghdr
import shutil
import random
import string
import tinify
import urllib
import sqlite3
import operator
from hashlib import md5
from qiniu import Auth, put_file, etag, BucketManager
from datetime import date
import validators
```

只看了入门书《A Byte of Python》中文版，就能使用上述工具，遇到问题直接Google，中文检索结果就能解决问题。

## 实现

代码上传到[GitHub](https://link.jianshu.com/?t=https://github.com/chchuj/useful_script/blob/master/Scripts/md%E6%96%87%E4%BB%B6%E5%9B%BE%E7%89%87%E5%9B%BE%E5%BA%8A%E8%BD%AC%E6%8D%A2/md_transfer_v3)，此处应该有个流程图。代码中都有详细的注释。

![](https://upload-images.jianshu.io/upload_images/2232435-a37c928f517c3bd0.png)

流程图

  
[点击看大图](https://link.jianshu.com/?t=https://chalkit.tk/20171119-upload-image-in-markdown-file-automatically-to-qiniu/#%E5%AE%9E%E7%8E%B0)

## 脚本配置

### 存储

网上都说七牛图床好，目录是10G空间+10G流量，图片大小适当控制应该够我用了。等到我不够用的时候，也许我有经济实力购买更多空间和流量了。

先[注册七牛个人账号](https://link.jianshu.com/?t=https://portal.qiniu.com/signup?code=3loze0od2v8eq)，获取AccessKey/SecretKey、空间名称（bucket name)和域名（domain)

填入python 脚本中

```
ak = ' '
sk = ' '
domain = ' ' 
bucket = '' 
```

### 图片压缩

调用tinypng来压缩，压缩配置直接照搬[《markdown文件中图片到图床的转换》](https://link.jianshu.com/?t=https://github.com/JyHu/useful_script/blob/master/Scripts/md%E6%96%87%E4%BB%B6%E5%9B%BE%E7%89%87%E5%9B%BE%E5%BA%8A%E8%BD%AC%E6%8D%A2/%E8%87%AA%E5%8A%A8%E8%BD%AC%E6%8D%A2markdown%E6%96%87%E4%BB%B6%E4%B)的。七牛的SDK也能提供压缩，后期有空再研究一个两个官方文档，改进一下脚本。

在非Windows系统运行，还要把脚本中对应的路径的/改为\\，已在脚本对应位置注释。

## 编程感想

由于只看了《A Byte of Python》，只能算是一个初步的Python User。而且不具备系统的Python基础知识，利用google搜索来慢慢调试是非常浪费时间的。这个脚本的编写和调试就用了几个晚上的时间。还花费了一些时间来搜索有没有现有的轮子。

[《markdown文件中图片到图床的转换》](https://link.jianshu.com/?t=https://github.com/JyHu/useful_script/blob/master/Scripts/md%E6%96%87%E4%BB%B6%E5%9B%BE%E7%89%87%E5%9B%BE%E5%BA%8A%E8%BD%AC%E6%8D%A2/%E8%87%AA%E5%8A%A8%E8%BD%AC%E6%8D%A2markdown%E6%96%87%E4%BB%B6%E4%B8%AD%E5%9B%BE%E7%89%87%E5%88%B0%E5%9B%BE%E5%BA%8A.md/)的脚本修改了一下就能运行，但是这个脚本比较激进，没有备份MD文件就直接覆盖了。[hxzqlh/qiniu-markdown-pics](https://link.jianshu.com/?t=http://hxzqlh.com/2016/04/26/%E6%AD%A4%E5%9B%BE%E7%89%87%E6%9D%A5%E8%87%AA%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%B9%B3%E5%8F%B0-%E6%9C%AA%E7%BB%8F%E5%85%81%E8%AE%B8%E4%B8%8D%E5%8F%AF%E5%BC%95%E7%94%A8/)的是python 2 脚本，而且我调试时总是出现os.remove无法运行，不知道是在哪里被占用了，只能把os.remove给注释掉，最后再手动删除Temp下面的缓存文件。另外，它是把网络图片下载下来，用的是tempfile.mkstemp方法，这个文件也是要手动删除的。在阅读七牛官方SDK文档时发现它可以直接获取url，不用下载，于是决定还是自己写一个上传函数。

本来想直接有命令行中运行的，但发现安装了Anaconda之后就没办法把模块安装到原版的Python 中，pip install之后的模块都安装在conda中，不过这样也好，在conda中还能自动更新。

## markdown图床现有轮子集锦

markdown传图的各种姿势

轮子

描述（教程链接）

批量处理

上传本地图片

转存网络图片

多人维护

安全性

[CodeFalling/hexo-asset-image](https://link.jianshu.com/?t=https://github.com/CodeFalling/hexo-asset-image)

[hexo g之后html中图像链接变为绝对路径](https://www.jianshu.com/p/c2ba9533088a)，插件有时有bug。与hexo qiniu插件合用时需要修改后者

√

push到GitHub之后MD中的图片链接仍可预览

×

用的人还算多

√

微信公众号图床

管理方便，微信压缩和CDN

√

√

√

√

去除了照片信息，更安全。但数据在腾讯手上

新浪图床上传脚本

[不需要发布微博,图片只要上传就会存在于图床中](https://www.jianshu.com/p/55fd50f289c3)

×

√

×

×

图床不稳定，链接没保障

[hexo-qiniu-sync](https://link.jianshu.com/?t=https://github.com/gyk001/hexo-qiniu-sync)

[qiniu官方传图插件](https://link.jianshu.com/?t=http://error408.com/2016/08/02/Hexo%E4%B8%83%E7%89%9B%E5%9B%BE%E5%BA%8A%E4%BD%BF%E7%94%A8/)，需要学习一下如何配置。[教程2](https://link.jianshu.com/?t=http://skyhacks.org/2017/08/02/UseQiniudnToStorePic/)

√

√

×

√

√

[批量替换网络图片的py2脚本](https://link.jianshu.com/?t=http://hxzqlh.com/2016/04/26/%E6%AD%A4%E5%9B%BE%E7%89%87%E6%9D%A5%E8%87%AA%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%B9%B3%E5%8F%B0-%E6%9C%AA%E7%BB%8F%E5%85%81%E8%AE%B8%E4%B8%8D%E5%8F%AF%E5%BC%95%E7%94%A8/)

os.remove运行出错。需要手动删除缓存文件。本文已改进

√

×

√

×

√

[qiniu-image-tool](https://link.jianshu.com/?t=http://jverson.com/2017/05/28/qiniu-image-v2/)

AutoHotkey和qshell实现，一键上传图片或截图至七牛云，获取图片的markdown引用至剪贴板，并自动粘贴到当前编辑器。经常出错

×

√

√

×

√

[MWeb](https://link.jianshu.com/?t=https://bzlue.com/2016/12/16/%E4%BD%BF%E7%94%A8MWeb%E4%B8%8E%E4%B8%83%E7%89%9B%E4%BA%91%E7%BC%96%E5%86%99%E5%8D%9A%E5%AE%A2/)

for Mac

√

[批量替换本地图片的py3脚本](https://link.jianshu.com/?t=https://github.com/JyHu/useful_script/blob/master/Scripts/md%E6%96%87%E4%BB%B6%E5%9B%BE%E7%89%87%E5%9B%BE%E5%BA%8A%E8%BD%AC%E6%8D%A2/%E8%87%AA%E5%8A%A8%E8%BD%AC%E6%8D%A2markdown%E6%96%87%E4%BB%B6%E4%B)

如正文所述

√

×

√

我

√

[laobie/WriteMarkdownLazily](https://link.jianshu.com/?t=https://github.com/laobie/WriteMarkdownLazily)py2

[自动化替换 Markdown 中的本地图片引用](https://link.jianshu.com/?t=http://www.voidcn.com/article/p-uuhdtifa-bmu.html)，使用LeadCloud

√

√

×

×

√

[上传简书图片到七牛的py2脚本](https://link.jianshu.com/?t=https://github.com/DaiYue/HAMJianShuBlogBot)

√

×

√

×

√

[markdown图片实用工具](https://link.jianshu.com/?t=https://github.com/tiann/markdown-img-upload)

[利用Python和AutoHotKey实现](https://link.jianshu.com/?t=http://weishu.me/2015/10/16/simplify-the-img-upload-in-markdown/)

×

√

√

×

√

[极简图床](https://link.jianshu.com/?t=https://link.zhihu.com/?target=http%3A//yotuku.cn/)

[全球CDN加速, 支持外链、不限流量的免费图床](https://link.jianshu.com/?t=https://link.zhihu.com/?target=http%3A//yotuku.cn/)

×

√

√

√

×

[MPic-图床神器](https://link.jianshu.com/?t=http://mpic.lzhaofu.cn/)

完成度非常高的插图小软件，支持压缩

×

√

×

√

软件未开源

[GitHub上的更多开源工具](https://link.jianshu.com/?t=https://github.com/search?utf8=%E2%9C%93&q=qiniu&type=)

？

？

？

？

？

原文发表于：

[markdown中本地和线上图片自动传图床并替换链接](https://link.jianshu.com/?t=https://chchuj.coding.me/20171119-upload-image-in-markdown-file-automatically-to-qiniu/)

更多精彩内容，就在简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![  ](https://cdn2.jianshu.io/assets/default_avatar/4-3397163ecdb3855a0a4139c34a695885.jpg)](https://www.jianshu.com/u/1d6fc4e89a52)

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   Android 自定义View的各种姿势1 Activity的显示之ViewRootImpl详解 Activity...
    
-   一款Mac小软件（用于上传图片到图床生成外链接(支持生成markdown链接)） 模仿iPic应用开发，感谢Mac...
    
    [![](https://upload-images.jianshu.io/upload_images/1282350-c93174ce3104a5b4.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/66d453d99c71)

-   1.目标 实现ubuntu16.04下的七牛云作为图床便捷方式.实现自动上传图片并复制外链,设置完成后基本就是懒人...
    
    [![](https://upload-images.jianshu.io/upload_images/3454042-533b6b49eb29a26c.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/893cdde74577)
-   在 Markdown 中插图很麻烦；还好，iPic 解决了这个问题。 可是，已有 Markdown 文件中的图片该...
    
-   虽然写过不少的配色方案，但你若问我什么颜色最百搭最高级，我还是会老老实实的回答：黑与白。 生活不是秀场，我们亦不是...
    
    [![](https://upload-images.jianshu.io/upload_images/4788228-07426f4d1fb1c0c1.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/7fd4ea89762b)
-   短篇小说|不知道今天我会不会遇见你 作者清凉 A. 晚上八点多，凯文一个人从便利店7-11里走出来，手里拎着一袋食...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/4-3397163ecdb3855a0a4139c34a695885.jpg)作者清凉](https://www.jianshu.com/u/576318100b8e)阅读 127评论 0赞 0
    
    [![](https://upload-images.jianshu.io/upload_images/560061-acc131dde6bd7fc2.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/978ef9ed5e07)

-   古往今来，那些真正成就大事的人，没有哪一个是一帆风顺的。俗话说“失败是成功之母。”正因为那一次次的失败，才锤炼了他...
    
    [![](https://upload-images.jianshu.io/upload_images/3937045-08ff5f0befa1d77e.JPG?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/jpg)](https://www.jianshu.com/p/9b802d441fde)
