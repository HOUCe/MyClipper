# [一步步滑向 Notion 的深渊](https://www.douban.com/note/753771881/?_i=1383946LqfnMpM)

使用了 11 年 Evernote，付费 9 年，累积了 12000 多条笔记，其中有将近 2000 条是网页收藏。春节前突然意识到对 Evernote 的使用是非常碎片化的，Evernote 在工具支持上，对知识系统化也很不友好。

最近一个多月并行使用 Evernote 和 [Notion](https://www.douban.com/link2?url=https%3A%2F%2Fwww.notion.so%2F%3Fr%3D5d5659c9871143cb977f2520fb5bfdcf&vendor=from_people_intro&type=redir&link2key=1c08255925)，正在一步步滑向 [Notion](https://www.douban.com/link2?url=https%3A%2F%2Fwww.notion.so%2F%3Fr%3D5d5659c9871143cb977f2520fb5bfdcf&vendor=from_people_intro&type=redir&link2key=1c08255925) 的深渊。使用过程中，还留有几个非常细枝末节但很影响日常体验的小毛病： 1. 官方的 App 不支持多 Tab； 2. 也不支持自定义样式，哪怕是小幅的；

就像钻进鞋子里的几粒沙子，虽然不影响 Notion 在系统化知识组织上的强大，但依然影响日常使用的观感和体验，尝试了几个方式都不完美： 1. 直接使用浏览器，缺少专注感； 2. 使用 Fluid App / Unite / Coherence Pro 把 Web App 包成 App，都有各种细节问题，比如 Fluid App / Coherence Pro 都不支持自定义 URL 的打开方式，很多时候更希望用默认浏览器打开笔记中引用的链接，而不是直接在“伪” Notion App 中打开（因为默认浏览器中有登陆状态），Unite 的字体渲染效果极其诡异。另外它们的多 Tab 也非常难受，一旦使用多 Tab，就又变得跟浏览器无异了。

昨天晚上突然想起了 Electron，就试了试，没想到基于 Electron 来修理这些小毛病竟如此简单，居然就这样草率地解决了！现在已经支持了： 1. 多 Tab、多窗口； 2. 使用系统默认行为打开外部链接； 3. 自定义样式；

![](https://img1.doubanio.com/view/note/l/public/p70418139.jpg)

![](https://img9.doubanio.com/view/note/l/public/p70418175.jpg)

它叫 [Potion - Personalized Notion](https://github.com/xupeng/Potion)。

非常草率地换了个 logo([https://freesvg.org/bubbling-potion](https://freesvg.org/bubbling-potion))，感谢 Public Domain，现在长这个样了：

![](https://img2.doubanio.com/view/note/l/public/p70420343.jpg)
