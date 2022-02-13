# [macOS] ClashX 使用教程 - 知识库 - xipCloud

source:: https://xipcloud.net/index.php?rp=/knowledgebase/3/macOS-ClashX-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.html

![](https://xipcloud.net/downloads/images/Clash-logo.png)_Clash 是一款基于 Go 语言的开源隧道客户端，支持众多协议，以及全平台统一配置文件_

，非常方便跨平台使用。

以下为 ClashX 具体使用方式，适用于 macOS 系统：

## 1\. 获取节点信息

请根据下图指引，进入产品明细：

在会员中心的菜单栏，点击服务 > 我的服务，并在新页面中点击箭头所指向的具体产品名称，如下图为 Micro 套餐：

![](https://xipcloud.net/downloads/images/step1.jpg)

进入产品详情之后，即可看到具体的服务器信息，包括使用期、价格、订阅、节点、流量等信息。

在订阅的部分，找到 Clash / Clash X 一行，点击最右侧的复制按钮，如下图：

![](https://xipcloud.net/downloads/images/step2.jpg)

**请保留此地址，后面会用到。**

另外，此地址包含账号、密码信息，请勿泄露。

## 2\. 获取 ClashX 客户端

官网地址：[https://github.com/yichengchen/clashX/releases](https://github.com/yichengchen/clashX/releases)

由于更新频繁，请尽可能通过上述链接下载最新客户端，如有困难，请发送工单联系客服解决。

## 3\. 安装客户端

与一般的 macOS 程序相同，只需要双击下载回来的 ClashX.dmg 文件，拖动到 Applications（应用程序）文件夹里即可：

![](https://xipcloud.net/downloads/images/ClashX1.jpg)

## 3\. 配置客户端

这里以最简洁方式配置 ClashX，请继续向下阅读：

### 3.1 启动 ClashX

通过**启动台**启动 ClashX：

![](https://xipcloud.net/downloads/images/ClashX2.jpg)

**首次启动，**需要管理员权限安装一个帮助程序，只需要根据提示，依次点击**安装** > 输入 macOS 登录密码 > **安装帮助程序** 即可。

![](https://xipcloud.net/downloads/images/ClashX3.1.jpg)

之后，可以在 macOS 的菜单栏找到 ClashX。

### 3.2 配置 ClashX

在 macOS 顶部右上角的菜单栏中，找到 ClashX，根据下图，进入：**配置 > 托管配置 > 管理**：

![](https://xipcloud.net/downloads/images/ClashX5.jpg)

在新的托管配置管理界面中，点击 **添加**，Url 位置输入之前节点信息部分复制的地址，Config Name 可随意，最后点击确定。

如下图：

![](https://xipcloud.net/downloads/images/ClashX6.jpg)

![](https://xipcloud.net/downloads/images/ClashX7.jpg)

关闭此窗口，全部设置已完成。

## 4\. 使用

在菜单栏点击 ClashX 图标，确保出站模式为**规则，配置**菜单中刚刚设置的名称前打勾。

然后勾选：

-   设置为系统代理
-   开机启动（可选）
-   允许局域网连接（可选）

![](https://xipcloud.net/downloads/images/ClashX8.jpg)

### 5\. 设置已完成

请打开浏览器使用。
