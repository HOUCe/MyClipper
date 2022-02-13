# [利用简悦导出到 hypothes.is 实现标注的共享与实时协作 - 知乎](https://zhuanlan.zhihu.com/p/384666922)

> hypothes.is 是一个网页标注工具，跟简悦在功能上有些「重复」，为什么简悦支持导入到 hypothes.is 呢？

1.  为了支持通过 hypothes.is 中转导入到 [readwise.io](https://readwise.io/i/kenshin) （ 在接入此功能时 Readwise 还未支持 API）
2.  标注协作（共享）

### 写给 hypothes.is 的用户

相比 hypothes.is 来说，简悦是基于阅读模式的标注方案，也就是说通过简悦生成的标注更加的「干净」。因为简悦不收集用户数据，所以你甚至可以将 hypothes.is 当作简悦标注的「数据库」来对待。

备注：简悦可以将稍后读和标注保存到本地任意位置。

### 什么场合需要标注共享

在使用此方式之前，我都是通过 Notion 做这个事情的，将需要的页面通过简悦导入到 Notion 然后共享给需要的成员，并在其中进行 Comment 操作。

但大多数时候我只是需要将标注以及备注内容共享给我的团队就可以了，尤其是在撰写文档的时候非常重要。

所以 Notion 的方式就显得太「复杂」了，后来在简悦社区用户的提示下我想到了此方式：

在简悦中标注，然后在 [hypothes.is](http://hypothes.is/) 生成的链接丢给有需要的成员或干脆使用 Shared Group 方案，即可。

下图是使用简悦标注，并在 hypothes.is 查看的效果：

![](https://pic4.zhimg.com/v2-1aec534076b85a51b289cfc0d706758b_b.jpg)

接下来看看如何配置。

### Hypothes.is 配置

进入 [https://hypothes.is/account/developer](https://hypothes.is/account/developer) 并复制 Token

![](https://pic2.zhimg.com/v2-b002f32123416d516795490d8594f3c9_b.jpg)

### 简悦 · 同步助手配置

[安装](http://ksria.com/simpread/docs/#/Sync?id=%E4%B8%8B%E8%BD%BD) 并 [验证](http://ksria.com/simpread/docs/#/Sync?id=%E9%AA%8C%E8%AF%81) 同步助手后，需要将上步得到的 Token 填入这里面并保存。

![](https://pic1.zhimg.com/v2-77d9b6e1e21bc267cea1c762144a99e8_b.jpg)

### 简悦扩展端的配置

配置 hypothes.is 的授权，位置在：选项页 → 授权管理 → 同步服务， 注意：不要选择 Public，而是选择一个你建立的 Group

![](https://pic1.zhimg.com/v2-915528d2995e47f313983a34bfffb6a4_b.jpg)

建立一个标注的自动化方案，按照下图所示方式设置

![](https://pic2.zhimg.com/v2-5c30e4c0cf082dffe4b9e7ad70af20cd_b.jpg)

### 如何使用

进入简悦的阅读模式后，进行标注操作即可，当操作成功后，稍等片刻即可在 `https://hypothes.is/groups/<你建立的 Group id>/simpread` 查看到，类似下图效果

![](https://pic2.zhimg.com/v2-92920d3faff87a4d9d593e28d02573e9_b.jpg)

### 如何共享

随便进入一个，按照下图所示点击这些不同的位置，就能打开

![](https://pic4.zhimg.com/v2-1245fde724fd874fa3dd9b33659bd0cf_b.jpg)

打开的链接类似 `https://hyp.is/*****/sspai.com/post/66883` 其中 `****` → 会对应一个唯一 ID

注意：任何得到这个链接的用户都可以查看，包括：在简悦生成的备注 / 标签均可同步到 hypothes.is

### 共享 Group

上面只是提到了共享某个具体标注，hypothes.is 还具有 Share Group 功能，如下图所示

![](https://pic3.zhimg.com/v2-46a7d4e88d5cd73cf68df86a56e88c46_b.jpg)

得到这个链接的人均可以可以看到此 Group 的全部内容。

### 注意事项

1.  _简悦支持_ **_文字_** **_图片_** **_代码段_** **_标注颜色_** **_标注样式_** **_标签_** **_备注_**_，但同步到 hypothes.is_ **_仅包含文字_**_，_**_标签_**_和_**_备注_**_。_
2.  在简悦中产生的标注会实时同步到 hypothes.is，包括：增删改的操作均可。
3.  在 hypothes.is 的改动无法「同步回」简悦，也就是说属于单向同步，即简悦 → hypothes.is
4.  任何用户**无需安装扩展端**，打开 `https://hyp.is/*****/sspai.com/post/66883` 后即可查看标注，但还是建议用户安装 hypothes.is 扩展端获取更好的效果。

### 后续

hypothes.is 能做的事情不仅如此，期待你的发现。

发布于 2021-06-28 13:23，编辑于 2021-06-28 13:27
