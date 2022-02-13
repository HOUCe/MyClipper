# 思源同步语雀 SiyuanYuque

[ld246.com](https://ld246.com/article/1631719629947)Clouder 2 个月前 via Linux

## SiyuanYuque 开发笔记

有了 AnkiSiyuan 的开发经验，大概会顺畅一些吧。

## 语雀 API

选择了 [yuque-py](https://link.ld246.com/forward?goto=https%3A%2F%2Fgithub.com%2FZheaoli%2Fyuque-py%2F) 库来避免重复劳动。

主要使用创建文档和更新文档。

语雀那边文档也有 ID，通过块属性写到思源中。

## 思源侧

将自定义属性 `yuque` 设置为 `true` 的文档块进行同步。

    SELECT *
    FROM blocks
    WHERE id IN (
        SELECT block_id
        FROM attributes AS a
        WHERE a.name ='custom-yuque' AND a.value = 'true'
    )  AND type='d' AND updated>'20210915181200'
    

获取到块 ID 后，根据是否在块属性中存在对应的语雀 ID 来决定更新还是创建。

这里注意一个处理：如果语雀 ID 失效，则重新创建。以后再说吧。#TODO#

* * *

整个开发过程还是比较轻松的……毕竟都是现有的东西，随便组合一下就行了。-
一共可能也就几十行代码。

[Github 仓库地址](https://github.com/Clouder0/SiyuanYuque "【净化】https://link.ld246.com/forward?goto=https://github.com/Clouder0/SiyuanYuque")

**注意使用了 1.3.5 预览版的新 API，旧版无法使用。**

[查看原网页: ld246.com](https://ld246.com/article/1631719629947)