# 从Workflowy到印象笔记

[juejin.cn](https://juejin.cn/post/6844903576989335565)青南 2018年03月17日 14:27 · 阅读 2518

Workflowy是一个极简风格的大纲写作工具，使用它提供的无限层级缩进和各种快捷键，可以非常方便的理清思路，写出一个好看而实用的大纲。如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a3990da4255%7Etplv-t2oaga2asx-watermark.awebp)

印象笔记更是家喻户晓，无人不知的跨平台笔记应用。虽然有很多竞争产品在和印象笔记争抢市场，但是印象笔记强大的搜索功能还是牢牢抓住了不少用户。

如果能够把用Workflowy写大纲的便利性，与印象笔记强大的搜索功能结合起来，那岂不是如虎添翼？如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a3990b56601%7Etplv-t2oaga2asx-watermark.awebp)

EverFlowy就是这样一个小工具。它可以自动把Workflowy上面的条目拉下来再同步到印象笔记中。如果Workflowy有更新，再运行一下这个小工具，它就会同步更新印象笔记上面的内容。Workflowy负责写，印象笔记负责存，各尽其能，各得其所。

## 工具介绍

Everflowy基于Python 3开发，代码托管在Github中，地址为：[github.com/kingname/Ev…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fkingname%2FEverFlowy "https://github.com/kingname/EverFlowy")这个小工具在持续开发中，目前可以实现Workflowy单向同步到印象笔记和差异更新。由于印象笔记的Oauth验证方式需要申请才能对正式的账号使用，但它又不会通过这种个人小工具的申请，所以目前暂时使用开发者Token。关于如何申请开通正式账号的开发者Token，在后文会有详细的说明。

## 安装

首先需要保证电脑中安装了Python 3，否则无法运行这个小工具。代码的依赖关系使用Pipenv来管理，所以需要首先使用pip安装pipenv：

    python3 -m pip install pipenv
    复制代码

有了Pipenv以后，使用Git把代码拉到本地再安装依赖：

    git clone https://github.com/kingname/EverFlowy.git
    cd EverFlowy
    pipenv install
    pipenv shell
    复制代码

运行了上面的4条命令以后，你的终端窗口应该如下图类似。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a399128e268%7Etplv-t2oaga2asx-watermark.awebp)

Pipenv会自动创建一个基于Virtualenv的虚拟环境，然后把EverFlowy依赖的第三方库自动安装到这个虚拟环境中，再自动激活这个虚拟环境。

## 配置

在代码的根目录，有一个config.json文件，打开以后如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a399159c1db%7Etplv-t2oaga2asx-watermark.awebp)

你需要修改三个地方，分别是`username`，`password`和`dev_token`。其中`username`和`password`分别对应了Workflowy的用户名和密码，而`dev_token`是印象笔记的开发者Token。

这里需要说明一下印象笔记的开发者Token。印象笔记的开发者Token有两套，分别是沙盒环境的开发者Token和生产环境的开发者Token。所谓沙盒环境，就是一个测试开发环境，这个环境是专门为了快速开发印象笔记App而设计的，它的地址为：[sandbox.evernote.com](https://link.juejin.cn/?target=https%3A%2F%2Fsandbox.evernote.com "https://sandbox.evernote.com")。打开这个网址，可以看到页面上弹出了警告，如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a39917f2356%7Etplv-t2oaga2asx-watermark.awebp)

无论你之前是否有印象笔记的账号，要使用沙盒环境，都必需重新注册。注册完成以后，通过访问[sandbox.evernote.com/api/Develop…](https://link.juejin.cn/?target=https%3A%2F%2Fsandbox.evernote.com%2Fapi%2FDeveloperToken.action "https://sandbox.evernote.com/api/DeveloperToken.action")获取沙盒环境的开发者Token。

关于印象笔记的沙盒环境，我将另外开一篇文章来说明。本文主要介绍如何申请生产环境的开发者Token，从而可以使用正式的印象笔记账号。

在2017年6月以后，印象笔记关闭了生产环境开发者Token的申请通道，如果打开申请网址：[app.yinxiang.com/api/Develop…](https://link.juejin.cn/?target=https%3A%2F%2Fapp.yinxiang.com%2Fapi%2FDeveloperToken.action "https://app.yinxiang.com/api/DeveloperToken.action")，你会发现申请的按钮是灰色的且无法点击。要解决这个问题，就需要让印象笔记的客服帮忙。

登录自己的印象笔记正式账号，打开印象笔记首页，把页面拉到最下面，可以看到有一个“联系我们”，如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a39918fed6c%7Etplv-t2oaga2asx-watermark.awebp)

进入“联系我们”，点击“联系客服”，如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a39b106de2c%7Etplv-t2oaga2asx-watermark.awebp)

在联系客服的页面填写如下信息，最后一项“简要描述问题”填写“我需要基于印象笔记API开发，请帮我开通生产环境开发者Token”并提交。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a39b6841c5e%7Etplv-t2oaga2asx-watermark.awebp)

大约24小时内，就可以受到客服回复的邮件，如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a39b7945b6b%7Etplv-t2oaga2asx-watermark.awebp)

此时再次打开[app.yinxiang.com/api/Develop…](https://link.juejin.cn/?target=https%3A%2F%2Fapp.yinxiang.com%2Fapi%2FDeveloperToken.action "https://app.yinxiang.com/api/DeveloperToken.action")就可以申请开发者Token了，如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F3%2F17%2F16232a39b7ebf927%7Etplv-t2oaga2asx-watermark.awebp)

需要注意的是，开发者Token只会显示一次，所以你需要立刻把它记录下来。

## 运行

有了生产环境的开发者Token以后，把它填写到config.json中，配置就算完成了。在终端输入命令：

    python3 EverFlowy.py
    复制代码

程序就可以开始同步Workflowy的数据到印象笔记了。

同步完成以后，你会发现程序的根目录出现了一个history.db文件。这是一个sqllite的文件，里面就是你在Workflowy中的所有大纲内容和对应的印象笔记GUID和enml格式的内容。这是为了实现数据的差异更新而生成的。你可以使用各种能够浏览sqllite的工具来查看里面的内容。

## 已知问题

*   如果删除了history.db，那么再次运行Everflowy，Workflowy中的所有内容都会再次写入印象笔记。
*   如果单独删除了EverFlowy写入印象笔记中的某一条目，却不删除history.db中的对应条目，WorkFlowy会因为找不到GUID而抛出异常。
*   没有测试国际版印象笔记账号是否可用。
*   如过你想测试沙盒环境的开发者账号，请修改`evernote_util/EverNoteUtil.py`第98行，把

    client = EvernoteClient(token=self.dev_token, sandbox=False, service_host='app.yinxiang.com')
    复制代码

修改为：

    client = EvernoteClient(token=self.dev_token)
    复制代码

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fp1-jj.byteimg.com%2Ftos-cn-i-t2oaga2asx%2Fgold-user-assets%2F2018%2F8%2F30%2F165894542d3adab3%7Etplv-t2oaga2asx-watermark.awebp)

我的公众号：未闻Code

[查看原网页: juejin.cn](https://juejin.cn/post/6844903576989335565)