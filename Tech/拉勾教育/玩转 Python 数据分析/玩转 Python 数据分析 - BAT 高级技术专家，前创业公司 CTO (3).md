---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134
author: 
---

# [玩转 Python 数据分析 - BAT 高级技术专家，前创业公司 CTO - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=820#/detail/pc?id=7134)


![](https://s0.lgstatic.com/i/image6/M00/29/69/Cgp9HWBhQ4uAQw-1AAAiBVxkCQk813.png)

开篇词

课前准备 | 搭建一个高效的 Python 开发环境

加餐 | VS Code 新版 Notebook 使用指南

兵马未动，粮草先行：获取用于数据分析的数据集

07 | 下载网页：如何使用模拟请求下载真实的网页？

08 | 提取数据：如何从网页中提取感兴趣的内容？

09 | 保存数据：如何将爬取的数据保存成 CSV 格式

10 | 实战：手把手教你构建国产电视剧评分数据集

数据处理的银弹：用 pandas 加载、浏览和修改数据

11 | 文件处理：如何读取多种文件（csv/excel）的数据？

12 | DataFrame：如何以表格的形式查看和操作数据？

16 | 案例实战：如何用 Python 分析电商用户行为？

深挖数字背后的逻辑：用 NumPy 进行统计、预测分析

18 | 基础统计：如何统计数据的均值、方差等特征？

可视化你的分析思路：用 Matplotlib 做各类图表展示你的数据

22 | 散点图与线图：如何展示不同特征之间的相关性？

23 | 直方图、条形图和饼图：如何分析数据分布与占比？

24 | 图像的脊柱、注解和图例：如何画出更专业的图表？

26 | 案例实战：用图例可视化用户行为分析和喜好预测过程

综合项目实战篇

27 | 初识 EDA：全球新冠肺炎确诊病例趋势分析

28 | AI 落地实战：训练通用电影票房预测模型

2021/05/20 千帆

在课前准备课时，有同学反映下载的版本和我们“课前准备”课时的版本不一样，导致界面和我们的截图也不一致，为了让大家用新版的版本也可以正常使用。现做加餐特以说明。

近期，VS Code 团队计划推出新版的 Notebook 插件，提供了相比之前的版本更简洁的 UI 和更好的体验，功能基本上是一致的。由于改版较大，所以这里针对性地说一下新版的用法。

新版界面，如下所示。

![图片1.png](https://s0.lgstatic.com/i/image6/M00/40/B0/CioPOWCmKMWAETJDAALxYbbRwnw089.png)

主操作区从左到右的功能依次是：

-   执行全部
    
-   清空输出
    
-   重启 Jupyter 内核
    
-   查看当前变量
    
-   导出
    
-   分栏
    

当我们的鼠标浮动在 Cell 之上时，便会出现 Cell 操作的相关按钮，如下图所示。

![图片2.png](https://s0.lgstatic.com/i/image6/M01/40/A8/Cgp9HWCmKL-AdxQEAAHr6akyeN4433.png)

Cell 操作区的功能，从左到右依次是：

-   执行上一个 Cell
    
-   执行下一个 Cell
    
-   拆分 Cell
    
-   更多菜单
    
-   删除 Cell
    

有了执行上一个和下一个 Cell 的功能，那执行当前 Cell 呢？就是 Cell 左边的三角形按钮，如下图所示（默认快捷键变为 CTRL + Enter）。

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M00/3F/3E/CioPOWCc5g-ATMxCAACrg1JdTgQ642.png)

新版本的介绍就到这里了，相信同学们现在可以用新版本继续往下学习了！

精选留言

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAMAAABF0y+mAAABDlBMVEUAAAAAAAArKyscHBwrKyswMDAtLS0xMTEuLi4rKyszMzMzMzMyMjIvLy8zMzMxMTEvLy8tLS0zMzMyMjIxMTEwMDAyMjIwMDAzMzMxMTEwMDAxMTEyMjIzMzMyMjIzMzMxMTEyMjIyMjIyMjIyMjIyMjIyMjIzMzMyMjIzMzMyMjIyMjIzMzMyMjIzMzMyMjIyMjIzMzMyMjIyMjIzMzMyMjIyMjIyMjIzMzMyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIzMzMzMzMyMjIzMzMzMzMzMzMzMzMyMjIzMzMyMjIzMzMzMzMzMzMyMjIzMzMzMzMzMzMyMjIzMzMzMzMyMjIzMzMMFbxYAAAAWXRSTlMAAwYJDBARFRYYHiMkJigqKy0tMzk7PUBBQ0VJTFBSVVlbXGFla3B0dXh/hYeKjY+QkZSVlpiao6ausbK3u73DxcfIys3O0NPY3N3g4uXm6Ovu8fX3+Pr7/Z2GrlIAAAEvSURBVCjPdZJrT8JAEEVvC4ggqAiKiq0vlLa8tIoWBQFBRK0yIrTM//8jfighLWzPh8nNnsnMZrPAAlkxe+Q41DMVGUGiOrHbbzYazb7LpEf9TiVuKzEvx5Q2k7pUUo0HOX9vbsA1aZFNvl9ZI9+x6SWdq1ijyjoApGcWBFizNIAWxUUyTi0gwyUIKXEGZTchlgm3jG4HAAq2XQhWoNMF1QHAZraDFagTHCNMGg4mlbCxlT+MLIRgjWB9hTjp28IZ74vlIV8gQk9i2fqNABqfiJzKVwDk4Xhr3e3QmwQAKfrcXnUp+yfppSzRadCdT2hv2TfkS5/Kv3Av6fsWjgHk1d3NZFa9HfFYk/xzpkb0gT3mr9eR4JLp88f85qCoacXj2NrNp2wfhb0x3h83BKf/ogM3zcQrR7gAAAAASUVORK5CYII=) 写留言

学习知识要善于思考，思考，再思考。—爱因斯坦
