---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=946#/detail/pc?id=7624
author: 
---

# [Vue.js 3.0 核心源码解析 - 前百度、滴滴资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=946#/detail/pc?id=7624)


![](https://s0.lgstatic.com/i/image6/M00/41/5A/Cgp9HWCrqJeAfHpMAAEhcFmUStA456.png)

Vue.js 3.0 核心源码内参

HuangYi

Zoom 前端架构师，前百度、滴滴资深技术专家

专属班主任

开篇词

开篇词 | 解析 Vue.js 源码，提升编码能力

模块一：直击 Vue.js 核心组件的实现

模块一导读 | 组件的实现：直击 Vue 核心的实现

01 | 组件渲染：vnode 到真实 DOM 是如何转变的？

02 | 组件更新：完整的 DOM diff 流程是怎样的？（上）

03 | 组件更新：完整的 DOM diff 流程是怎样的？（下）

模块二：学会新设计 Composition API

模块二导读 | 逻辑复用最佳实践：Composition API

04 | Setup：组件渲染前的初始化过程是怎样的？

05 | 响应式：响应式内部的实现原理是怎样的？（上）

08 | 侦听器：侦听器的实现原理和使用场景是什么？（上）

06 | 响应式：响应式内部的实现原理是怎样的？（下）

09 | 侦听器：侦听器的实现原理和使用场景是什么？（下）

10 | 生命周期：各个生命周期的执行时机和应用场景是怎样的？

模块三：编译过程和背后的优化思想

12 | 模板解析：构造 AST 的完整流程是怎样的？（上）

模块三导读 | 编译和优化：了解编译过程和背后的优化思想

13 | 模板解析：构造 AST 的完整流程是怎样的？（下）

14 | AST 转换：AST 节点内部做了哪些转换？（上）

15 | AST 转换：AST 节点内部做了哪些转换？（下）

16 | 生成代码：AST 如何生成可运行的代码？（上）

17 | 生成代码：AST 如何生成可运行的代码？（下）

模块四：探索更多实用特性背后的实现原理

模块四导读 | 实用特性：探索更多实用特性背后的原理

18 | Props：Props 的初始化和更新流程是怎样的？

21 | v-model：双向绑定到底是怎么实现的？

模块五：学习 Vue 内置组件的实现原理

模块五导读 | 内置组件：学习 Vue 内置组件的实现原理

22 | Teleport 组件：如何脱离当前组件渲染子组件？

23 | KeepAlive 组件：如何让组件在内存中缓存和调度？

24 | Transition 组件：过渡动画的实现原理是怎样的？（上）

25 | Transition 组件：过渡动画的实现原理是怎样的？（下）

特别放送：研究 Vue 官方生态的实现原理

特别放送导读 | 研究 Vue 官方生态的实现原理

26 | Vue Router：如何实现一个前端路由？（上）

27 | Vue Router：如何实现一个前端路由？（下）

2021/05/24 HuangYi

Vue.js 除了提供了组件化和响应式的能力，以及实用的特性外，还提供了很多好用的内置组件辅助我们的开发，这些极大地丰富了 Vue.js 的能力。

那么，既然我们平时经常用到这些内置组件，了解它的实现原理可以让我们更好地运用这些组件，遇到 Bug 后可以及时定位问题。同时 Vue.js 内置组件的源码，也是一个很好的编写组件的参考学习范例，我们可以借鉴其中的一些实现为自己所用。

既然学习内置组件有那么多的好处，那么就让我们马不停蹄，一起来探索内置组件的秘密吧！

精选留言

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABwAAAAcCAMAAABF0y+mAAABDlBMVEUAAAAAAAArKyscHBwrKyswMDAtLS0xMTEuLi4rKyszMzMzMzMyMjIvLy8zMzMxMTEvLy8tLS0zMzMyMjIxMTEwMDAyMjIwMDAzMzMxMTEwMDAxMTEyMjIzMzMyMjIzMzMxMTEyMjIyMjIyMjIyMjIyMjIyMjIzMzMyMjIzMzMyMjIyMjIzMzMyMjIzMzMyMjIyMjIzMzMyMjIyMjIzMzMyMjIyMjIyMjIzMzMyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIzMzMzMzMyMjIzMzMzMzMzMzMzMzMyMjIzMzMyMjIzMzMzMzMzMzMyMjIzMzMzMzMzMzMyMjIzMzMzMzMyMjIzMzMMFbxYAAAAWXRSTlMAAwYJDBARFRYYHiMkJigqKy0tMzk7PUBBQ0VJTFBSVVlbXGFla3B0dXh/hYeKjY+QkZSVlpiao6ausbK3u73DxcfIys3O0NPY3N3g4uXm6Ovu8fX3+Pr7/Z2GrlIAAAEvSURBVCjPdZJrT8JAEEVvC4ggqAiKiq0vlLa8tIoWBQFBRK0yIrTM//8jfighLWzPh8nNnsnMZrPAAlkxe+Q41DMVGUGiOrHbbzYazb7LpEf9TiVuKzEvx5Q2k7pUUo0HOX9vbsA1aZFNvl9ZI9+x6SWdq1ijyjoApGcWBFizNIAWxUUyTi0gwyUIKXEGZTchlgm3jG4HAAq2XQhWoNMF1QHAZraDFagTHCNMGg4mlbCxlT+MLIRgjWB9hTjp28IZ74vlIV8gQk9i2fqNABqfiJzKVwDk4Xhr3e3QmwQAKfrcXnUp+yfppSzRadCdT2hv2TfkS5/Kv3Av6fsWjgHk1d3NZFa9HfFYk/xzpkb0gT3mr9eR4JLp88f85qCoacXj2NrNp2wfhb0x3h83BKf/ogM3zcQrR7gAAAAASUVORK5CYII=) 写留言

学习知识要善于思考，思考，再思考。—爱因斯坦
