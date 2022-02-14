# Mac 最强大的终端工具之 iTerm2 详解

[www.pengzhenjin.top](https://www.pengzhenjin.top/archives/mac%E6%95%88%E7%8E%87%E7%A5%9E%E5%99%A8%E5%B7%A5%E5%85%B7%E4%B9%8Baifred%E8%AF%A6%E8%A7%A3#SGZGfHDP)1,196 次访问

## 前言

macOS 内置的 Spotlight（聚焦） 功能让我们可以方便地搜索文件、启动应用、查询单词，我还记得刚使用时感到的那份惊艳。那有没有比 Spotlight 更好用，更强大的工具呢？当然有啦，答案就是 Alfred。那 Alfred 是什么呢？让我们拭目以待吧。

*   Alfred 是 Mac 上一款著名的效率应用，强大的功能和众多的扩展能让你在实际操作中大幅提升工作效率。
*   Alfred 是一个用键盘通过热键、关键字、自定义插件来加快操作效率的工具，它不但是搜索工具，还是快速启动工具，甚至能够操作许多系统功能，扩充性极强，如果有兴趣应该还可以写一个煮咖啡的插件出来。简单点说就是使用了 Alfred 后你就可以丢掉鼠标了！
*   Windows 版本请看这里：[火柴官网](https://huochaipro.com/)

## Alfred 的安装

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_logo.png)

*   可以在 [Alfred 官网](https://www.alfredapp.com/) 免费下载，并安装。
*   可以在 App Store 里面搜索”Alfred“关键字，免费下载，并安装。

## Alfred 偏好设置

### General 设置（即：通用设置）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_general_setting.png)

**Startup**: 开机是否自动启动。

**Alfred Hotkey**: 启动热键（快捷键），默认为 option + 空格，我这里设置为 双击 command 键。

**Where are you**: 这个设置比较特别，因为 Alfred 内置了常用网站搜索功能，在这里设置了你所在的国家后，Alfred 在搜索时会打开搜索网站对应国家的网站。

### Features 设置（即：特性设置）

这里是免费版的重点，Alfred 里所有的搜索功能都在这里设置。接下来我们一一介绍：

#### Default Results（默认结果）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_1.png)

**Essentials**：可设置搜索“应用程序”、“联系人”、“设置”、“Safari书签”。

**Extras**：可设置搜索“文件夹”、“文本文件”、“压缩文件”、“个人文档目录”、“图片”、“AppleScript”等其他文件。

**Unintelligent**：Search all file types 搜索所有文件类型。若勾选此项不但影响巡查速度，还混淆默认搜索结果。

**Search Scope**：设置 Alfred 查询时会搜索的文件夹范围，可自己添加和删除。

**Fallbacks**：若上面的查询搜索不到结果时，就会调用这里设置的网站或搜索引擎来进行进一步的查询。默认反馈结果为 Google、Amazon、Wikipedia 网页搜索。

> 注意：检索外置移动硬盘数据：如果需要 Alfred 也所能搜索外置移动硬盘中的文件、应用程序和元数据的话，请添加外置移动硬盘的目录或拖动文件夹到 Search Scope 中。排除 Library 文件夹：为了保证搜索结果的准确性和相关性，建议排除应用程序文件存放位置 ~Library。检索 Chrome 书签：Alfred 检索的书签是 Safari 中的数据，因此，如果你的主力浏览器是 Chrome 的话，则需要打开 Safari 后，通过文件 → 导入自 → 谷歌 Chrome 导入书签数据。

#### File Search（文件搜索）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_2.png)

**Quick Search**：快速搜索，勾选该选项后，我们可以使用‘（单引号）或者 Space（空格键）快速启用打开文件或者文件夹，功能类似于使用 Open + 关键字。

**Opening Files**：输入 open 打开文件或者文件夹。

**Revealing Files**：输入 find 查询文件或者文件夹的位置。

**Inside Files**：输入 in 查找文本文件内含有查询文字的文件（这个功能很强大啊）。

**File Tags**：输入 tags 查询含有查询 tags（标签） 的文件或者文件夹。

**Don‘t Show**：选择查询结果中不出现「邮件」、「书签」、「音乐」、「联系人」、「历史记录」等其它文件内容（注：如果需要更为复杂的结果过滤，则需要使用自定义结果过滤的 WorkFlow ）。

**Result Limit**：自定义显示结果个数——更多的结果意味着更大的灵活性（flexibility），而更少的结果以为这更高的性能（performance）。

#### Action（动作）

此设置需要付费版才能使用，这里就先不做介绍了。

#### Web Search（网页搜索)

这里当然是网站搜索的一些设置，我们可以使用 Alfred 默认的一些搜索功能，或者自己设置一些自定义搜索。图中可以看到已经设置了「亚马逊中国」、「亚马逊日本」、「Google」、「百度」、「BiliBili」、「Youku」等其它自定义查询。 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_4_1.png)

**Keyword**：搜索关键字。

**DisplayText**：为此搜索功能的标题。

**Custom**：有图标表示这个网页为用户自定义网页。

**Enabled**：是否启用。

点击又下方 “Add Custom Search” 按钮，可以添加自定义搜索，如： ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_4_2.png)  **Search URL**：网站查询的 URL，每个网站的查询 URL 可以先通过网站查询功能，然后查看浏览器的地址栏就能知道了。当然查询内容使用 {query} 变量来代替。

**Title**：标题，这个是设置在查询时 Alfred 查询主界面显示的提示文字。

**Keyword**：查询关键字，尽量使用简短容易辨识的文字。

**Validation**：有效性，这个是用来测试设置是否有效的。

#### Web Bookmarks（网页书签）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_5.png)

#### Clipboard History（剪贴板历史）

此设置需要付费版才能使用，这里就先不做介绍了。

#### Sinppets（片段）

此设置需要付费版才能使用，这里就先不做介绍了。

#### Calculator（计算器）

计算器这个就不多说了，主要有两个功能，一个就是直接输入简单的加减运算，一个就是输入 = 来输入复杂的计算，支持许多高级的数学函数。 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_8.png)

#### Dictionary（字典）

字典功能其实使用的是 Mac 系统自带的字典，可以设置使用的字典和查询关键字，输入 di + 关键字 来查询中英字典。 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_9.png)

#### Contacts（联系人）

此设置需要付费版才能使用，这里就先不做介绍了。

#### Muisic（音乐）

此设置需要付费版才能使用，这里就先不做介绍了。

#### 1Password（密码）

此设置需要付费版才能使用，这里就先不做介绍了。

#### System（系统）

这里主要是设置一些系统命令的关键字。建议将一些常用的系统命令、程序管理命令、盘符管理命令设置为剪短好记的语词。 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_13.png)

**常规系统命令**：屏幕保护程序（screen saver）、显示回收站（trash）、清空回收站（empty trash）、登出（logout）、睡眠（sleep）、锁定（lock）、重启（restart）、关机（shutdown）。

**程序管理命令**：隐藏（hide）、关闭（quit）、强制关闭（forcequit）、关闭所有应用程序（quitall）。

**盘符管理命令**：推出某个盘符（eject）、推出所有盘符（ejectall）、设置盘符黑名单。

#### Terminal（终端）

此设置需要付费版才能使用，这里就先不做介绍了。

#### Large Type（大字体）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_15.png)

#### Previews（预览）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_features_setting_16.png)

### Workflows 设置（即：工作流设置）

此设置需要付费版才能使用，这里就先不做介绍了。

### Appearance 设置

Alfred 主题设置，在这里我们可以找到更多预置的主题模式。如果这些主题你还是不够满意，可以选择自行创建或前往 [Packal](http://www.packal.org/theme-list) 下载。 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_appearance_setting.png)

**自行创建主题的方法**：点击界面下方的 “+” 按钮 并输入主题名称和创建者，然后 Alfred 会默认以当前选择的主题为模板，创建新的方案，最后就可以在左侧「预览区」进行自定义。（Tips：按住 CMD, Option, Control, Shift 或 fn 键会有更多选项哦）

什么是 [Packal](http://www.packal.org/theme-list)？简而言之，它就是一个集成了 Workflows 工作流程和 Themes 主题的平台。由于它专为 Alfred 服务，所以相关开发者会更加选择在这里发布自己的作品（及更新），用户也能获得最新的插件版本，而不是被动地关注来源地。因此建议每位 Alfred 用户都使用 [Packal](http://www.packal.org/theme-list)。

另外，点击界面下方的 “Options” 按钮，有更为详细的选项。例如：

*   Hide hat on Alfred window - 隐藏「小黑帽」图标。
*   visible result - 索引列表的显示数量。
*   也可以通过修改「How he acts」「Where he shows」参数调整其状态及位置。

## 特色功能

Alfred 的大部分功能是免费的，它们虽然比较基础，但也能帮助轻度用户完成不少操作。如果你想拓展出更多功能，那就建议你购买 [Powerpack](https://buy.alfredapp.com/) 包了。 ![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.pengzhenjin.top%2Fpost%2Fimage%2Falfred_powerpack.png)

完成购买后，可以解锁一项非常强大的特性：Workflow。作为扩展插件的主要来源，它不仅为 Alfred 培养了大批忠实粉丝，也博得了开发者的心，甚至还有人专为 Alfred 扩展搭建了公共平台进行收集（感兴趣的朋友可以 [点击这里](http://www.alfredworkflow.com/) 进行访问）。

## 注意事项

*   Alfred 虽然界面不适配简体中文，但实际上输入框是支持的。所以用户可以直接输入中文，不会报错。
*   若索引结果列表过于冗长，而目标也恰巧位列下方，那么此时千万不要盲目地继续/删除输入，只需按照右侧对应的「快捷键提示」在键盘上轻敲即可。
*   如果你正在使用的输入法会对 Alfred 产生「兼容性」方面的问题，建议在「偏好设置 - Force Keyboard」中选择原生输入法。
*   大部分情况下，只要 Alfred 遇到严重的问题，都可以通过「Clear」进行修复。点击 Advanced 页面下的「Clear Application Cache」「Rebuild OS X Metadata」和「Clear Knowledge」即可。

## 参考资料

[从零开始学习 Alfred：基础功能及设置](https://sspai.com/post/32979) [OS X 效率启动器 Alfred 详解与使用技巧](https://sspai.com/post/27900)

[查看原网页: www.pengzhenjin.top](https://www.pengzhenjin.top/archives/mac%E6%95%88%E7%8E%87%E7%A5%9E%E5%99%A8%E5%B7%A5%E5%85%B7%E4%B9%8Baifred%E8%AF%A6%E8%A7%A3#SGZGfHDP)