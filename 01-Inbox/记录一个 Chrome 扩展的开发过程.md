# 记录一个 Chrome 扩展的开发过程

[learnku.com](https://learnku.com/articles/6513/record-the-development-process-of-a-chrome-extension)Summer

> [chrome-extension-smms](https://github.com/96qbhy/chrome-extension-smms) 是基于 [smms](https://github.com/96qbhy/smms) 实现的一个 `chrome` 图床扩展。-
> 其实很早之前就想学习 `Chrome` 扩展开发 了，只是一直没有抽出时间，一直瞎忙。今天刚好闲下来了而且刚好看到一些跟 `Chrome` 扩展开发相关的文章，于是就像动手写一个试试，于是就有了这篇文章。

## 什么是 Chrome 扩展 ？

首先，这里所说的 `Chrome扩展` ，并非 `Chrome插件`，两者是有区别的，想要弄明白这两个概念的可以看看知乎的这个问题 [Chrome 的插件（Plugin）与扩展（Extension）有什么区别？](https://www.zhihu.com/question/20628768)

*   扩展（Extension）: 指的是通过调用 `Chrome` 提供的 `Chrome API` 来扩展浏览器功能的一种组件，工作在浏览器层面，使用 `HTML` + `Javascript` 语言开发。比如著名的 `Adblock plus`。

## 如何开发一个 Chrome 扩展 ？#

既然知道了 `Chrome 扩展` 是用 `HTML` + `javascript` 开发的了，那么我们首先要做的就是要搭建一个前端项目出来。

> 本文讲述的是一个 `Chrome扩展` 的大致过程，不会出现具体的功能如何实现。

### 初始化项目#

由于我个人喜欢用 `react.js` 来构建前端应用， 这里我使用 `dva-cli` 来初始化项目，这个没有特殊要求，你可以随意使用自己喜欢的前端项目构建工具。

    npm i -g dva-cli
    dva new smms-extension
    cd smms-extension

### 编写扩展的相关业务实现#

在项目中编写业务的实现，例如 [smms 图床扩展](https://github.com/96qbhy/chrome-extension-smms) 的逻辑是上传图片到图床，然后展示列表，提供删除图片和预览图片的方法，然后实现他。

### 打包前端项目并在 Chrome 中预览#

`npm run build` 打包好应用后，添加 `manifest.json` 文件到打包好的资源的文件夹，我这里是 `dist` 文件夹。然后修改 `manifest.json` 文件:

    {
      "manifest_version": 2,
      "name": "SMMS图床",
      "description": "一个简单实用的图床扩展",
      "version": "1.",
      "icons": {
        "16": "qbhy.png",
        "48": "qbhy.png",
        "128": "qbhy.png"
      },
      "permissions": [
        "tabs"
      ],
      "background": {
        "persistent": false,
        "scripts": ["background.js"]
      },
      "browser_action": {
        "default_icon": "qbhy.png",
        "default_title": "SMMS图床",
        "default_popup": "index.html"
      }
    }

以上内容大概描述了该扩展的图标、默认打开方式、入口文件 、`title` 等属性，完整的 `manifest.json` 属性请移步 [360 浏览器扩展开发文档](http://open.chrome.360.cn/extension_dev/manifest.html) (其实就是翻译过后的 `Chrome` 扩展开发文档)。

> 注意 `index.html` 文件里面的资源引用问题，不能使用 `/index.js` 这种方式引用脚本，要用相对路径引用，例如直接写 `index.js`，css 同理。

然后用 `Chrome` 打开 `chrome://extensions` 页面，勾选开发者模式。-
然后点击 `加载已解压的扩展程序`-
[](https://i.loli.net/2017/10/26/59f190b027d36.png)

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f190b027d36.png)](https://i.loli.net/2017/10/26/59f190b027d36.png)

选择包含 `manifest.json` 的文件夹，我这里是 dist。

[

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f19171979db.png)



](https://i.loli.net/2017/10/26/59f19171979db.png)

然后右上角就多了一个图标，不过现在还不是我们设置的那个图标，因为现在还没打包成 `.crx` 后缀的扩展。

[

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f191e86d288.png)



](https://i.loli.net/2017/10/26/59f191e86d288.png)

然后点开就可以预览了

[

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f19273b320f.png)



](https://i.loli.net/2017/10/26/59f19273b320f.png)

### 打包扩展程序#

经过一番调试，确定扩展没有问题后，就可以打包了。-
[](https://i.loli.net/2017/10/26/59f193561cf04.png)

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f193561cf04.png)](https://i.loli.net/2017/10/26/59f193561cf04.png)

-
[

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f1935d79981.jpg)



](https://i.loli.net/2017/10/26/59f1935d79981.jpg)

打包好后会在跟目录同级的地方生成一个跟目录同名的以 `.crx` 为后缀的扩展文件，这个文件就可以直接发给别人使用了，至于密钥文件我还不知道有什么用，反正我是删了。

### 后续步骤#

最后初始化一个 `git` 仓库，上传上去，这事就算这么完了。什么？你要发布到 `chrome store` ？首先告诉你这要付 5$ 的开发者年费，对是 5 刀，不是 5 软妹币，其实也不算贵，毕竟装一个很愉快的逼比这点钱重要多了，但是我还是在发布的路上碰到坑了， 例如：-
[](https://i.loli.net/2017/10/26/59f195a440ea6.jpeg)

[![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fi.loli.net%2F2017%2F10%2F26%2F59f195a440ea6.jpeg)](https://i.loli.net/2017/10/26/59f195a440ea6.jpeg)

然后我就放弃了发布到 chrome store 的想法。-
感兴趣的朋友可以自行尝试一下，[传送门](https://chrome.google.com/webstore/developer/dashboard/)

> 本文主要想记录一个 Chrome 扩展开发的大致过程，同时希望让初学者能够看清 Chrome 扩展的神秘面纱后面的真相，所以许多细节并未深究，如读者有兴趣可以在下方评论与我一起讨论。

> 本作品采用[《CC 协议》](https://learnku.com/docs/guide/cc4.0/6589)，转载必须注明作者和本文链接

[桥边红药的博客](https://blog.janguly.com/)

[查看原网页: learnku.com](https://learnku.com/articles/6513/record-the-development-process-of-a-chrome-extension)