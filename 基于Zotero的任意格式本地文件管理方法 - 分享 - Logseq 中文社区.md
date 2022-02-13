# 基于Zotero的任意格式本地文件管理方法 - 分享 - Logseq 中文社区

source:: https://cn.logseq.com/t/topic/194

刚看了2021年的logseq线上聚会，其中有位同志的提议我很感兴趣，他的需求是使用logseq管理本地文档，希望能在logseq中加入任意格式文件（如word，excel，ppt，pdf等）的链接功能，以方便对本地文件进行记录管理。这个功能也是我十分想要的。  
目前logseq尚未开发该功能，我也一直在尝试其他软件实现该功能，目前已经有初步的效果，和大家分享一下。  
我主要是使用[Zotero文献管理软件 33](https://www.zotero.org/)实现该功能的。Zotero是一款开源的文献管理软件，类似于Endnote、Mendeley。它其中一个特点是不仅可以管理论文文献，还可以管理任意的文档、甚至网页。基本界面如下图所示：

[![](https://cn.logseq.com/uploads/default/optimized/1X/e18c5679f9f1405cd84cb5e884294155a5c4fe32_2_690x388.png)](https://cn.logseq.com/uploads/default/original/1X/e18c5679f9f1405cd84cb5e884294155a5c4fe32.png)

中间是以表格形式展示条目的信息，右边是单个条目的属性信息，左边是条目分组的文件夹。  
Zotero有个很少被人知道的功能，那就是zotero专用的链接，形式如“zotero://select/library/items/9R9XRTGT”。它的功能类似网页链接，当你在其他地方点击该链接时，zotero就会弹出来并选中该条目。具体使用方法很简单，和网页链接一样，在markdown中，使用`[链接到word文件](zotero://select/library/items/9R9XRTGT)`即可。![QQ截图20210130161617](https://cn.logseq.com/uploads/default/original/1X/d7f950a81c42a8a5d99083c8c69a21b44c70c610.png)  
点击蓝色的文字链接即可转跳zotero条目，然后双击条目就能打开该文件，或者右键该条目打开所在文件夹。![新建位图图像](https://cn.logseq.com/uploads/default/original/1X/a75a0b6db3ae328d53bab8f69982a7a798e6ad52.png)  
当然，该方法不仅在logseq中适用，其他软件也同样适用，如Obsidian、typora等。

谢谢！最近客户端版本实现了对任意文件的链接，我的需求就是这样，非常感谢你的回复，也感谢天生团队的开发。

-   #### 创建时间
    
    21年1月
    
-   [
    
    #### 最后回复
    
    ](https://cn.logseq.com/t/topic/194/12)
    
    [](https://cn.logseq.com/t/topic/194/12)21年4月
    
-   1.8k
    
    #### 浏览
    
-   8
    
    #### 用户
    
-   1
    
    #### 赞
    
-   4
    
    #### 链接
    
-   ![](https://cn.logseq.com/user_avatar/cn.logseq.com/li360/64/127_2.png "看天")
    
    ![](https://cn.logseq.com/user_avatar/cn.logseq.com/cai-98/64/167_2.png "Cai 98")
    

要想顺利使用该功能，zotero还需另外安装一个插件[zutilo 47](https://github.com/wshanks/Zutilo)。安装好以后在打开插件设置界面，勾选下面的选项。

[![QQ截图20210130163212](https://cn.logseq.com/uploads/default/original/1X/d10f1cf00832f2c250c201fee6adc880eff9dc7c.png)](https://cn.logseq.com/uploads/default/original/1X/d10f1cf00832f2c250c201fee6adc880eff9dc7c.png "QQ截图20210130163212")

右键点击文件，选择“复制选中条目链接”即可将该文件的zotero链接复制到剪切板，然后可以在其他软件中粘贴。  
![新建位图图像](https://cn.logseq.com/uploads/default/original/1X/090c757468cbdee2fb8af323897dfeee02468e15.png)  
需要注意的是，zotero中是分为普通条目和文件，它们的链接是不一样的，你可以在一个普通条目下附上任意各附件文件，在复制链接时最好选择复制附件文件的链接。  
![QQ截图20210130164055](https://cn.logseq.com/uploads/default/original/1X/2a4dbc368e960af4b61abe11ada246c5a5638746.png)

不做学术，看过 `zotero` 觉得对我没用，原来还可以做文件管理用 ![:+1:](https://cn.logseq.com/images/emoji/twitter/+1.png?v=9 ":+1:")  
我一直用 `total commander`+`ulister插件`， 偶尔用 `taglyst` 来管理文件，用 `quicker` 结合 `listary` 来将文件和笔记关联。  
现在试试 `zotero`，看来一个软件就可以关联起来了。

软件默认的支持这么多种文件呢，不够的话还可以自定义条目类型，按需求设置条目属性。中间的表格展示也可以自定义展示哪一列内容。  

[![新建位图图像](https://cn.logseq.com/uploads/default/optimized/1X/3adab5414dfff8d2febd160bd29679c6c0130b33_2_10x10.png)](https://cn.logseq.com/uploads/default/original/1X/3adab5414dfff8d2febd160bd29679c6c0130b33.png "新建位图图像")

不过需要注意的是直接把文件拖进zotero里默认是复制一份到zotero储存位置，看你需求是用zotero储存文件还是只是链接到原始文件而已，设置里可以修改。

谢谢！最近客户端版本实现了对任意文件的链接，我的需求就是这样，非常感谢你的回复，也感谢天生团队的开发。

同样的方法，我是用 Devonthink 实现的。

事实上你可以通过markdown 的链接格式文本打开任意的文件夹和文件。  
比如 \[The name of link\](File:\\\\D:/dir of files)

logseq不支持 ![:joy:](https://cn.logseq.com/images/emoji/twitter/joy.png?v=9 ":joy:")  
logseq只能打开库的asset文件夹里的附件。  
而且我本身就在使用zotero管理文献，所以使用起来还是挺方便的。
