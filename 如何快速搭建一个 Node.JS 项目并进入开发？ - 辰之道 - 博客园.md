# [如何快速搭建一个 Node.JS 项目并进入开发？ - 辰之道 - 博客园](https://www.cnblogs.com/chongsaid/p/nodejs_getStart.html)

在此不概述 Node.JS 的历史以及发展过程。

因为之前接触过通过 Java 开发语言，所以明确地知道一个服务器所需的文件，以及一个服务器所需要的操作。

那么，我们细分一下，所有的服务器都至少需要什么呢？

-   静态文件访问
-   路由分发
-   数据库连接

这三者是最重要的服务器基础功能：

　　静态文件是类似如图片、CSS、JS、HTML等前端需要的界面资源

　　路由分发则是当浏览器 OR 客户端访问某个URL地址时，服务器会自行解析并分发给某段处理代码中。

　　而数据库连接则是将数据保存至磁盘（即数据库）而不至于关闭服务器便消失了（静态服务器可忽略数据库）

一个服务器最要紧的是让浏览器访问到相关 URL 时，就会自行解析 URL ，之后分发给相关处理程序进行处理，即路由分发功能。

所以第一步，让我们了解一个 Node.JS 项目的基础文件并知道怎样进行创建。

## 1.1 引入 npm 概念

npm 是一个包管理器，它可以对项目的依赖文件进行加载，卸载等操作，同时可以创建一个项目的配置文件（package.json）（目前我初学了解的）  
同样的，一个项目需要怎样的依赖环境，都需要 npm 来配置

## 1.2 创建一个 Node.JS 项目

打开文件管理器，在你想要的索引位置上**新建一个文件夹，文件夹的名称即是项目名**

打开 DOS / 终端 ，执行 npm 进行项目的创建（注意，在项目的当前文件夹）

将会在项目目录下创建一个 package.json 文件

![](https://img2018.cnblogs.com/i-beta/1140908/202002/1140908-20200210233425301-44740281.png)

箭头所指的即是创建过程中填写的配置信息，比方：author，指的是开发者名称，而 main，指的是初始化时指定的主文件入口

至于其它，我相信网上有一个更全面的 配置文件属性介绍文章。

创建好 package.json 后，需再创建项目必备文件夹以及文件（有些方法可以一键生成，这里不作推荐）

## 1.3 创建项目必备文件夹以及文件

![](https://img2018.cnblogs.com/i-beta/1140908/202002/1140908-20200211125944342-706759673.png)

注意：package-lock.json 不必手动创建，以及 node\_modules 文件夹亦无需创建。

仅需创建：bin、public、routes、views 文件夹，同时创建一个名为 app.js ，它将作为主函数入口。

**【重要】**请在 package.json 中手动将其中的 main 属性配置为 app.js （ 自动生成的默认为 index.js ）

## 1.4 安装 Express 框架

引言：

 _express 框架是 node.js 官方唯一推荐的框架（当前，从一些书上了解的）_

 _所以我觉得，它应该可以作为入门框架，同时可以开发一些小项目。_

安装：在当前的项目根目录中打开 DOS / 终端，之后执行以下命令：

npm install express -save

**关于 npm 安装参数中的 -save 或是 -dev 的说明：**

当项目中依赖某个模块时，或者项目中使用的某个模块依赖另外一个模块时，正常情况下需要安装它们。

一般而言，模块依赖什么模块，便会在其 package.json 中的 dependencies 写上需要什么依赖模块。

比如 express 框架的 package.json ：

![](https://img2018.cnblogs.com/i-beta/1140908/202002/1140908-20200211131347289-1231037833.png)

我们再来看看它其中的的内容，没有全部复制，仅复制其中的 dependencies 属性

![复制代码](https://common.cnblogs.com/images/copycode.gif)

"dependencies": { "accepts": "~1.3.7", "array-flatten": "1.1.1", "body-parser": "1.19.0", "content-disposition": "0.5.3", "content-type": "~1.0.4", "cookie": "0.4.0", "cookie-signature": "1.0.6", "debug": "2.6.9", "depd": "~1.1.2", "encodeurl": "~1.0.2", "escape-html": "~1.0.3", "etag": "~1.8.1", "finalhandler": "~1.1.2", "fresh": "0.5.2", "merge-descriptors": "1.0.1", "methods": "~1.1.2", "on-finished": "~2.3.0", "parseurl": "~1.3.3", "path-to-regexp": "0.1.7", "proxy-addr": "~2.0.5", "qs": "6.7.0", "range-parser": "~1.2.1", "safe-buffer": "5.1.2", "send": "0.17.1", "serve-static": "1.14.1", "setprototypeof": "1.1.1", "statuses": "~1.5.0", "type-is": "~1.6.18", "utils-merge": "1.0.1", "vary": "~1.1.2" }

![复制代码](https://common.cnblogs.com/images/copycode.gif)

这样一来，需要什么依赖便知道得一清二楚了。

但是我们的项目，总不能说模块依赖什么时，我们手动加上，如果有成千上百个依赖模块呢

所以，引入了 npm -save 的概念，也就是当你为项目安装什么模块时，它会把这个模块的名称，版本写入你的项目依赖中

\-save 和 -save -dev 省掉了手写或修改 package.json 的步骤，各自功能如下：

\-save ：自动将模块和版本号添加到 dependencies 部分

\-save -dev ：自动将模块和版本号添加到 devdependencies 部分

配置文件中的这两个属性，具体区别可以在网络中搜索一下，相信会有更好的解释，这里不作说明。

## 1.5 编写 app.js 文件

![复制代码](https://common.cnblogs.com/images/copycode.gif)

// 引入 express 框架 -> 需 npm 安装
var express = require('express'); /\*\*
 \* 初始化框架,并将初始化后的函数给予 '当前页面'全局变量 app
 \* 也就是说, app 是 express \*/
var app = express(); /\* 配置框架环境 S \*/

// 设置 public 为静态文件的存放文件夹
app.use('/public', express.static('public')); /\* 配置框架环境 E \*/ app.get('/', function(req, res) {
    res.send('Hello World');
}) var server = app.listen(8081, function() { var host = server.address().address var port = server.address().port
    
    console.log("Node.JS 服务器已启动，访问地址： http://%s:%s", host, port)

})

![复制代码](https://common.cnblogs.com/images/copycode.gif)

注意：部分网络中，访问地址会出现  http://:::8081 的情况，在浏览器中输入 localhost:8081 便可以访问了。

因为 Node.JS 启用了 IPv6 的缘故，并不影响网址的访问。

## 1.6 启动服务器

在项目根目录中打开 DOS / 终端，执行 node app.js 

![](https://img2018.cnblogs.com/i-beta/1140908/202002/1140908-20200211133229668-1859284365.png)

终端出现的错误请忽略，因为出于习惯我总是拼写成 note，现在已经不会犯同样的错误了。

到这里，你也可以访问到 node.js 服务器了。

至于框架类的学习，后续再发一篇文章，顺便，我也需要练手一些项目了。

2020-02-11 写于揭阳

转载请附上本博客地址：[https://www.cnblogs.com/chongsaid/](https://www.cnblogs.com/chongsaid/) 或当前文章链接。
