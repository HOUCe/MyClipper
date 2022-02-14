# [Diigo、Pinboard 以及 Collamark 有何区别？如何选择？ - 知乎](https://www.zhihu.com/question/50767529/answer/125334446)

用Diigo好几年了。但官方也是三心二意，精力放在其他产品线上，Firefox扩展都N年没更新了。

Diigo和Collamark 区别，一言以蔽之：

就像Firefox和Chrome的区别：前者是瑞士军刀，大而全；后者是豪华跑车，小且快。看你适合哪一种。

如果你在Firefox上用Diigo，推荐几个技巧：

• 隐藏丑陋的工具栏。给命令'diigo.handle('bookmark', false);'绑定一个鼠标手势，就能快速加书签了

• 给侧边栏设定快捷键，再用AutoHotkey绑定为F3。这样，单键F3，就是Diigo侧边栏的开关了。不需要工具栏

• 用Stylish增加CSS，让Diigo Sticker固定位置，妈妈再也不怕我找不到便签了：

```
@-moz-document regexp(".*") {
    div.diigoHighlight.type_2 {
        position: fixed !important;
        top: 90px !important;
        right: 90px !important;
        left: unset !important;
        bottom: unset !important;
    }
}
```

• Diigo的Library页面，自动展开More Annotation...的脚本：

[Diigo Expand More Annotation](https://greasyfork.org/zh-CN/scripts/27821-diigo-expand-more-annotation)
