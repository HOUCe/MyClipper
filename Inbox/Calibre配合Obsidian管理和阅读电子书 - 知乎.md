# [Calibre配合Obsidian管理和阅读电子书 - 知乎](https://zhuanlan.zhihu.com/p/438010687)

![Calibre配合Obsidian管理和阅读电子书](https://pic3.zhimg.com/v2-e14d9a9714a527a3420c0312b381ee9f_1440w.jpg?source=172ae18b)

## **我的想法**

_我有这样一个坏习惯。不爱看书，反倒喜欢收书，特别是电子书。_

收集的电子书多了，想要用的时候却找不到。后来，朋友介绍了一款免费而且强大的电子书管理软件——calibre。这个软件真是太好使了，而且还可以给电子书转换格式，配合kindle等使用，爱折腾的化还可以搞calibre-web。今天简单介绍一下与obsidian笔记软件的配合。

![](https://pic1.zhimg.com/v2-8decc51924ba453fbfe035a27bff43a0_b.jpg)

## **内容提要**

### **安装calibre**

calibre下载安装很简单，文末有官网链接。支持各种系统。

![](https://pic4.zhimg.com/v2-258bff09027ba8bda18ce44b917adbab_b.jpg)

安装好之后打开看看：

![](https://pic1.zhimg.com/v2-52dcad8f011faa3e1bf01db1d2694768_b.jpg)

### **链接obsidian**

这里通过Calibre启动一个后台服务，然后把iframe嵌入到obsidian笔记里面进行阅读。点击`连结/共享`,然后点击`启动内容服务器`，启动之后记下来`IP地址和端口号`。

![](https://pic2.zhimg.com/v2-473e53cae954607ce843ebbb69ad3345_b.jpg)

把上面的IP和端口号复制替换到下面，可以按需调整width、height。

```
<iframe src="http://IP地址:端口号/" width=100% height="740" border="0" frameborder="0"> </iframe>
```

把上面的文本嵌入到obsidian某篇笔记中。

![](https://pic1.zhimg.com/v2-7a53350f496ce9c4e2638e47a5633d00_b.jpg)

点击图书打开, 就可以阅读了。

## **相关链接**

**[calibre - E-book management (calibre-ebook.com)](https://calibre-ebook.com/)**

**[使用 Obsidian 阅读电子书（epub、mobi、azw等）](https://zhuanlan.zhihu.com/p/408789657)**

发布于 2021-11-26 13:27

### 文章被以下专栏收录

[

![World of Lee](https://pica.zhimg.com/4b70deef7_xs.jpg?source=172ae18b)



](https://www.zhihu.com/column/c_1352583027856359424)

## [](https://www.zhihu.com/column/c_1352583027856359424)

世界太大我太小，沉迷于网络世界，与现实渐行渐远……

Drag to outliner or Upload

Close
