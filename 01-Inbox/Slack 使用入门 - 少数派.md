# Slack 使用入门 - 少数派

[sspai.com](https://sspai.com/post/47602)Hum

本文整理并补充了 [@文刀漢三](https://www.weibo.com/wendaohansan?is_all=1)、[@Minja](https://www.weibo.com/u/2440387022)、[@沨沄极客](https://www.weibo.com/u/1691644702) 之前在 Power+ 群与 Matrix 群分享的 Slack 使用技巧。它不是一篇全面系统的文章，只是为了能让入门者可以快速上手 Slack 的索引性的帖子。

注：本文的桌面版界面为 macOS，移动端界面为 iOS。

简单来说，是因为 Slack 通过它的频道、回复、通知体系，将团体沟通的干扰降低到最小，同时又可以做到不错过重要提示。

**一、频道体系**

公司的不同部门或者同一团体间的不同话题，都可以开设一个单独的频道。如果不喜欢或者没必要参与某个频道内的发言，可以单独屏蔽这个频道或者退出，而不需要退出整个团体。

Slack 的频道分为开放频道和私密频道，前者人人可见可加入，后者普通用户不可见，需要邀请加入。

**二、回复体系**

Slack 支持 3 种回复，评论、转发以及表情。

评论的价值是：当大家在讨论一个比较大的问题时，其中几个人针对某句发言进行分支讨论。这个分支讨论不会出现在主体版面上，而是需要单独打开这条发言才能看到。这样就不会影响到大讨论的发展。

转发的价值是：可以将某句发言转发到其他频道或者个人，并附上评论，类似于 Twitter 的 Quote 或者微博的转发。

表情的价值是：不再因为一些最浅度的回复影响他人（包括发言者）。比如当我们支持一个人的发言，我们可以给他附上 👏 （鼓掌的表情），而不必每个人都回一句「支持」，干扰其他人。

**三、通知体系**

Slack 的通知设定比较全面，使我们能够在减少干扰的前提下不错过重要信息。比如我们可以全局关闭通知，只对 at 到自己的，或是提到某个关键词的时候，再弹出提示。

**四、开放性**

Slack 是一个开放性很强的服务，有一定的平台属性。它已经接入不少国外主流服务，比如 Todoist、IFTTT。没有你想用的服务也可以自制 bot，或者用别人做好的 Bot。

总的来说，它是目前在减少多人讨论中的干扰这个方面处理得最好的服务。每个人都可以按自己的需求，控制自己的被干扰程度，并可以减少对别人的干扰。

## 加入频道

**桌面端**：点击左侧边栏里的「CHANNELS」就可以浏览所有公开频道，或者也可以使用快捷键 `Shift-Command-L` 来进入频道列表。随后选择你想加入的频道。

加入频道（桌面端）

**移动端**：轻触左侧边栏的「CHANNELS」后面的「加号（+）」来浏览所有公开频道，选择你想加入的频道。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2Fc60091d1653a7bf1ecd2762d0a9b90d1.png)

加入频道（移动端）

## 修改个人信息

进入一个团体大家一般会修改 ID 和头像。Slack 支持大家进行任意的自定义，但还是建议大家把 ID 和头像设得和自己其它社交平台一致，这样易于辨别身份。

**桌面端**：点击左上角的 Slack Workspace 名，随后可以看到「Profile & account」，点击后会弹出右边栏，再在里面点击「Edit Profile」即可。

编辑个人信息（桌面端）

**移动端**：轻触界面右上角的菜单按钮，在弹出的右边栏中选择「Edit Profile」即可。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F874d34a9295ab403c16a18a3e9b89d0e.png)

编辑个人信息（移动端）

## 与信息互动的方式

Slack 中针对每条信息的互动方式有 4 种：

1.  评论：跟帖后所有讨论都在这一个消息下被折叠，避免「刷屏」。
2.  转发：可以理解为微博的转发功能，好处是可以跨频道转发。
3.  附上表情：用于表达心情，还能作为投票手段，鼠标放在表情上（移动端长按表情）可以看到谁选了哪个表情。
4.  收藏：收藏信息，以便日后统一查看。

**桌面端**：将鼠标悬停在消息上，即可看见该菜单。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2Fb3d324329d8350850ce1a6ada848921e.jpg)

与消息互动（桌面端）

**移动端**：轻触你想互动的消息进入该消息的界面即可看见该菜单。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F4c850838dab9c80f2b8212fc136347b3.jpg)

与消息互动（移动端）

冷知识：在某次颁奖典礼中，少数派的作者们发现：一条消息最多可以添加 25 个表情。

## 修改/删除消息

Slack 支持修改后修改和删除消息。不过普通用户只能删除、编辑自己的信息，而管理员可以删除他人信息。管理员也能禁止普通用户删除、编辑自己的信息。

**桌面端**：在信息互动按钮的最后一项——**More Actions（更多操作）**里有编辑和删除信息的选项。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F30b03bdb98d025966e8d5e7f90b8a4fb.png)

编辑和删除消息（桌面端）

**移动端**：长按信息，再弹出菜单内有 **Edit Message（编辑信息）和Delete Message（删除信息）**的选项。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F6edb45e575520079611fe736fccb0258.png)

编辑和删除消息（移动端）

## 通知

Slack 的通知设置灵活，主要分为以下两个类别：

1.  全局通知设定，包括：通知所有新信息通知关键字和 at全都不提示
2.  频道通知设定，包括：屏蔽整个频道（如果开了屏蔽整个频道就不会看到剩下到选项）在收到 @channel （相当于 @所有人）时不提示通知所有新信息通知关键字和 at全都不提示

### 全局通知设定的位置

**桌面端**：点击左上角的 Slack Workspace 名，随后可以看到「Preference」，点击后会弹出右边栏，再在里面点击「Edit Profile」即可。

全局通知设定（桌面端）

**移动端**：轻触界面右上角的菜单，在右边栏中选择「Settings」，再选择「Notifications」即可看到选项。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F1620227b7da622bfeec0234c342d3b6d.png)

全局通知设定（移动端）

### 频道通知设定的位置

**桌面端**：点击频道名右边的齿轮（⚙️）按钮，在弹出选项中选择「Notification preferences」即可。

频道通知设定（桌面端）

**移动端**：轻触频道名，在弹出菜单中选择「Notification」即可。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2Ffdbe13345f04da3742ade31e1f544299.png)

频道通知设定（移动端）

注：关键字之间用半角逗号 `,` 分割。

## 类 Markdown 的语法

Slack 采用了一套与 Markdown 类似的语法，来帮助我们实现简单的格式排版。

共有以下六种样式：

\*需要加粗的文字\* \_需要斜体的文字\_ ~被删除的文字~ \`行内代码\` \`代码块\`\`\` >这是一段引用 

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F109a8ba7202ed7cdbf66dd35bd9e2fa8.png)

Slack 中的类 Markdown 语法

之所以说是「类 Markdown」是因为 Slack 的语法并不完全和 Markdown 一致。比如它的加粗是两边各一个星号 `*加粗*` 而不是各两个星号 `**加粗**` 。

注：在使用这些语法时，被标记的字段的前后**都需要加空格**，不然特殊格式会失效。

## 置顶信息

Slack 可以在频道内实现置顶内容，但是这个置顶内容是对该频道内所有成员有效的。所以一般由管理员置顶一些所有群成员需要知道的信息，群规则等。

**桌面端**：将鼠标悬停到消息上，选择最后一项菜单按钮，即可看到「Pin to #频道名」的选项。

置顶消息（桌面端）

**移动端**：轻触消息，进入消息界面，再轻触右下角的菜单按钮，在弹出菜单中会出现「Pin message」。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2Faec5ea029964db1b236986ec64c0c8e9.png)

置顶消息（移动端）

### 显示被置顶的信息

**桌面端**：桌面端点击中间上方的图钉小图标查看，点击消息后会直接跳到该消息。

显示被置顶的消息（桌面端）

**移动端**：轻触界面左上角的频道名，在弹出菜单中选择「Pinned」，即可看到该频道中被置顶的消息。轻触消息后会跳转到该消息。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F0af10eb2565fc67f84bcaadb000b3e16.png)

显示被置顶的消息（移动端）

### 显示被收藏的信息

**桌面端**：在主界面右上角有个空心的五角星（☆）按钮，点开它即可看到被收藏的消息。

显示被收藏的消息（桌面端）

**移动端**：轻触主界面右上角的菜单，在右边栏中选择「Starred Items」，即可看到被收藏的消息。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2Fc963deebc86c36bc4f419a4a6bb83374.png)

显示被收藏的消息（移动端）

## 收藏联系人和频道

除了收藏信息，你也可以在 Slack 里收藏联系人和频道，使其显示在侧边栏的明显位置。

**桌面端**：在界面的左上角，栏目名的下方有空心的五角星（☆）。点击它即可收藏频道/联系人。随后该频道/联系人将在侧边栏被置顶。

收藏联系人和频道（桌面端）

**移动端**：轻触界面左上角的频道名，在弹出菜单中轻触右上角的空心五角星（☆）即可收藏频道/联系人。随后该频道/联系人将在侧边栏被置顶。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fcdn.sspai.com%2F2018%2F10%2F20%2F2f51e8c7ae9b1406321466da88401ac0.png)

收藏联系人和频道（移动端）

## 快捷键

在 Slack 里，不论桌面版还是移动版，使用快捷键 `⌘Command - K` 可以打开一个类似 Spotlight 的搜索窗，输入关键词就能快速跳转到用户私信（direct message）、频道或者已经添加的机器人。

快捷键展示

## 各工具规则设置

最后，为了大家可以更顺畅地访问 Slack， [@沨沄极客](https://www.weibo.com/u/1691644702) 帮大家总结了在各个系统中[提高 Slack 访问速度的设置方法](https://app.simplenote.com/p/V1lRXw)。

[查看原网页: sspai.com](https://sspai.com/post/47602)