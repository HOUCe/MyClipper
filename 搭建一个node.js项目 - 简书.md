# [搭建一个node.js项目 - 简书](https://www.jianshu.com/p/a6d430a00242)

[![](https://cdn2.jianshu.io/assets/default_avatar/3-9a2bcc21a5d89e21dafc73b39dc5f582.jpg)](https://www.jianshu.com/u/7e8ad7e73ecf)

0.0362017.03.31 11:16:46字数 217阅读 17,254

## 初始化项目

新建一个文件夹，运行 `npm init` 初始化项目

```
mkdir okadaGo
cd okadaGo
npm init
```

按照提示输入一些项目的相关信息

```
D:\web\node>mkdir okadaGo

D:\web\node>cd okadaGo

D:\web\node\okadaGo>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (okadaGo)
Sorry, name can no longer contain capital letters.
name: (okadaGo) okada_go
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to D:\web\node\okadaGo\package.json:

{
  "name": "okada_go",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes)

D:\web\node\okadaGo>
```

### 目录结构

建立三个文件夹：public、routes 和 views。

项目的文件结构如下

```
├─models/
├─public/
├─routes/
├─views/
├─index.js
└─package.json
```

对应文件及文件夹的用处：

-   `models` 存放操作数据库的文件
-   `public` 存放静态文件，如 css、图片等
-   `routes` 存放路由文件
-   `views` 存放模板文件
-   `index.js` 程序主文件
-   `package.json` 存储项目的信息，比如项目名、描述、作者、依赖等

## 安装依赖

安装 express 框架

```
npm install express --save
```

## 启动项目

进入项目的根目录，建立一个 `index.js` 文件，并加入如下代码

```
var express = require('express');
var app = express();

app.get('/', function(res, rep) {
    rep.send('Hello, word!');
});

app.listen(3000);
```

然后在控制台中输入

接着使用浏览器访问 [http://localhost:3000/](https://link.jianshu.com/?t=http://localhost:3000/)，就可以看到效果了

![](https://upload-images.jianshu.io/upload_images/3617116-0bc9dc1e0875eba8.png)

[![  ](https://cdn2.jianshu.io/assets/default_avatar/3-9a2bcc21a5d89e21dafc73b39dc5f582.jpg)](https://www.jianshu.com/u/7e8ad7e73ecf)

[红烧排骨饭](https://www.jianshu.com/u/7e8ad7e73ecf "红烧排骨饭")快乐获得的越容易，空虚也来得越快

总资产3共写了3.3W字获得124个赞共35个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

-   搭建开发环境并模拟交互数据 一、实验说明 下述介绍为实验楼默认环境，如果您使用的是定制环境，请修改成您自己的环境介...
    
-   本章主要讲什么（一句话）？ 《项目实战：基于Angular2+Mongodb+Node技术实现的多用户博客系统教程...
    
    [![](https://upload-images.jianshu.io/upload_images/4504891-ed0d0a38eb827723.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/71458a7e3609)

-   Spring Cloud为开发人员提供了快速构建分布式系统中一些常见模式的工具（例如配置管理，服务发现，断路器，智...
    
    [![](https://upload-images.jianshu.io/upload_images/7328262-54f7992145380c10.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/46fd0faecac1)
-   Express是Node社区里的超级明星，他的作者TJ Holowaychuk也因此成为了社区里大红大紫的开发者。...
    
    [![](https://upload-images.jianshu.io/upload_images/2005304-9e8b8bf2ab01c05e.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/666fb7479c3d)
-   在商业传播无孔不入的今天，如何做好一次成功的商业传播，让其有着持续的生命力和价值，最终让企业获得发展，树立高端的品...
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/13-394c31a9cb492fcb39c27422ca7d2815.jpg)松哥生涯](https://www.jianshu.com/u/dfc52c107dda)阅读 361评论 1赞 5
    
    [![](https://upload-images.jianshu.io/upload_images/4918193-7c7203770473cc95.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/908cb071c115)
-   以财相交，财尽而交绝 以色相交，华落而爱渝 以利相交，利尽而交疏 以势相交，势倾而交绝 以道相交，天荒而地老。
    
    [![](https://cdn2.jianshu.io/assets/default_avatar/5-33d2da32c552b8be9a0548c7a4576607.jpg)标新](https://www.jianshu.com/u/3d782713d8e8)阅读 170评论 0赞 1
    

-   经常有怀孕的准妈妈来来问专家，怀疑自己得了孕期焦虑症，要怎么办？甚至有准妈妈担心自己得了产后抑郁症，苦恼如何摆脱孕...
    
-   别轻易说迷茫的追求便是愚蠢的错 选择把背影留给世界本是就是折磨 牵牛花绽出绝美妖艳的骨朵 却总不如半残衰夏的雨打残...
    
    [![](https://upload-images.jianshu.io/upload_images/7138170-c3b15b48b6199a64.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/png)](https://www.jianshu.com/p/be747c7a2b7c)
