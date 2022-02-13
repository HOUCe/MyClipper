# [Logseq学习笔记：文件嵌入 - 知乎](https://zhuanlan.zhihu.com/p/411147573)

![Logseq学习笔记：文件嵌入](https://pic1.zhimg.com/v2-0e4f963eeeb8f0db81b961a8fd81e6e5_1440w.jpg?source=172ae18b)

**1\. 如何嵌入本地图片并显示图片内容？**

参考语法，\[\]内可为空：

```
![](../assets/Avatar.png)
```

assets文件夹为Logseq笔记根目录下。

效果：

![](https://pic1.zhimg.com/v2-39e67f1fa7b79359c40dd523ad2c0134_b.jpg)

**2\. 如何嵌入本地图片但不显示图片内容？**

参考语法：

```
[Avatar.png](../assets/Avatar.png)
```

效果，为可点击链接：

![](https://pic4.zhimg.com/v2-8f8bc3b2d4a451ba97da2c5d7e45d1df_b.png)

PS：\[\]内不可为空，否则不显示内容；点击该链接，**弹出系统默认图片查看器进行查看**。

**3\. 如何嵌入在线PDF？**

参考语法：

```
![标点符号用法 - 中华人民共和国教育部](http://www.moe.gov.cn/ewebeditor/uploadfile/20150113091548267.pdf)
```

效果：

![](https://pic1.zhimg.com/v2-a0d84940b84c4028830e03ee16b1e68c_b.png)

点击上述链接，显示PDF文件下载中：

```
Downloading PDF file http://www.moe.gov.cn/ewebeditor/uploadfile/20150113091548267.pdf
```

下载完毕后，点击PDF，即可在Logseq阅读、标注PDF，无需借助第三方软件。

**4\. 如何嵌入Office文档？**

参考语法：

```
[花名册.xlsx](../assets/花名册.xlsx)
[札记.docx](../assets/札记.docx)
```

效果，为可点击链接：

![](https://pic1.zhimg.com/v2-62fc0b26015f945926292388d7350c64_b.png)

PS：点击后，会弹出Excel、Word程序打开该文件。

**5\. 如何嵌入本地视频？**

参考语法：

```
![](../assets/NBA加时赛绝杀.mp4)
```

效果：可直接在Logseq中观看。

![](https://pic2.zhimg.com/v2-f2f24fbbf5b13dd608546a2de231f499_b.jpg)

**6\. 如何嵌入在线视频？**

使用方法：YouTube视频右键点击“复制内嵌代码”，粘贴至Markdown文档。

![](https://pic2.zhimg.com/v2-b21f172e1c52600e7877a5a5da65e8a9_b.jpg)

发布于 2021-09-16 20:46

Drag to outliner or Upload

Close
