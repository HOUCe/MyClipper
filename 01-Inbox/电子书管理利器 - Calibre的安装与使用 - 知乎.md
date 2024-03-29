# [电子书管理利器 - Calibre的安装与使用 - 知乎](https://zhuanlan.zhihu.com/p/245217590)

_[GNUcash](https://zhuanlan.zhihu.com/p/144809982)的坑太大了，暂时还没有勇气去填，先写写怎么用Calibre来管理电子书吧。熟悉Calibre的同学也许可以直接跳转到_

## 一、Calibre的安装

Calibre更新还是非常勤奋的，目前已经升级到了版本4.23.0，下载网址（Windows 10）：

如果是Windows7或者更早版本，抱歉只能用版本3.48了

Mac用户请看这里：

下载完之后，Calibre的安装很简单，就不截屏了，按照默认设置一路点击next就可以。

## 二、Calibre的书库

安装完成后，Calibre会自动启动，我们看到的界面是这样的：

![](https://pic3.zhimg.com/v2-c999079a642e4c7b3fc227dfbfba3a72_b.jpg)

在开始使用之前，我认为有必要了解一下Calibre里最基本的一个概念：书库（Collections）。

> Calibre的书库是和电子书的存储位置相关的，需要指定一个目录。指定后在该书库下导入的书籍，都会以 **_书库名-作者-书名_** 这样的目录结构进行保存。

这里有个大坑，就是**_作者_**这个目录不支持中文，只支持拼音，对于国人的浏览体验非常差。

> 导入的书籍都会保存在书库目录下，**如果原来的文件不删除，将会在原导入目录和Calibre书库目录下分别有一份拷贝**

所以，原来的拷贝该删就删了吧

> Calibre支持多个书库，你可以创建任意多个书库，比如一个外语书库、一个科普书库、一个豆瓣8.5分书库等等...  
> Calibre除了支持创建、切换、重命名或移除书库，还支持书库数据的导出导入，帮助用户从一台电脑迁移到另外一台电脑上。

了解了Calire的书库概念以及问题，强烈建议在开始导入电子书之前，下载并安装**_Calibre中文目录补丁，_**

## 三、Calibre之导入书籍

理解了Calibre的书库的概念，我们就可以开始导入精心收藏的电子书了。按照下图，首先点击下黄色的”Calibre“按钮（这里是书库的名字），确认下书库存储位置是对的。然后点左上角的”添加书籍“

![](https://pic2.zhimg.com/v2-885912035e195d789c23bfeb9eca3efd_b.jpg)

以下copy/paste from用户手册：

1.  **从单一目录添加书籍**：本操作将打开一个文件选择窗口，并允许你指定一个目录，该目录中的书籍将会被添加。这个操作是\*内容敏感的\*，也就是说，这个操作依赖于你选择的:ref:书目<catalogs>。如果你选择的是:guilabel:书库，则书籍将被添加至书库。如果你选择的是电子书阅读设备，则书籍将被上传至设备，等等。
2.  **从目录和子目录添加图书**：允许您选择目录。递归扫描该目录及其所有子目录，并将找到的任何电子书添加到图书馆。您可以选择是否让Calibre将单个目录中存在的所有文件添加到单个帐簿记录或多个帐簿记录。Calibre假设每个目录包含一本书。假定目录中的所有电子书文件都是不同格式的同一本书。此操作与：ref：[\`](https://manual.calibre-ebook.com/zh_CN/gui.html#id1)保存到磁盘 <save\_to\_disk\_multiple>\`操作相反，即您可以在：guilabel：[\`](https://manual.calibre-ebook.com/zh_CN/gui.html#id3)保存到磁盘\`删除书籍，然后在每个目录的单个书籍模式下重新添加它们，除日期外不会丢失任何信息(这假设您没有更改保存到磁盘操作的任何设置)。
3.  **从压缩文件（ZIP/RAR）中添加多本书籍**：这一操作允许你被被选中的 ZIP 或 RAR 文件中存储的电子书文件进行添加，是一个可以避免事先将压缩文件解压、再通过前两个操作添加书籍的快捷操作。

我自己用的最多的还是从目录添加电子书：

![](https://pic3.zhimg.com/v2-d5c66213037161507028d65304eeb12a_b.jpg)

![](https://pic3.zhimg.com/v2-1841b740f99221e51ed15789d96846a2_b.jpg)

这一步要选”否“

![](https://pic3.zhimg.com/v2-6de10cc1253b0aa555481562a016c7d2_b.jpg)

选择文件夹

![](https://pic2.zhimg.com/v2-6aa1074028265b2eedb4b052e6b0d839_b.jpg)

导入中

![](https://pic3.zhimg.com/v2-c4ec31db4da244f69b85547c472cc896_b.jpg)

导入成功

让我们到书库目录下看下生成的目录文件，可以看到所有的中文作者名都是以拼音的形式展现的，易读性很差。如果需要改为中文，请打自制补丁：

![](https://pic4.zhimg.com/v2-597b9b0f189b37e8c8cbbc29d99e761f_b.jpg)

打补丁后：

![](https://pic1.zhimg.com/v2-e2419ac641bdfefdc1eed6439d33f3f8_b.jpg)

## 三、Calibre之书籍元数据管理

电子书导入后，可以添加标签，转换格式，使用起来都比较方便。这里要特别提到的是更新电子书的元数据。由于电子书的来源不同，元数据有些也是不是很完整的，比如缺书号、出版社、封面等等，Calibre提供了Amazon、Google、Douban等多个插件作为元数据更新的数据源。对于中文书籍来说，最方便的当然是使用豆瓣作为数据源，可惜之前有段时间不太好用，在2020年3月份fix之后，目前仍然有问题。

亲测目前可以使用且唯一推荐的，一个是Amazon的中国服务器（中文书），一个是Amazon的美国服务器（英文书）。以下用截图演示如何配置和使用Metadata的插件批量更新电子书的元数据：

![](https://pic1.zhimg.com/v2-507f69a200c7696ee665e441d3a6a414_b.jpg)

选择多个要更新的电子书

![](https://pic4.zhimg.com/v2-13e0c9e13d3297599021f08aa2b3317f_b.jpg)

右键选择 ”编辑元数据“ - ”下载元数据和封面“。也可以使用快捷键 Ctrl-D

![](https://pic3.zhimg.com/v2-badcea99b20bcc1d0a73a7966f872cbe_b.jpg)

点击”配置下载参数“

![](https://pic3.zhimg.com/v2-e569c8ef1383739d60724374e73f435a_b.jpg)

选择”Amazon.com&amp;amp;quot;, 然后点击“配置所选源数据”

![](https://pic2.zhimg.com/v2-133c2b5c9bfda422a7d777189b242f29_b.jpg)

选择“中国”和“亚马逊服务器”，如果是英文书可以选择“美国”，点击保存、应用

![](https://pic3.zhimg.com/v2-ea9a2c677b792dfeaba23009962aeb8a_b.jpg)

点击“下载元数据与封面”

![](https://pic3.zhimg.com/v2-32a9a2b1bf96205a286c22633acd72aa_b.jpg)

点击右下角的“任务”

![](https://pic3.zhimg.com/v2-3aafa87a2abbabe0e250907f95adc69e_b.jpg)

可以看到元数据下载的进度

![](https://pic3.zhimg.com/v2-e1ad2dbab83e704667bd675e4cef70ca_b.jpg)

元数据下载完成

这一步点击“是”就可以应用新的元数据了，或者可以逐一检查下载的元数据后再应用。可以看到有11本书的元数据/封面无法下载，一般原因是在网站找不到相应的数据。

更新完成后，右侧的元数据栏里书号一栏出现“Amazon.cn”，就表示这本书的元数据是从Amazon中国更新过的。

![](https://pic4.zhimg.com/v2-89433b77860d1bd901692ec15c9b9e9f_b.jpg)
