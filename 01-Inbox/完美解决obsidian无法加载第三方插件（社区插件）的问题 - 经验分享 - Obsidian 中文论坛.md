# [完美解决obsidian无法加载第三方插件（社区插件）的问题 - 经验分享 - Obsidian 中文论坛](https://forum-zh.obsidian.md/t/topic/1627)

之前做了一个补丁来加载第三方插件。具体介绍在后面。但总觉得不爽，而且后面obsidian升级之后，可能还需要调整。为了更好的解决当前问题。我又做了一个插件。

## [](https://forum-zh.obsidian.md/t/topic/1627#heading-1)

下载

插件的github地址是：

在gitee上我也传了一份：

## [](https://forum-zh.obsidian.md/t/topic/1627#heading-2)

使用方法

首先它是一个插件，所以不需要对obsidian进行修改，但是，需要在每个库的【.obsidian/plugins】目录下都要放一份这个插件。

1.  下载 obsidian-proxy-github.zip
    
2.  解压 obsidian-proxy-github.zip
    
3.  将解压的文件夹放入笔记目录下的插件目录内。如：XXX/.obsidian/plugins
    
4.  如果是第一次使用这个插件，需要执行 【XXX/.obsidian/plugins/obsidian-proxy-github/ObsidianLink.bat】在桌面创建一个快捷方式。作用是取消安全限制，不然会因为跨域问题无法下载插件。
    
5.  此后，如果需要安装插件时，需要运行桌面上新创建的 Obsidian 快捷方式
    

[![image](https://forum-zh.obsidian.md/uploads/default/original/2X/5/5b3cd26ef4b36bbe2f659e9e2a35d42f82f984e8.png)](https://forum-zh.obsidian.md/uploads/default/original/2X/5/5b3cd26ef4b36bbe2f659e9e2a35d42f82f984e8.png "image")

在需要的时候，把ProxyGithub的配置打开，不需要的时候关闭就可以了。

-   #### 创建
    
    21 年 11 月
    
-   [
    
    #### 最后回复
    
    ](https://forum-zh.obsidian.md/t/topic/1627/5)
    
    [](https://forum-zh.obsidian.md/t/topic/1627/5)21 年 11 月
    
-   393
    
    #### 浏览量
    
-   3
    
    #### 用户
    
-   2
    
    #### 赞
    
-   4
    
    #### 链接
    
-   ![](https://forum-zh.obsidian.md/user_avatar/forum-zh.obsidian.md/awyugan/64/1240_2.png "awyugan")
    
    ![](https://forum-zh.obsidian.md/letter_avatar_proxy/v4/letter/l/e480ec/64.png "Leo Wang")
    

由于现在论坛只能发两个链接，一个图片，所以很不方便

这个说明将主要维护在知乎上，可以访问：

第一次运行ObsidianLink.bat，出现错误

[![微信截图_20211113215601](https://forum-zh.obsidian.md/uploads/default/optimized/2X/7/7cf132f4f5526bb50da77db0ad674742cf3e765a_2_690x357.png)](https://forum-zh.obsidian.md/uploads/default/original/2X/7/7cf132f4f5526bb50da77db0ad674742cf3e765a.png "微信截图_20211113215601")
