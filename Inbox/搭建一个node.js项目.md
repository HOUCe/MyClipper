# 搭建一个node.js项目

[www.jianshu.com](https://www.jianshu.com/p/a6d430a00242)

## 初始化项目

新建一个文件夹，运行 `npm init` 初始化项目

    mkdir okadaGo
    cd okadaGo
    npm init
    

按照提示输入一些项目的相关信息

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
    

### 目录结构

建立三个文件夹：public、routes 和 views。

项目的文件结构如下

    ├─models/
    ├─public/
    ├─routes/
    ├─views/
    ├─index.js
    └─package.json
    

对应文件及文件夹的用处：

*   `models` 存放操作数据库的文件
*   `public` 存放静态文件，如 css、图片等
*   `routes` 存放路由文件
*   `views` 存放模板文件
*   `index.js` 程序主文件
*   `package.json` 存储项目的信息，比如项目名、描述、作者、依赖等

## 安装依赖

安装 express 框架

    npm install express --save
    

## 启动项目

进入项目的根目录，建立一个 `index.js` 文件，并加入如下代码

    var express = require('express');
    var app = express();
    
    app.get('/', function(res, rep) {
        rep.send('Hello, word!');
    });
    
    app.listen(3000);
    

然后在控制台中输入

    node index.js
    

接着使用浏览器访问 [http://localhost:3000/](https://link.jianshu.com/?t=http://localhost:3000/)，就可以看到效果了

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fupload-images.jianshu.io%2Fupload_images%2F3617116-0bc9dc1e0875eba8.png%3FimageMogr2%2Fauto-orient%2Fstrip%7CimageView2%2F2%2Fw%2F523%2Fformat%2Fpng)

[查看原网页: www.jianshu.com](https://www.jianshu.com/p/a6d430a00242)