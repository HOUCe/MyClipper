# [如何从零开始写一个 Chrome 扩展？ - 知乎](https://www.zhihu.com/question/20179805)

看了好多回答，基本上都是推荐的教程，然而 Google 的官方教程得翻墙才能看，360 的文档又太老，刚好最近兴趣来了想学习写 Chrome 扩展，就顺便写了个入门教程，有不对或者不完善的地方欢迎指正。

**一个 Chrome 扩展其实就是一个配置（入口）文件 manifest.json 和一系列 html、css、js、图片文件的集合**，所以只要有前端基础，写一个简单的 Chrome 扩展是分分钟的事情。

由于 html、css、js 及文件组织，跟普通的 web 开发一样，那写 Chrome 扩展主要就是 manifest 文件的编写和扩展的调试了，下面以一个

[股票信息查看](https://github.com/huangtengfei/stock)

扩展和一个

[天气预报](https://github.com/huangtengfei/weather)

扩展举例。

一、manifest 文件的编写

1\. 基本属性

包括扩展的名称（name）、版本（version）、描述（description）、图标位置（icons）和 manifest 版本（manifest\_version）等信息。

其中，name、version 和 manifest\_version 是必须的，而且 manifest\_version 必须为 2

栗子：

```
{
    ...
    "manifest_version": 2,
    "name": "Weather",
    "description": "a currently clone",
    "version": "0.1",
    ...
}
```

2\. browser\_action

browser\_action 指定扩展的图标放在 Chrome 工具栏中，它定义了扩展图标文件位置（default\_icon）、悬浮提示（default\_title）和点击扩展图标所显示的页面位置（default\_popup）。

栗子：

```
{
    ...
    "browser_action": {
        "default_icon": {
            "19": "images/icon19.png",
            "38": "images/icon38.png"
        },
        "default_title": "stock helper",
        "default_popup": "popup.html"
    },
    ...
}
```

比如一个查看股票信息的扩展，点开图标后是这样的效果：

![](https://pic1.zhimg.com/50/815368755b9f4572aad0bb8be0598881_720w.jpg?source=1940ef5c)

3\. options\_page

options\_page 属性定义了扩展的设置页面，配置后在扩展图标点击右键可以看到 _选项_，点击即打开指定页面。对于没有图标（没有设置 browser\_action ）的扩展，可以在 chrome://extensions/ 页面找到选项按钮。

栗子：

```
{
    ...
    "options_page": "options.html",
    ...
}
```

4\. permissions

permissions 属性是一个数组，它定义了扩展需要向 Chrome 申请的权限，比如通过 XMLHttpRequest 跨域请求数据、访问浏览器选项卡（tabs）、获取当前活动选项卡（activeTab）、浏览器通知（notifications）、存储（storage）等，可以根据需要添加。

栗子：

```
{
    ...
    "permissions": [
        "http://api.wunderground.com/api/"
        "tabs",
        "activeTab",
        "notifications",
        "storage"
    ],
    ...
}
```

5\. background

background 可以使扩展常驻后台，比较常用的是指定子属性 scripts，表示在扩展启动时自动创建一个包含所有指定脚本的页面。

栗子：

```
{
    ...
    "background": {
        "scripts": ["js/background.js"]
    },
    ...
}
```

6\. chrome\_url\_overrides

chrome\_url\_overrides 属性可以自定义的页面替换 Chrome 相应默认的页面，比如新标签页（newtab）、书签页面（bookmarks）和历史记录（history）。

栗子：

```
{
    ...
    "chrome_url_overrides": {
        "newtab": "tab.html"
    },
    ...
}
```

结合前面的 background 属性，可以做一个打开新标签页，就能看到天气和当前时间的扩展，如下图：

![](https://pic1.zhimg.com/50/244e211570ef3b088de32fbc1309f624_720w.jpg?source=1940ef5c)

二、扩展的调试

打开 Chrome _设置_\-_扩展程序_（chrome://extensions/）页面，勾选 _开发者模式_，点击 _加载正在开发的扩展程序_ 按钮，选择扩展所在的文件夹，就可以在浏览器工具栏中看到自己写的扩展了。

![](https://pic1.zhimg.com/50/bf8ce4d0168bebe1be0639af8ffcfebb_720w.jpg?source=1940ef5c)

如果是带图标的扩展，右键点击图标，点击审查弹出内容，在相应位置加断点，然后在控制台上，输入以下命令：

重新加载这个页面，就可以调试了。

如果是覆盖新标签页的应用，直接右键审查元素，加断点，刷新即可。

大致就这些了，想写复杂的 Chrome 扩展，再去自行深入了解吧~~
