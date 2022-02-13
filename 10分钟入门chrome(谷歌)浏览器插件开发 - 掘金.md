# [10分钟入门chrome(谷歌)浏览器插件开发 - 掘金](https://juejin.cn/post/6904797929056239630)

> 整理chrome插件有哪些能力，插件开发入门，整理文档。

chrome谷歌浏览器插件开发，听上去很高大上，其实只要熟悉`HTML、CSS、JS`即可开发，插件也是将html页面渲染出来并执行js脚本而已。

## 插件能做哪些事？

-   书签控制；
-   下载控制；
-   窗口控制；
-   标签控制；
-   网络请求控制，
-   各类事件监听；
-   自定义原生菜单；
-   完善的通信机制；

## 简介

扩展程序主要名词

-   Manifest （清单文件）
-   Background Script （后台脚本）
-   UI Elements （页面元素）
-   Content Script （内容脚本）
-   Options Page（配置页面）

## 开发入门

### 1\. 新建一个文件夹，目录结构如下：

```
chrome-plugin-demo
├── background.js
├── images
│   ├── 128.png
│   ├── 16.png
│   ├── 32.png
│   └── 48.png
├── manifest.json
├── popup.html
└── popup.js
复制代码
```

### 2\. 创建 `manifest.json` 配置文件

```
{
    "name": "chrome-plugin-demo",
    "version": "1.0",
    "description": "Build an Extension!",
    "manifest_version": 2 
}
复制代码
```

### 3\. 创建 `popup.html` 文件

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #content{color: red;}
    </style>
</head>
<body>
    <h1>chrome-plugin-test</h1>
    <p id="content"></p>
    <script src="popup.js"></script>
</body>
</html>
复制代码
```

### 4\. 创建 `popup.js` 文件

```
document.getElementById('content').innerText = 'Hello world!';
复制代码
```

### 5\. 在 chrome 中安装扩展

-   谷歌浏览器右上角: 更多按钮 -> 更多工具 -> 扩展程序
-   点击`加载已解压的扩展程序`，选择刚创建的文件夹
-   点开谷歌浏览器右上角的拼图图标即可看到你的插件。

## 好用的插件推荐

### 当前浏览的网页链接变成二维码

想在手机上看电脑访问的链接，不需要手动敲或者复制链接通过QQ等工具转发到手机上，仅需这个插件就可以扫二维码访问。

-   [chrome 网上应用店](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2F%25E4%25BA%258C%25E7%25BB%25B4%25E7%25A0%2581qr%25E7%25A0%2581%25E7%2594%259F%25E6%2588%2590%25E5%2599%25A8qr-code-generato%2Fpflgjjogbmmcmfhfcnlohagkablhbpmg%3Fhl%3Dzh-CN "https://chrome.google.com/webstore/detail/%E4%BA%8C%E7%BB%B4%E7%A0%81qr%E7%A0%81%E7%94%9F%E6%88%90%E5%99%A8qr-code-generato/pflgjjogbmmcmfhfcnlohagkablhbpmg?hl=zh-CN")

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3c3e2faa828041c2aee0d4c2e8827cc6~tplv-k3u1fbpfcp-watermark.awebp)

### ColorZilla（网页颜色吸取器）

> 可吸取网页的字体、背景、元素等的颜色值

-   [chrome 网上应用店](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fcolorzilla%2Fbhlhnicpbhignbdhedgjhgdocnmhomnp "https://chrome.google.com/webstore/detail/colorzilla/bhlhnicpbhignbdhedgjhgdocnmhomnp")

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cea143e9fbf84f95b30f3bf61911d79e~tplv-k3u1fbpfcp-watermark.awebp)

### Markdown Preview Plus（可视化markdown）

> 可选主题，支持自定义css主题

-   [chrome 网上应用店](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fmarkdown-preview-plus%2Ffebilkbfcbhebfnokafefeacimjdckgl "https://chrome.google.com/webstore/detail/markdown-preview-plus/febilkbfcbhebfnokafefeacimjdckgl")
-   [使用介绍](https://link.juejin.cn/?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2F7a088d19bf9c "https://www.jianshu.com/p/7a088d19bf9c")

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/14af07fa540b42e5968b0909e125c80a~tplv-k3u1fbpfcp-watermark.awebp)

### json-viewer（可视化json）

> 可选主题，支持自定义css主题，可收缩展开，可编辑

-   [chrome 网上应用店](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fjson-viewer%2Fgbmdgpbipfallnflgajpaliibnhdgobh "https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh")
-   [github](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Ftulios%2Fjson-viewer "https://github.com/tulios/json-viewer")

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/417bcfff588845f68056ad328858d554~tplv-k3u1fbpfcp-watermark.awebp)

### JavaScript and CSS Code Beautifier（可视化js、css）

> 可选主题，支持自定义css主题

-   [chrome 网上应用店](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fjavascript-and-css-code-b%2Fiiglodndmmefofehaibmaignglbpdald "https://chrome.google.com/webstore/detail/javascript-and-css-code-b/iiglodndmmefofehaibmaignglbpdald")

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9cb2735a6204ad6ba2fa7406e00fb1f~tplv-k3u1fbpfcp-watermark.awebp)

## 更多请参考

-   [chrome网上应用店官网](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fcategory%2Fextensions%3Fhl%3Dzh-CN "https://chrome.google.com/webstore/category/extensions?hl=zh-CN")
-   [入门：建立 Chrome 扩展程序](https://link.juejin.cn/?target=https%3A%2F%2Fcrxdoc-zh.appspot.com%2Fextensions%2Fgetstarted "https://crxdoc-zh.appspot.com/extensions/getstarted")
-   [chrome扩展 - 开发者指南](https://link.juejin.cn/?target=https%3A%2F%2Fcrxdoc-zh.appspot.com%2Fextensions%2Fdevguide "https://crxdoc-zh.appspot.com/extensions/devguide")
-   [chrome扩展 - JavaScript API](https://link.juejin.cn/?target=https%3A%2F%2Fcrxdoc-zh.appspot.com%2Fextensions%2Fapi_index "https://crxdoc-zh.appspot.com/extensions/api_index")
-   [chrome扩展 - 开发文档 - gitbook](https://link.juejin.cn/?target=https%3A%2F%2Fwizardforcel.gitbooks.io%2Fchrome-doc%2Fcontent%2F1.html "https://wizardforcel.gitbooks.io/chrome-doc/content/1.html")
-   [手把手教你开发一个 chrome 扩展程序](https://juejin.cn/post/6844904077889912839 "https://juejin.cn/post/6844904077889912839")
