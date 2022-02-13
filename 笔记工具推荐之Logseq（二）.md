# 笔记工具推荐之Logseq（二）

[zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/421714726?utm_source=wechat_session&utm_medium=social&utm_oi=38400975437824&utm_campaign=shareopn)莯然 生活是种律动，需有光有影，有晴有雨

各位爱学习爱生活的小伙伴们，又到了周五，提前祝大家周末愉快呀~

在上周的分享中，给大家推荐了本小白近期在用的笔记工具Logseq，主要介绍了三个易用点，两个主要特性（双向链接和细粒度），以及一些基本语法和快捷键的使用。

回顾在这里哦 ：[莯然：笔记工具推荐之Logseq](https://zhuanlan.zhihu.com/p/418969961%22%20%5Ct%20%22_blank)

在上期文章评论里，有小伙伴提到，Logseq支持关联Zotero这一功能，是他在众多笔记软件里选择这款工具的主要原因。没错，这一功能就是如此强大~

对于很多在校生或科研工作者而言，不管是为了完成作业还是课题项目，平时都需要阅读大量文献。

若是不经加工的机械化阅读，遗忘便是最自然不过的结果，别说提取有用知识了，就是回顾一下主要内容都很难，也经常会因此对自我记忆力产生怀疑（别问我是怎么知道的....)

上面的痛点告诉我，记录文献笔记很重要。而Logseq搭配Zotero，就是管理文献笔记的渠道之一了。

本周就来给大家介绍一下，Logseq和Zotero如何关联，以及如何用Logseq来直接批阅Pdf文件~

## **Logseq关联Zotero**

首先，我们打开Logseq网页版或软件版，在右上角的位置点击（…）进入设置选项，在编辑器选项卡中找到zotero settings。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic3.zhimg.com%2Fv2-835bc9083287bf9601d77497c45a2c36_b.jpg)

在设置页面的待填项中，会看到有Zotero API key以及ID这两个涉及个人信息的栏目。这就需要我们转到Zotero 官网去查看了。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic1.zhimg.com%2Fv2-0c45405256af059d13f41a36e301e11c_b.jpg)

打开Zotero官网中生成私有密钥的[页面](https://link.zhihu.com/?target=https%3A//www.zotero.org/settings/privacy)，会看到你的user ID。我们点击 create new private key 设置密钥。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic4.zhimg.com%2Fv2-a63c91d64689408e6d60d0dee87f4ce7_b.jpg)

进去之后，在key description的空白栏中随意起个名。

在Personal Library下，可以看到有三个可勾选项，其中第一个“allow library access”让Logseq可访问Zotero的文库；第二个“allow notes access”让Logseq可访问你在Zotero中的笔记；第三个是允许第三方更改你的文库。（一般勾选前两个即可~）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic2.zhimg.com%2Fv2-4d3aabfa4a7037e1ef1cdeafc677e745_b.jpg)

点击最下方save key之后,将生成的字符串复制下来，就可以返回到Logseq中zotero settings的页面,填写在相应区域啦。

接下来我们打开Logseq的每日笔记页面，**输入/Zotero，就可以在弹出来的框里输入要导入的文献标题或关键词等**，这时很神奇的事情就出现了，**Logseq会自动地将相关信息列出以供选择**，是不是很棒？

如果有小伙伴操作到这里后，没有在Logseq的页面看到有相关文献信息弹出，请不要慌张，你可能只是没有设置好文件路径而已，往下看哦~

第一，我们打开Zotero，在编辑--首选项--高级--文件和文件夹，进行文件位置的设置。在框①里，指定保存pdf、链接等文件附件的目录，后续在zotero条目里添加的附件都会存储在这一文件夹中。

在框②里，指定保存Zotero中数据的目录（建议存在D盘~）。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic1.zhimg.com%2Fv2-d200be58b2b8be68d44be7ed2e10e0a0_b.jpg)

第二，返回到Logseq当中zotero settings的部分，可以看到有两个需要设置directory的地方。这里需要和上面zotero中设置的路径一一对应。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic1.zhimg.com%2Fv2-5b17cf340080a0308d5e8821aa2967d8_b.jpg)

设置好文件夹位置之后，再重新尝试在Logseq中关联zotero，应该就正常了~

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic4.zhimg.com%2Fv2-3fb171ec01d50b5f4757aaf819f4f43f_b.jpg)

熟悉完Logseq关联Zotero的操作后，再来看下一部分——

## **如何在Logseq中批阅pdf**

通常我们会选择在专门的pdf阅读编辑软件当中读文献，比如福昕pdf阅读器、Adobe Acrobat DC等等。

而使用Logseq的小伙伴呢，就又多了一种方式，因为直接在这款笔记工具当中就可以标注文献内容~

在这里主要有三种方式：

方法1比较简单粗暴--**直接将pdf文件拖入到Logseq的块区域内**。

拖进后，点击该文件名，就会在左侧页面看到打开的文件内容。（这里以王老师写的关于复杂系统仿真的论文为例）

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic2.zhimg.com%2Fv2-42287a86b80e60884f877657bb29cd9d_b.jpg)**

**当看到需要标注的内容时，我们直接选中文本高亮表示，在高亮区域右击鼠标，会有“复制引用”、“复制文本”、“转到注解”等选项，根据需要选择就可。**

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic2.zhimg.com%2Fv2-f90ea50dad57cb8cdf42349fe639b019_b.jpg)**

**如果点击“复制引用”后粘贴到右侧区域，再点击区块的内容，右侧pdf也会自动跳转定位到文献的该部分内容。**

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic2.zhimg.com%2Fv2-bfc2b439b5426a99909294eb7de362a5_b.jpg)**

法2就是上文第一部分提到的使用zotero关联。

如下图，点击标题会看到文献的一些基本信息：

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic2.zhimg.com%2Fv2-9f3f35b166a8bd5e0b31f598e8efe3a1_b.jpg)**

法3，使用Logseq引用链接的方式。

格式是：感叹号 + 方括号\[文件.pdf\]+原括号(file:///绝对路径)

以上就是关于Logseq的补充分享啦，可能Logseq还有许多其他功能等待我去探索，欢迎已经是资深用户的小伙伴们在评论区补充，帮助本小白进步哦~

（PS:如果有小伙伴动动小手点赞关注一下，我会更开心的hahaha）

[查看原网页: zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/421714726?utm_source=wechat_session&utm_medium=social&utm_oi=38400975437824&utm_campaign=shareopn)