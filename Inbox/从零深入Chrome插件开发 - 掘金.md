# [从零深入Chrome插件开发 - 掘金](https://juejin.cn/post/7035782439590952968)

　　使用Chrome插件可以为Chrome浏览器带来一些功能性的扩展，进而提高使用体验；俗话说的好Chrome没插件，香味少一半，Chrome最大的优势还是其支持众多强大好用的扩展程序；今天我们就来了解一下插件是如何开发的，和我们普通的浏览器页面开发有什么区别，以及安利一些功能强大的插件。

　　Chrome插件，官方名称extensions（扩展程序）；为了方便理解，以下都称为Chrome插件，或者简称插件，那么什么是Chrome插件呢？

> 扩展程序是自定义浏览体验的小型软件程序。它们让用户可以通过多种方式定制`Chrome`的功能和行为。

　　插件程序提供了以下几个功能：

-   生产力工具：
-   网页内容丰富
-   信息聚合
-   乐趣和游戏

　　我们可以通过点击`更多工具 -> 扩展程序`来查看我们所有安装的插件，或者直接打开[插件标签页](https://link.juejin.cn/?target=undefined)。

![查看插件标签页](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/add8999ccdda4c878d925e2eb8337071~tplv-k3u1fbpfcp-watermark.awebp)

　　那么学习开发插件有什么意义呢？我们为什么要来学习插件开发呢？个人总结下，有以下几方面的意义：

-   增强浏览器功能，实现属于自己的定制插件功能
-   了解现有的插件，对其功能进行优化改进

## 获取插件

　　大多数Chrome用户从[Chrome网上应用店](https://link.juejin.cn/?target=https%3A%2F%2Fchrome.google.com%2Fwebstore "https://chrome.google.com/webstore")获得插件程序。世界各地的开发人员会在Chrome网上应用店中发布他们的插件，经过Chrome的审查并向最终用户提供。

　　但是由于一些众所周知的原因，我们并不能访问网上应用店，但同时Chrome又要求插件必须从它的Chrome应用商店下载安装，这仿佛是一个绕不开的死循环，不过俗话说`魔高一尺道高一尺一`，下面我们会讲解如何从本地加载插件，绕开网上应用店的限制。

## 插件如何工作？

　　插件是基于Web技术构建的，例如HTML、JavaScript和CSS。它们在单独的沙盒执行环境中运行并与Chrome浏览器进行交互。

　　插件允许我们通过使用API修改浏览器行为和访问Web内容来扩展和增强浏览器的功能。插件通过最终用户UI和开发人员API进行操作：

-   扩展用户界面，这为用户提供了一种一致的方式来管理他们的扩展。
-   扩展的API允许浏览器本身的扩展的代码访问功能：激活标签，修改网络请求等等。

　　要创建插件程序，我们需要组合构成插件程序的一些资源清单，例如JS文件和HTML文件、图像等。对于开发和测试，可以使用扩展开发者模式将这些“解压”加载到Chrome中。如果我们对自己开发出来的插件程序感到满意，就可以通过网上商店将其打包并分享给其他的用户。

## 插件的原则

　　我们编写的插件想要发布到`Chrome网上应用店`中，就必须遵守网上应用店政策，它规定了以下几点：

-   插件必须满足一个定义狭窄且易于理解的单一目的。单个插件可以包含多个组件和一系列功能，只要一切都有助于实现一个共同的目的。
-   用户界面应该是最小的并且有意图。它们的范围可以从简单的图标到打开一个带有表单的新窗口。

　　Chrome插件并没有很严格的项目结构要求，比如src、public、components等等，因此我们如果去看很多插件的源码，会发现每个插件的项目结构，甚至项目下的文件名称都大相径庭；但是在根目录下我们都会找到一个`manifest.json`文件，这是插件的配置文件，说明了插件的各种信息；它的作用等同于小程序的app.json和前端项目的package.json。

　　我们在项目中创建一个最简单的`manifest.json`配置文件：

```
{
    
    "name": "Hello Extensions",
    
    "description" : "Base Level Extension",
    
    "version": "1.0",
    
    "manifest_version": 2
}
复制代码
```

　　我们经常会点击右上角插件图标时弹出一个小窗口的页面，焦点离开时就关闭了，一般做一些临时性的交互操作；在配置文件中新增`browser_action`字段，配置popup弹框：

```
{
    "name": "Hello Extensions",
    "description" : "Base Level Extension",
    "version": "1.0",
    "manifest_version": 2,
    
    "browser_action": {
      "default_popup": "popup.html",
      "default_icon": "popup.png"
    }
}
复制代码
```

　　然后创建我们的弹框页面popup.html：

```
<html>
  <body>
    <h1>Hello Extensions</h1>
  </body>
</html>
复制代码
```

　　点击图标后，插件显示popup.html。

![弹框页面](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/336a9f66b4c44d1ba8c1263c1b2a6268~tplv-k3u1fbpfcp-watermark.awebp)

　　为了用户方便点击，我们还可以在manifest.json中设置一个键盘快捷键的命令，通过快捷键来弹出popup页面：

```
{
  "name": "Hello Extensions",
  "description" : "Base Level Extension",
  "version": "1.0",
  "manifest_version": 2,
  "browser_action": {
    "default_popup": "popup.html",
    "default_icon": "popup.png"
  },
  
  "commands": {
    "_execute_browser_action": {
      "suggested_key": {
        "default": "Ctrl+Shift+F",
        "mac": "MacCtrl+Shift+F"
      },
      "description": "Opens popup.html"
    }
  }
}
复制代码
```

　　这样我们的插件就可以通过按键盘上的`Ctrl+Shift+F`来弹出。

## 加载及调试插件

　　我们开发的插件需要在浏览器里面运行，打开`插件标签页`，打开`开发者模式`，点击`加载已解压的扩展程序`，选择项目文件夹，就可将开发中的插件加载进来。

![加载插件](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f71cac50d0f8499ebeda70b831e25c48~tplv-k3u1fbpfcp-watermark.awebp)

> 开发中更改了代码，点击插件右下角刷新按钮即可重新加载

　　如果我们的代码中有错误，加载插件后，会显示红色的`错误按钮`：

![插件加载错误](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45b46b39f8954c458e3c40218abd013b~tplv-k3u1fbpfcp-watermark.awebp)

　　点击错误按钮以查看错误的日志：

![错误日志](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b76a95e2a1eb4665ad3f6faf2d063402~tplv-k3u1fbpfcp-watermark.awebp)

　　我们上面说过Chrome插件只能从网上应用店中下载安装，但是第三方平台也提供了下载的渠道，下载下来的文件后缀是`.crx`的压缩包，现在的问题就是如何将crx文件进行安装了。

　　从Chrome 73版本开始，谷歌修改了插件策略，不可以随意安装crx文件：如果直接将crx文件拖拽安装可能会提示一下报错：

```
程序包无效："CRX_HEADER_INVALID"
复制代码
```

　　我们可以尝试以下几种方法，第一种方法：将crx后缀改为zip，解压后`加载已解压的扩展程序`的方式，将插件用开发者模式进行加载。

　　第二种办法，通过`Chrome插件伴侣`，将crx提取到桌面，然后还是用开发者模式进行加载。

![Chrome插件伴侣](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b50bc6b7721d46bfaeafffa69f32eb97~tplv-k3u1fbpfcp-watermark.awebp)

　　使用插件伴侣提取插件后，插件内容默认会被放在你的电脑桌面上，可以把它剪切/复制到任意位置；加载插件选择的文件夹路径时，一定要包含manifest.json文件；加载后请勿删除提取的文件夹。

　　第三种方法就是用梯子了，直接去网上应用店下载，没有梯子的同学可以使用插件伴侣进行代理，插件伴侣的获取方式下文会给出。

## 后台background

　　我们的插件安装后，popup页面也运行了；但是我们也发现了，popup页面只能做临时性的交互操作，用完就关了，不能存储信息或者和其他标签页进行交互等等；这时就需要用到background（后台），它是一个常驻的页面，它的生命周期是插件中所有类型页面中最长的；它随着浏览器的打开而打开，随着浏览器的关闭而关闭，所以通常把需要一直运行的、启动就运行的、全局的代码放在background里面。

　　background也是需要在`manifest.json`中进行配置，可以通过`page`指定一张网页，或者通过`scripts`直接指定一个js数组，Chrome会自动为js生成默认网页：

```
{
  "background": {
    
    "scripts": ["background.js"],
    "persistent": true
  }
}
复制代码
```

　　**需要注意**的是，page属性和scripts属性只需要配置一个即可，如果两个同时配置，则会报以下错误信息：

```
Only one of 'background.page', 'background.scripts', and 'background.service_worker' can be specified.
复制代码
```

　　我们给background设置一个监听事件，当插件安装时打印日志：

```

chrome.runtime.onInstalled.addListener(function () {
  console.log("插件已被安装");
});
复制代码
```

　　点击`查看视图`旁边的`背景页`，看到我们设置的background：

![背景页](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d52be349eae044a780bfa53f63d17346~tplv-k3u1fbpfcp-watermark.awebp)

## storage存储

　　我们在插件安装时在storage中设置一个值，这将允许多个插件组件访问该值并进行更新操作：

```

chrome.runtime.onInstalled.addListener(function () {
  
  chrome.storage.sync.set({ color: "#3aa757" }, function () {
    console.log("storage init color value");
  });
  
  chrome.declarativeContent.onPageChanged.removeRules(undefined, function () {
    chrome.declarativeContent.onPageChanged.addRules([
      {
        conditions: [
          new chrome.declarativeContent.PageStateMatcher({
            pageUrl: { hostEquals: "baidu.com" },
          }),
        ],
        actions: [new chrome.declarativeContent.ShowPageAction()],
      },
    ]);
  });
});
复制代码
```

　　`chrome.declarativeContent`用于精确地控制什么时候显示我们的页面按钮，或者需要在用户单击它之前更改它的外观以匹配当前标签页。

　　这里调用的chrome.storage和我们常用的localStorage和sessionStorage不是一个东西；由于调用到了storage和declarativeContent的API，因此我们需要在manifest中给插件注册使用的权限：

```
{
  
  "permissions": ["storage", "declarativeContent"],
  "background": {
    "scripts": ["background.js"],
    "persistent": true
  }
}
复制代码
```

　　再次查看背景页的视图，我们就能看到打印的日志了；既然可以存储，那也能取出来，我们在popup中添加事件进行获取，首先我们新增一个触发的button：

```

<html>
  <head>
    <style>
      button {
        width: 60px;
        height: 30px;
        outline: none;
      }
    </style>
  </head>
  <body>
    <button id="changeColor">change</button>
    <script src="popup.js"></script>
  </body>
</html>
复制代码
```

　　我们再创建一个`popup.js`的文件，用来从storage存储中拿到颜色值，并将此颜色作为按钮的背景色：

```
let changeColor = document.getElementById("changeColor");

changeColor.onclick = function (el) {
  chrome.storage.sync.get("color", function (data) {
    changeColor.style.backgroundColor = data.color;
  });
};
复制代码
```

> 如果需要调试popup页面，可以在弹框中右击 => 检查，在DevTools中进行调试查看。

　　我们多次打开popup页面，发现页面每次点开按钮都会恢复最开始的默认状态。

![storage存储值](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/deef7dffedfc424ca3c09d70f1c26eac~tplv-k3u1fbpfcp-watermark.awebp)

## 获取浏览器tabs

　　现在，我们获取到了storage中的值，需要逻辑来进一步与用户交互；更新popup.js中的交互代码：

```

changeColor.onclick = function (el) {
  chrome.storage.sync.get("color", function (data) {
    let { color } = data;
    chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
      chrome.tabs.executeScript(tabs[0].id, {
        code: 'document.body.style.backgroundColor = "' + color + '";',
      });
    });
  });
};
复制代码
```

　　`chrome.tabs`的API主要是和浏览器的标签页进行交互，通过`query`找到当前的激活中的tab，然后使用`executeScript`向标签页注入脚本内容。

　　manifest同样需要`activeTab`的权限，来允许我们的插件使用`tabs`的API。

```
{
  "name": "Hello Extensions",
  
  "permissions": ["storage", "declarativeContent", "activeTab"],
}
复制代码
```

　　重新加载插件，我们点击按钮，会发现当前页面的背景颜色已经变成storage中设置的色值了；但是某些用户可能希望使用不同的色值，我们给用户提供选择的机会。

## 颜色选项页面

　　现在我们的插件功能还比较单一，只能让用户选择唯一的颜色；我们可以在插件中加入选项页面，以便用户更好的自定义插件的功能。

　　在程序目录新增一个options.html文件：

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      button {
        height: 30px;
        width: 30px;
        outline: none;
        margin: 10px;
      }
    </style>
  </head>
  <body>
    <div id="buttonDiv"></div>
    <div>
      <p>选择一个不同的颜色</p>
    </div>
  </body>
  <script src="options.js"></script>
</html>
复制代码
```

　　然后添加选择页面的逻辑代码`options.js`：

```
let page = document.getElementById("buttonDiv");
const kButtonColors = ["#3aa757", "#e8453c", "#f9bb2d", "#4688f1"];
function constructOptions(kButtonColors) {
  for (let item of kButtonColors) {
    let button = document.createElement("button");
    button.style.backgroundColor = item;
    button.addEventListener("click", function () {
      chrome.storage.sync.set({ color: item }, function () {
        console.log("color is " + item);
      });
    });
    page.appendChild(button);
  }
}
constructOptions(kButtonColors);
复制代码
```

　　上面代码中预设了四个颜色选项，通过onclick事件监听，生成页面上的按钮；当用户单击按钮时，将更新storage中存储的颜色值。

　　options页面完成后，我们可以将其在manifest的`options_page`进行注册：

```
{
  "name": "Hello Extensions",
  
  "options_page": "options.html",
  
  "manifest_version": 2
}
复制代码
```

　　重新加载我们的插件，点击详情，滚动到底部，点击`扩展程序选项`来查看选项页面。

![选项页面](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/67bfbf3cd7bb48f7b4f974c6e5d4159e~tplv-k3u1fbpfcp-watermark.awebp)

　　或者可以在浏览器右上角插件图标上`右击 => 选项`。

　　通过上面一个简单的小插件，相信大家对插件的功能和组件都有了一个大致的了解，知道了每个组件在其中发挥的作用；但这还只是插件的一小部分功能，下面我们对插件每个部分的功能以及组件做一个更深入的了解。

## 使用background管理事件

　　background是插件的事件处理程序，它包含对插件很重要的浏览器事件的监听器。background处于休眠状态，直到触发事件，然后执行指示的逻辑；一个好的background仅在需要时加载，并在空闲时卸载。

![background和popup交互](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/42fadde3d4fd4ee78f8acc36082099aa~tplv-k3u1fbpfcp-watermark.awebp)

　　background监听的一些浏览器事件包括：

-   插件程序首次安装或更新为新版本。
-   后台页面正在监听事件，并且已调度该事件
-   内容脚本或其他插件发送消息
-   插件中的另一个视图（例如弹出窗口）调用runtime.getBackgroundPage

　　加载完成后，只要触发某个事件，background就会保持运行状态；在上面manifest中，我们还指定了一个`persistent`属性：

```
{
  "background": {
    "scripts": ["background.js"],
    "persistent": true
  }
}
复制代码
```

　　`persistent`属性定义了插件常驻后台的方式；当其值为true时，表示插件将一直在后台运行，无论其是否正在工作；当其值为false时，表示插件在后台按需运行，这就是Chrome后来提出的`Event Page`（非持久性后台）。`Event Page`是基于事件驱动运行的，只有在事件发生的时候才可以访问；这样做的目的是为了能够有效减小插件对内存的消耗，如非必要，请将persistent设置为false。

> persistent属性的默认值为true

### alarms

　　一些基于DOM页面的计时器（例如window.setTimeout或window.setInterval），如果在非持久后台休眠时进行了触发，可能不会按照预定的时间运行：

```
let timeout = 1000 * 60 * 3;  
window.setTimeout(function() {
  alert('Hello, world!');
}, timeout);
复制代码
```

　　Chrome提供了另外的API，alarms：

```
chrome.alarms.create({delayInMinutes: 3.0})

chrome.alarms.onAlarm.addListener(function() {
  alert("Hello, world!")
});
复制代码
```

## browserAction

　　我们知道了browser\_action字段用来配置popup的页面，在其他的一些文档中还给出了`page_action`字段的配置，不过page\_action并不是所有的页面都能够使用；不过随着Chrome的版本更新，这两者的功能也越来越相近；在Chrome 48版本之后，page\_action也从原来的地址栏中移出来，和插件放在一起；笔者在配置`page_action`的时候没有发现有什么比较大的区别，因此下面以browser\_action为主。

　　在browserAction的配置中，我们可以提供多种尺寸的图标，Chrome会选择最接近的图标并将其缩放到适当的大小来填充；如果没有提供确切的大小，这种缩放会导致图标丢失细节或看起来模糊。

```
{
  
  "browser_action": {
    "default_icon": {                
      "16": "images/icon16.png",     
      "24": "images/icon24.png",     
      "32": "images/icon32.png"      
    },
    "default_title": "hello popup",  
    "default_popup": "popup.html"    
  },
}
复制代码
```

　　也可以通过调用`browserAction.setPopup`动态设置弹出窗口。

```
chrome.browserAction.setPopup({popup: 'popup_new.html'});
复制代码
```

### Tooltip

　　要设置提示文案，使用default\_title字段，或者调用`browserAction.setTitle`函数。

```
chrome.browserAction.setTitle({ title: "New Tooltip" });
复制代码
```

![Tooltip](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/750298ae188d49928633e7bbd9b7456a~tplv-k3u1fbpfcp-watermark.awebp)

### Badge

　　Badge（徽章）就是在图标上显示的一些文本内容，用来详细显示插件的提示信息；由于Bage的空间有限，因此最多显示4个英文字符或者2个函数；badge无法通过配置文件来指定，必须通过代码实现，设置badge文字和颜色可以分别使用`browserAction.setBadgeText()`和`browserAction.setBadgeBackgroundColor()`：

```
chrome.browserAction.setBadgeText({ text: "new" });
chrome.browserAction.setBadgeBackgroundColor({ color: [255, 0, 0, 255] });


复制代码
```

![Badge](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4b8293069f384801800bb74112cd962b~tplv-k3u1fbpfcp-watermark.awebp)

## content-scripts

　　content-scripts（内容脚本）是在网页上下文中运行的文件。通过使用标准的文档对象模型(DOM)，它能够读取浏览器访问的网页的详细信息，对其进行更改，并将信息传递给其父级插件。内容脚本相对于background还是有一些访问API上的限制，它可以直接访问以下chrome的API：

-   i18n
-   storage
-   runtime:
    -   connect
    -   getManifest
    -   getURL
    -   id
    -   onConnect
    -   onMessage
    -   sendMessage

　　内容脚本运行于一个独立、隔离的环境，它不会和`主页面的脚本`或者`其他插件的内容脚本`发生冲突，当然也不能调用其上下文和变量。假设我们在主页面中定义了变量和函数：

```
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <script>
      const a = { a: 1, b: "2" };
      const b = { a: 1, b: "2", c: [] };
      function add(a, b){ return a + b };
    </script>
  </body>
</html>
复制代码
```

　　由于隔离的机制，在内容脚本中调用add函数会报错：Uncaught ReferenceError: add is not defined。

　　内容脚本分为以代码方式或声明方式注入。

### 代码方式注入

　　对于需要在特定情况下运行的代码，我们需要使用代码注入的方式；在上面的popup页面中，我们就是将内容脚本以代码的方式进行注入到页面中：

```
chrome.tabs.executeScript(tabs[0].id, {
  code: 'document.body.style.backgroundColor = "red";',
});
复制代码
```

　　或者可以注入整个文件。

```
chrome.tabs.executeScript(tabs[0].id, {
  file: "contentScript.js",
});
复制代码
```

### 声明式注入

　　在指定页面上自动运行的内容脚本，我们可使用声明式注入的方式；以声明方式注入的脚本需注册在manifest文件的`content_scripts`属性下。它们可以包括JS文件或CSS文件。

```
{
  "content_scripts": [
    {
      
      "matches": ["https://*.xieyufei.com/*"],
      "css": ["myStyles.css"],
      "js": ["contentScript.js"]
    }
  ]
}
复制代码
```

　　声明式注入除了matches必须外，还可以包含以下字段，来自定义指定页面匹配：

Name

Type

Description

exclude\_matches

字符串数组

可选。排除此内容脚本将被注入的页面。

include\_globs

字符串数组

可选。 在 matches 后应用，以匹配与此 glob 匹配的URL。旨在模拟 @exclude 油猴关键字。

exclude\_globs

字符串数组

可选。 在 matches 后应用，以排除与此 glob 匹配的URL。旨在模拟 @exclude 油猴关键字。

　　声明匹配URL可以使用Glob属性，Glob属性遵循更灵活的语法。可接受的Glob字符串可能包含“通配符”星号和问号的URL。星号`*`匹配任意长度的字符串，包括空字符串，而问号`？`匹配任何单个字符。

```
{
  "content_scripts": [
    {
      "matches": ["https://*.xieyufei.com/*"],
      "exclude_matches": ["*://*/*business*"],
      "include_globs": ["*xieyufei.com/???s/*"],
      "exclude_globs": ["*science*"],
      "js": ["contentScript.js"]
    }
  ]
}
复制代码
```

　　将JS文件注入网页时，还需要控制文件注入的时机，由`run_at`字段控制；首选的默认字段是`document_idle`，但如果需要，也可以指定为 “document\_start” 或“document\_end”。

```
{
  "content_scripts": [
    {
      "matches": ["https://*.xieyufei.com/*"],
      "run_at": "document_idle",
      "js": ["contentScript.js"]
    }
  ]
}
复制代码
```

　　三个字段注入的时机区别如下：

Name

Type

Description

document\_idle

string

首选。 尽可能使用 “document\_idle”。浏览器选择一个时间在 “document\_end” 和window.onload 事件触发后立即注入脚本。 注入的确切时间取决于文档的复杂程度以及加载所需的时间，并且已针对页面加载速度进行了优化。在 “document\_idle” 上运行的内容脚本不需要监听 window.onload 事件，因此可以确保它们在 DOM 完成之后运行。如果确实需要在window.onload 之后运行脚本，则扩展可以使用 document.readyState 属性检查 onload 是否已触发。

document\_start

string

在 css 文件之后，但在构造其他 DOM 或运行其他脚本前注入。

document\_end

string

在 DOM 创建完成后，但在加载子资源（例如 images 和 frames ）之前，立即注入脚本。

### 消息通信

　　尽管内容脚本的执行环境和托管它们的页面是相互隔离的，但是它们共享对页面DOM的访问；如果内容脚本想要和插件通信，可以通过`onMessage`和`sendMessage`

```

chrome.runtime.onMessage.addListener(function (message, sender, sendResponse) {
  console.log("content-script收到的消息", message);
  sendResponse("我收到了你的消息！");
});

chrome.runtime.sendMessage(
  { greeting: "我是content-script呀，我主动发消息给后台！" },
  function (response) {
    console.log("收到来自后台的回复：" + response);
  }
);
复制代码
```

　　更多消息通信的在后面我们会进行详细的总结。

## contextMenus

　　contextMenus可以自定义浏览器的右键菜单（也有叫上下文菜单的），主要是通过`chrome.contextMenus`API实现；在manifest中添加权限来开启菜单权限：

```
{
  
  "permissions": ["contextMenus"],
  "icons": {
    "16": "contextMenus16.png",
    "48": "contextMenus48.png",
    "128": "contextMenus128.png"
   }
}
复制代码
```

　　通过`icons`字段配置contextMenus菜单旁边的图标：

![ContextMenu](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad9c07492aa643c1ab7c5c6a803b19fa~tplv-k3u1fbpfcp-watermark.awebp)

　　我们可以在background中调用`contextMenus.create`来创建菜单，这个操作应该在`runtime.onInstalled`监听回调执行：

```
chrome.contextMenus.create({
  id: "1",
  title: "Test Context Menu",
  contexts: ["all"],
});

chrome.contextMenus.create({
  type: "separator",
});

chrome.contextMenus.create({
  id: "2",
  title: "Parent Context Menu",
  contexts: ["all"],
});
chrome.contextMenus.create({
  id: "21",
  parentId: "2",
  title: "Child Context Menu1",
  contexts: ["all"],
});

复制代码
```

　　如果我们的插件创建多个右键菜单，则Chrome会自动将其折叠为一个父菜单。

![多级右键菜单](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8bb14398546c4eeabc78880600ceaf52~tplv-k3u1fbpfcp-watermark.awebp)

　　contextMenus创建对象的属性可以在附录里面找到；我们看到在title属性中有一个`%s`的标识符，当contexts为selection，使用`%s`来表示选中的文字；我们通过这个功能可以实现一个选中文字调用百度搜索的小功能：

```
chrome.contextMenus.create({
  id: "1",
  title: "使用百度搜索：%s",
  contexts: ["selection"],
  onclick: function (params) {
    chrome.tabs.create({
      url:
        "https://www.baidu.com/s?ie=utf-8&wd=" +
        encodeURI(params.selectionText),
    });
  },
});
复制代码
```

　　效果如下：

![百度搜索小功能](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9d9e52b6dc8410cb8cc411600161be5~tplv-k3u1fbpfcp-watermark.awebp)

　　contextMenus还有一些API可以调用：

```

chrome.contextMenus.remove(menuItemId)；

chrome.contextMenus.removeAll();

chrome.contextMenus.update(menuItemId, updateProperties);

chrome.contextMenus.onClicked.addListener(function(OnClickData info, tabs.Tab tab) {...});
复制代码
```

## override

　　覆盖页面（override）是一种将Chrome默认的特定页面替换为插件程序中的HTML文件。除了HTML之外，覆盖页面通常还有CSS和JS代码；插件可以替换以下Chrome的页面。

-   书签管理器
    -   当用户从 Chrome 菜单中选择书签管理器菜单项或在 Mac 上从书签菜单中选择书签管理器项时出现的页面。您也可以通过输入 URL chrome://bookmarks来访问此页面。
-   历史记录
    -   当用户从 Chrome 菜单中选择历史记录菜单项或在 Mac 上从历史记录菜单中选择显示完整历史记录项时出现的页面。您也可以通过输入URL chrome://history来访问此页面。
-   新标签
    -   当用户创建新标签或窗口时出现的页面。您也可以通过输入 URL chrome://newtab来访问此页面。

　　PS：像我们熟知的Momentum插件，就是覆盖了新标签页面。

> 需要注意的是：单个插件只能覆盖某一个页面。例如，插件程序不能同时覆盖书签管理器和历史记录页面。

　　在manifest进行如下配置：

```
{
  "chrome_url_overrides": {
    "newtab": "newtab.html",
    
    
  }
}
复制代码
```

　　覆盖newtab效果如下：

![覆盖newtab](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3f04fe5452ce408dbbb64139c71118ef~tplv-k3u1fbpfcp-watermark.awebp)

　　如果我们覆盖多个特定页面，Chrome加载插件时会直接报错：

![覆盖多个页面报错](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0623e705d94e4cd3bcdd5b5b3274cfc5~tplv-k3u1fbpfcp-watermark.awebp)

## storage

　　用户在操作时，会产生一些用户数据，插件需要在本地存储这些数据，在需要调用的时候再拿出来；Chrome推荐使用`chrome.storage`的API，该API经过优化，提供和localStorage相同的存储功能；不推荐直接存在`localStorage`中，两者主要有以下区别：

-   用户数据使用chrome.storage存储可以和Chrome的同步功能自动同步。
-   插件的内容脚本可以直接访问用户数据，而无需background。
-   chrome.storage可以直接存储对象，而localStorage是存储字符串，需要再次转换

　　如果要使用storage的自动同步，我们可以使用`storage.sync`：

```
chrome.storage.sync.set({key: value}, function() {
  console.log('Value is set to ' + value);
});

chrome.storage.sync.get(['key'], function(result) {
  console.log('Value currently is ' + result.key);
});
复制代码
```

　　当Chrome离线时，Chrome会将数据存储在本地。下次浏览器在线时，Chrome会同步数据。即使用户禁用同步，storage.sync仍将工作。

　　不需要同步的数据可以用`storage.local`进行存储：

```
chrome.storage.local.set({key: value}, function() {
  console.log('Value is set to ' + value);
});

chrome.storage.local.get(['key'], function(result) {
  console.log('Value currently is ' + result.key);
});
复制代码
```

　　如果我们想要监听storage中的数据变化，可以用`onChanged`添加监听事件；每当存储中的数据发生变化时，就会触发该事件：

```


chrome.storage.onChanged.addListener(function (changes, namespace) {
  for (let [key, { oldValue, newValue }] of Object.entries(changes)) {
    console.log(
      `Storage key "${key}" in namespace "${namespace}" changed.`,
      `Old value was "${oldValue}", new value is "${newValue}".`
    );
  }
});
复制代码
```

## devtools

　　用过Vue或者React的devtools的童鞋应该见过这样新增的扩展面板：

![Vue Panel](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/797d03728824441fba4ad45b3befca8a~tplv-k3u1fbpfcp-watermark.awebp)

　　DevTools可以为Chrome的DevTools添加功能，它可以添加新的UI面板和侧边栏，与检查的页面交互，获取有关网络请求的信息等等；它可以访问以下特定的API：

-   devtools.inspectedWindow
-   devtools.network
-   devtools.panels

　　DevTools扩展的结构与任何其他扩展一样：它可以有一个背景页面、内容脚本和其他项目。此外，每个DevTools扩展都有一个DevTools页面，可以访问DevTools的API。

![DevTools扩展](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d67bddfe82b44f4cbbab0a642eb5d4ca~tplv-k3u1fbpfcp-watermark.awebp)

　　配置devtools不需要权限，只要在manifest中配置一个`devtools.html`：

```
{
  "devtools_page": "devtools.html",
}
复制代码
```

　　devtools.html中只引用了devtools.js，如果写了其他内容也不会展示：

```
<!DOCTYPE html>
<html lang="en">
  <head> </head>
  <body>
    <script type="text/javascript" src="./devtools.js"></script>
  </body>
</html>
复制代码
```

　　在项目中新建`devtools.js`：

```


chrome.devtools.panels.create(
  
  "DevPanel",
  
  "panel.png",
  
  "Panel.html",
  function (panel) {
    console.log("自定义面板创建成功！");
  }
);


chrome.devtools.panels.elements.createSidebarPane(
  "Sidebar",
  function (sidebar) {
    sidebar.setPage("sidebar.html");
  }
);
复制代码
```

　　这里调用create创建扩展面板，createSidebarPane创建侧边栏，每个扩展面板和侧边栏都是一个单独的HTML页面，其中可以包含其他资源（JavaScript、CSS、图像等）。

### DevPanel

　　DevPanel面板是一个顶级标签，和Element、Source、Network等是同一级，在一个devtools.js可以创建多个；在`Panel.html`中我们先设置2个按钮：

```
<!DOCTYPE html>
<html lang="en">
  <head></head>
  <body>
    <div>dev panel</div>
    <button id="check_jquery">检查jquery</button>
    <button id="get_all_resources">获取所有资源</button>
    <script src="panel.js"></script>
  </body>
</html>
复制代码
```

　　panel.js中我们使用`devtools.inspectedWindow`的API来和被检查窗口进行交互：

```
document.getElementById("check_jquery").addEventListener("click", function () {
  chrome.devtools.inspectedWindow.eval(
    "jQuery.fn.jquery",
    function (result, isException) {
      if (isException) {
        console.log("the page is not using jQuery");
      } else {
        console.log("The page is using jQuery v" + result);
      }
    }
  );
});

document
  .getElementById("get_all_resources")
  .addEventListener("click", function () {
    chrome.devtools.inspectedWindow.getResources(function (resources) {
      console.log(resources);
    });
  });
复制代码
```

　　`eval`函数为插件提供了在被检查页面的上下文中执行JS代码的能力，而`getResources`获取页面上所有加载的资源；我们找到一个页面，然后右击检查打开调试工具，发现在最右侧多了一个`DevPanel`的tab页，点击我们的调试按钮，那么日志在哪里能看到呢？

　　我们在调试工具上右击检查，再开一个调试工具，这个就是调试工具的调试工具。。。。

![禁止套娃](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/760a2f5613594597a1997d4ad8831d2a~tplv-k3u1fbpfcp-watermark.awebp)

　　最终两个调试工具的效果如下：

![DevTools面板](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/00117c3fff474e0194622a144ce52d15~tplv-k3u1fbpfcp-watermark.awebp)

### Sidebar

　　回到devtools.js，我们使用`createSidebarPane`创建了侧边栏面板，并且设置为`sidebar.html`，最终呈现在`Element`面板的最右侧：

![侧边栏面板](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3594f2554a7843dd87c4a713e16c7dff~tplv-k3u1fbpfcp-watermark.awebp)

　　有几种方法可以在侧边栏中显示内容：

-   HTML内容。调用setPage以指定要在窗格中显示的 HTML 页面。
-   JSON数据。将JSON对象传递给setObject.
-   JavaScript表达式。将表达式传递给setExpression

　　通过JS表达式，我们可以很方便进行页面查询，比如，查询页面上所有的img元素：

```
chrome.devtools.panels.elements.createSidebarPane(
  "All Images",
  function (sidebar) {
    sidebar.setExpression('document.querySelectorAll("img")', "All Images");
  }
);
复制代码
```

![展示所有图片](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e71a977920ff45b5a07c2fd4f2a70f34~tplv-k3u1fbpfcp-watermark.awebp)

　　另外，我们可以通过`elements.onSelectionChanged`监听事件，在Element面板选中元素更改后，更新侧边栏面板的状态；例如，可以将我们关心的一些元素的样式进行实时展示在侧边面板，方面查看：

```
var page_getProperties = function () {
  if ($0 instanceof HTMLElement) {
    return {
      tag: $0.tagName.toLocaleLowerCase(),
      class: $0.classList,
      width: window.getComputedStyle($0)["width"],
      height: window.getComputedStyle($0)["height"],
      margin: window.getComputedStyle($0)["margin"],
      padding: window.getComputedStyle($0)["padding"],
      color: window.getComputedStyle($0)["color"],
      fontSize: window.getComputedStyle($0)["fontSize"],
    };
  } else {
    return {};
  }
};

chrome.devtools.panels.elements.createSidebarPane(
  "Select Element",
  function (sidebar) {
    function updateElementProperties() {
      sidebar.setExpression(
        "(" + page_getProperties.toString() + ")()",
        "select element"
      );
    }
    updateElementProperties();
    chrome.devtools.panels.elements.onSelectionChanged.addListener(
      updateElementProperties
    );
  }
);
复制代码
```

![元素样式实时展示](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1334aad0f2914a4dab90a982f9d28db9~tplv-k3u1fbpfcp-watermark.awebp)

## notifications

　　Chrome提供`chrome.notifications`的API来推送桌面通知；同样也需要现在manifest中注册权限：

```
{
  "permissions": [
    "notifications"
  ],
}
复制代码
```

　　在background调用创建即可

```

chrome.notifications.create(null, {
  type: "basic",
  iconUrl: "drink.png",
  title: "喝水小助手",
  message: "看到此消息的人可以和我一起来喝一杯水",
});
复制代码
```

　　效果如下：

![消息推送](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5b81d17fc27b402391c3441152f235e9~tplv-k3u1fbpfcp-watermark.awebp)

　　根据chrome.notifications的API，笔者做了一个[喝水小助手](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Facexyf%2Fdrink-helper "https://github.com/acexyf/drink-helper")，和我一起成为一天八杯水的人吧！

> chrome.notifications支持更多的属性详见阅读原文的附录。

## webRequest

　　通过webRequest的API可以对浏览器发出的任何HTTP请求进行拦截、组织或者修改；可以拦截的请求还包括脚本、样式的GET请求以及图片的链接；我们也需要在manifest中配置权限才能使用API：

```
{
  
  "permissions": [
    "webRequest",
    "webRequestBlocking",
    "*://*.xieyufei.com/"
  ],
}
复制代码
```

　　权限中还需要声明拦截请求的URL，如果你想拦截所有的URL，可以使用`*://*/*`（不过不推荐这么做，数据会非常多），如果我们想以阻塞方式使用Web请求API，则需要用到`webRequestBlocking`权限。

　　比如我们可以对拦截的请求进行取消：

```
chrome.webRequest.onBeforeRequest.addListener(
  function (detail) {
    return {cancel: details.url.indexOf("example.com") != -1};;
  },
  { urls: ["<all_urls>"] },
  ["blocking"]
);
复制代码
```

　　不同组件之间经常需要进行消息通信来进行数据的传递，我们来看下他们之间是如何进行通信的：

## background和popup通信

　　background和popup之间的通信比较简单，在popup中，我们可以通过`extension.getBackgroundPage`直接获取到background对象，直接调用对象上的方法即可：

```

var bg = chrome.extension.getBackgroundPage();
bg.someMethods()
复制代码
```

　　而background访问popup上则通过`extension.getViews`来访问，不过前提是popup弹框已经展示，否则获取到的views是空数组：

```

var views = chrome.extension.getViews({type:'popup'});
if(views.length > 0) {
  
console.log(views[0].location.href);
}
复制代码
```

## background和内容脚本通信

　　在background和内容脚本通信，我们可以使用简单直接的`runtime.sendMessage`或者`tabs.sendMessage`发送消息，消息内容可以是JSON数据

　　从内容脚本发送消息如下：

```

chrome.runtime.sendMessage(
  { greeting: "hello，我是content-script，主动发消息给后台！" },
  function (response) {
    console.log("收到来自后台的回复：" + response);
  }
);
复制代码
```

　　而从后台发送消息到内容脚本时，由于有多个标签页，我们需要指定发送到某个标签页：

```

chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
  chrome.tabs.sendMessage(
    tabs[0].id,
    { greeting: "hello，我是后台，主动发消息给content-script" },
    function (response) {
      console.log(response.farewell);
    }
  );
});
复制代码
```

　　而不管是在后台，还是在内容脚本中，我们都使用`runtime.onMessage`监听消息的接收事件，不同的是回调函数中的`sender`，标识不同的发送方：

```
chrome.runtime.onMessage.addListener(
  function(request, sender, sendResponse) {
    console.log(sender.tab ?
      "from a content script:" + sender.tab.url :
      "from the extension");
    if (request.greeting.indexOf("hello") !== -1){
      sendResponse({farewell: "goodbye"});
    }
  });
复制代码
```

## 长链接

　　上面的`runtime.sendMessage`和`tabs.sendMessage`都属于短链接，所谓的短连接，就是类似于HTTP请求，如果接收方不在线，就会出现请求失败的情况；但有些情况下，需要持续对话，这时候就需要用到长链接，类似于websocket，可以在通信双方之间进行持久链接。

　　长链接使用`runtime.connect`或`tabs.connect`来打开长生命周期通道，通道可以有一个名称，以便区分不同类型的连接。

```


var port = chrome.runtime.connect({name: "knockknock"});
port.postMessage({joke: "Knock knock"});
port.onMessage.addListener(function(msg) {
  if (msg.question == "Who's there?")
    port.postMessage({answer: "Madame"});
  else if (msg.question == "Madame who?")
    port.postMessage({answer: "Madame... Bovary"});
});
复制代码
```

　　从background向内容脚本发送消息也类似，不同之处在于需要指定连接的tab页，将`runtime.connect`改为`tabs.connect`。

　　在接收端，我们需要设置`onConnect`的事件监听器，当发送端调用`connect`进行连接时触发该事件，以及通过连接发送和接收消息的`port`对象：

```

chrome.runtime.onConnect.addListener(function(port) {
  console.assert(port.name == "knockknock");
  port.onMessage.addListener(function(msg) {
    if (msg.joke == "Knock knock")
      port.postMessage({question: "Who's there?"});
    else if (msg.answer == "Madame")
      port.postMessage({question: "Madame who?"});
    else if (msg.answer == "Madame... Bovary")
      port.postMessage({question: "I don't get it."});
  });
});
复制代码
```

　　介绍了这么多插件的开发，我们来介绍一下应用市场上的优秀插件，这些插件能够帮助我们在平时的开发中提高生产效率。

## Adblock Plus

　　Adblock Plus是一款可以屏蔽广告以及任何你想屏蔽元素的软件；它不仅内置了一些过滤规则，可以自动屏蔽广告，还可以自行添加屏蔽内容。

![屏蔽效果](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ffd0753833604ac599a225bed3675cbc~tplv-k3u1fbpfcp-watermark.awebp)

　　选择拦截元素，淡黄色框住的内容就是拦截的内容

![自定义拦截内容](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7fe8ad60aaed4bea9fdf41b9acc76ab9~tplv-k3u1fbpfcp-watermark.awebp)

## Axure RP Extension for Chrome

　　Axure RP Extension for Chrome是原型设计工具Axure RP的Chrome浏览器插件，chrome浏览器打开axure生成的HTML静态文件页面预览打开如下报错，这是因为chrome浏览器没有安装Axure插件导致的。

![Axure](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c4b79ddbc5114072a738cf995c147cc0~tplv-k3u1fbpfcp-watermark.awebp)

## FeHelper前端助手

　　FE助手是由国人开发的一款前端工具集合的小插件，插件功能比较全面：包括字符串编解码、代码压缩、美化、JSON格式化、正则表达式、时间转换工具、二维码生成与解码、编码规范检测、页面性能检测、页面取色、Ajax接口调试。

![FE助手](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ec17e14d3cef4abdb11b0f333aea1f47~tplv-k3u1fbpfcp-watermark.awebp)

## Momentum

　　Momentum插件是一款自动更换壁纸，自带时钟，任务日历和工作清单的chrome浏览器插件。官方的解释就是：替换你 Chrome 浏览器默认的“标签页”。里面的图片全部来自500PX里面的高清图，无广告，无弹窗，非常适合笔记本使用，让装逼再上新台阶。让我来感受下出自细节，触及心灵的美。

![Momentum](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b6650db09ad40c89997148d5c7e0aa7~tplv-k3u1fbpfcp-watermark.awebp)

## Octotree

　　在Github上查看源代码的体验十分糟糕，尤其是从一个目录跳转到另一个目录的时候，非常麻烦。Octotree是一款chrome插件，用于将Github项目代码以树形格式展示，而且在展示的列表中，我们可以下载指定的文件，而不需要下载整个项目。

![Octotree](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d9d2878fe9d403a9c8a6517b84dc355~tplv-k3u1fbpfcp-watermark.awebp)

## OneTab

　　Chrome浏览器很好用，这是我们不得不承认的；但是人无完人，何况一个浏览器呢？一直以来，Chrome占用内存这样的“吃相”就很让人头疼。

![占用内存](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0aef163ec4524969b6aa86385ca95e75~tplv-k3u1fbpfcp-watermark.awebp)

　　OneTab是一款可以在用户打开过多Chrome标签页而“不知所措”的时候点击OneTab插件一键释放Chrome标签页内存的谷歌浏览器插件，OneTab插件并不是像关闭浏览器那样直接把所有的标签页都关闭掉，它会先把现有的标签页都缓存起来，然后使用一键关闭所有标签页的功能弹出只有一个恢复窗口的新标签页，在这个OneTab插件的标签页中用户可以选择恢复其中有用的Chrome标签页而放弃其他应该关闭的标签页。

![OneTab](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa7b62ef54084e8ab57e59a42d480f96~tplv-k3u1fbpfcp-watermark.awebp)

　　在恢复标签页的时候，OneTab插件会以新标签页的方式去恢复，所以用户可以简单地点击几次鼠标都可以把有用的标签都找出来一起恢复，当用户打开的Chrome标签页过多的时候使用OneTab插件大约能够节省用户95%的系统内存，还可以让用户在标签页变小的情况下更加清晰地关注自己应该关注的Chrome标签页。

## Tampermonkey

　　Tampermonkey（俗称油猴）是一款免费的浏览器扩展和最为流行的用户脚本管理器。虽然有些受支持的浏览器拥有原生的用户脚本支持，但 Tampermonkey将在您的用户脚本管理方面提供更多的便利。 它提供了诸如便捷脚本安装、自动更新检查、标签中的脚本运行状况速览、内置的编辑器等众多功能， 同时Tampermonkey还有可能正常运行原本并不兼容的脚本。

![Tampermonkey](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0cd0f3dccc7d40bebc490b7a2f103d93~tplv-k3u1fbpfcp-watermark.awebp)

　　通过给管理器安装各类脚本，可以让大部分 HTML 为主的网页更方便易用，比如：全速下载网盘文件、去广告、悬停显示大图、Flash/HTML5 播放器转换、阅读模式等。有点像给Chrome的插件装上插件（这里又是一个套娃）。

## Web Vitals

　　多年来，Google 提供了很多工具：Lighthouse, Chrome DevTools, PageSpeed Insights, Search Console's Speed Report 等来衡量和报告性能。而其中的衡量标准都很难学习和使用，Web Vitals计划的目的就是简化场景，降低学习成本，并帮助站点关注最重要的指标

　　Web Vitals是Google发起的，旨在提供各种质量信号的统一指南，其可获取三个关键指标（CLS、FID、LCP）：

![Web Vitals](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c0cec7b6592486abf8de7556e33a8fe~tplv-k3u1fbpfcp-watermark.awebp)

　　通过在浏览器安装Web Vitals插件，我们就可以在页面加载完成后很方便的查看这三个指标的情况。

![Web Vitals插件](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbb9e9debea945c782288b49a2ef5c2b~tplv-k3u1fbpfcp-watermark.awebp)

## Allow CORS

　　我们开发过程中经常会遇到接口跨域的问题，通过Allow CORS: Access-Control-Allow-Origin这个插件，可以允许我们在接口的响应头轻松执行跨域请求，只需要激活插件并且执行。

![Allow CORS插件](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/027aa889f7fd414aa0df6f06b5087629~tplv-k3u1fbpfcp-watermark.awebp)

　　安装插件后，默认是不会添加跨域响应头的，点击插件弹框的C字母按钮，按钮变成橙色插件激活。

## Window Resizer

　　Window Resizer是一款可以设置浏览器窗口大小的Chrome扩展，用户安装了window resizer插件后可以快速调节chrome的窗口大小，用户可以将窗口调节为320x480、480x800、1024x768等大小，也可以选择自定义浏览器窗口的尺寸。

![Window Resizer](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c9063ea71f84ef99ba9bf5a42d05eb4~tplv-k3u1fbpfcp-watermark.awebp)

> 以上所有插件和工具在公众号`前端壹读`后台回复关键字`Chrome插件`即可获取。

如果觉得写得还不错，请关注我的[掘金主页](https://juejin.im/user/580038cebf22ec0064bd0b2d "//juejin.im/user/580038cebf22ec0064bd0b2d")。更多文章请访问[谢小飞的博客](https://link.juejin.cn/?target=http%3A%2F%2Fxieyufei.com "http://xieyufei.com")
