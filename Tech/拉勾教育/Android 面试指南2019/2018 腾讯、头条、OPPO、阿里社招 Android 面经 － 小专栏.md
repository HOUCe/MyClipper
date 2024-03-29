_注：本文适合工作三年以内的初、中级工程师观看，大佬们可以关掉了。_

## 自我介绍

本人 211 本硕，软件工程专业，研究生时期研究的是机器学习，工作选择的是深圳的平安科技，从事 Android 开发，到如今刚好工作满两年。不想满足于现状，就利用闲暇时间好好学习，天天向上，向腾讯、头条、OPPO、阿里投递了简历。

## 面试前准备

2018 年年后，公司的 App 已步入稳定迭代阶段，个人的学习时间也很充裕了，就下定决心准备跳槽，目标主要瞄准一些大厂。  
然而深圳的互联网大厂机会没有北京那么多，尤其是对于初级工程师的选择比较少，目前了解到腾讯视频，QQ 音乐，腾讯微视在招聘，阿里、百度、今日头条侧重招聘高级及资深的 Android 开发，Oppo 招聘力度很大，但是要求学历 211、985，其他的多是一些金融互联网公司，还有一些创业公司。  
从 4 月份开始学习，一直学习到 7 月末，这段时间主要从以下几方面着手学习：

-   复习 Java 基础，如 HashMap、多线程技术、synchronized、Lock、equals、hashCode, 垃圾回收算法等等
-   刷算法，手写剑指 offer 上的算法，leetCode 的 easy 难度的刷了 100 多个，了解动态规划、贪心、分治等算法的基本思想
-   复习 Android 基础，任教主的《Android 开发艺术探索》是本好书，不看 3 遍不敢去找工作。其次，简书大佬 JasmineBen 提供的 Android 面试题资料也给予了很大帮助
-   梳理这两年的开发工作，解决的疑难问题。
-   复习过程中不仅要理解掌握，还要及时整理，我前前后后写满了三个笔记本，有精力的更建议去写写博客。

## 面试经历奉上

### Oppo（3 轮技术面，Offer 拿到）

#### 一面

-   自我介绍，聊聊简历上写的项目
-   MVC、MVP 的区别
-   rxjava 的线程切换原理
-   listView 的卡顿优化、资讯流懒加载如何实现
-   序列化前后对象有何区别
-   Binder 通信原理
-   插件化开发流程，插件化优势，插件化开发中遇到的问题以及如何解决的

#### 二面

-   自我介绍，项目介绍
-   Glide 的优势，它是如何绑定生命周期的
-   listView 如何优化，listView 和 RecyclerView 的区别，二者的缓存逻辑
-   提高 app 安全性的方法
-   View 的绘制流程
-   插件化原理

#### 三面

-   自我介绍，项目介绍
-   为什么选用插件化，插件化框架的比较，梳理插件化的架构
-   App 的启动流程
-   自己的优势和劣势

Oppo 对学历要求比较高，必须是 211、985 的，面试过程中主要针对简历面试，没有问算法和数据结构，薪酬很不错。

### 腾讯视频（4 轮技术面，Offer 拿到）

#### 一面

-   自我介绍，项目介绍
-   如何处理 App 启动流程优化
-   View 的性能优化
-   MVC 到 MVP 的架构重构流程

#### 二面

-   自我介绍，项目介绍
-   为什么用 Glide，它的优势
-   HashMap 的原理，ConCurrentHashMap 的原理
-   View 的绘制流程
-   Handler 的工作原理
-   LeakCanary 原理，内存泄露的分析流程
-   进程间通信方式，Binder 原理
-   垃圾回收原理以及回收策略
-   Java 中的强、软、弱、虚引用
-   算法：排序，要求时间复杂度是 O(n)，例如 1，4，5，2，7，6，9。如果数列是 10，102，99，10000，825，1003030 如何排序

#### 三面

-   View 的性能优化，页面卡顿的原因
-   TCP、IP 各自所在的网络层次，IP 报文中的内容
-   https 的对称加密和非对称加密
-   指针占几个字节
-   自己的优势和劣势
-   对开发流程的理解
-   平时写博客吗，喜欢看哪些技术文章
-   为什么离职找工作
-   切饼问题：1 刀切 2 块，2 刀切 4 块，10 刀最多切几块。
-   追击问题：2 辆火车相向同时出发，一个从 A 点出发，速度是 30km/h, 一个从 B 点出发，速度是 20km/h，A、B 之间的距离是 S，同时一只鸟也从 A 点出发，速度是 50km/h，鸟在两辆火车间折返飞行，问三者相遇时，鸟飞的总路程。

#### 四面

-   梳理你遇到的最严重的问题，以及如何解决的
-   在白板上写出对于 View 的绘制和优化流程
-   算法：给定一段已排好序的数列，数列中元素有可能是重复的，找出数列中目标值的最小索引，要求不能用递归，只能用循环。
-   称重问题：10 个箱子，每个箱子里都有若干砖头，其中有一个箱子里每个砖头重 9 克，其他的箱子里的砖头每个重 10 克，给你一个秤，要求只能用一次称重找出砖头重 9 克的箱子。

腾讯面试对于基本功很重视，算法和数据结构也不难，很喜欢问一些智力题，重视面试者分析问题，解决问题的能力。

### 今日头条（三面挂）

#### 一面

-   算法：连续子数组的最大和
-   了解哪些设计模式，手写单例设计模式
-   View 的位置参数有哪些，left，x，translationX 的含义以及三者的关系
-   View 的绘制流程，onMeasure，onLayout 的计算流程，MeasureSpec 的几种模式
-   处理滑动的几种方式，Scroller 滑动的原理
-   自定义 View 的流程，自定义 View 需要注意的问题，例如自定义 View 是否需要重写 onLayout，onMeasure
-   invalidate、postInvalidate、requestLayout 的区别

#### 二面

-   TCP、UDP 区别，拥塞控制原理，视频传输为什么用 UDP，UDP 丢包会产生什么问题，如何处理数据包的发送速度和带宽的关系
-   Touch 事件的分发流程
-   类的加载机制，为什么用双亲委派模型，java 是否支持多态
-   插件化 Activity 生命周期的管理，资源的访问原理
-   算法：Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.  
    You have the following 3 operations permitted on a word:  
    1、Insert a character  
    2、Delete a character  
    3、Replace a character  
    Input: word1 = "horse", word2 = "ros"  
    Output: 3  
    Explanation:  
    horse -> rorse (replace 'h' with 'r')  
    rorse -> rose (remove 'r')  
    rose -> ros (remove 'e')

#### 三面

-   算法：int\[] insert(int\[] arr,int len,int cap,int pos,int value)，给定一个数组，例如 {1，2，3，空，空}, 该数组的 len=3，容积 cap=5，插入位置 pos 是 1，插入的值是 5，结果是 {1，5，2，3，空}，返回插入后的数组（考虑扩容）；
-   App 的启动优化流程
-   实现一个库，完成日志的实时上报和延迟上报两种功能，该从哪些方面考虑
-   是否使用过 Flutter 开源框架

今日头条是三轮视频面试，面试中对于算法很看重，问题也很刨根问底，很考验面试者对于实际问题的分析和解决能力。自己的算法功底还是不足，对于技术研究的还不够深入，工作经验也有限，面试没通过也是意料之中，但是还是很珍惜今日头条给予的面试机会。

### 阿里搜索事业部（一面通过，二面 hr 通知简历不符）

#### 一面

-   View 的性能优化，listView 的卡顿优化，资讯流懒加载实现。
-   App 的启动流程优化
-   组件化开发流程，ARouter 路由协议的原理分析
-   插件化优势，插件化加载 Activity 原理，加载资源原理
-   插件化首次启动耗时长，如何优化
-   如何处理几百人的团队分工
-   你对淘宝 App 的使用体验，从哪些方面优化淘宝 App

深圳的阿里搜索事业部很看重工作经验，问的问题不难，但是很有深度，而自己的经验不足，能力有限，hr 中止了后续的面试。

### 招商基金（一面通过，放弃之后的面试）

#### 一面

-   Handler 工作原理
-   View 的绘制原理
-   自定义 View 的流程
-   如何优化 WebView，WebView 的泄露
-   native 和 js 交互流程

招商基金听猎头说待遇很可观，对于学历比较看重，但是不习惯金融公司的氛围，就放弃了后续的面试流程。

## 总结

-   初级工程师一定要对 Java 基础、Android 基础有深入的理解和掌握
-   基本的数据结构必须要掌握，算法要早做准备，有精力一定要刷刷剑指 offer、leetCode
-   要多看优秀的开源框架，例如 rxjava、Glide、LeakCanary，OkHttp 等等
-   简历上的内容既要有深度，也要有广度，面试官一定会文简历上的内容
-   面试遇到不会的问题，千万不要说不会，要将问题进行拆解，尽量回答自己知道的，这样面试官才会帮助你，指点你
-   面试前尽量做好充分准备，这样面试大厂才会信心充足，即使没有通过，自己在大厂留下的面试经历也不会太差，以后再面试的机会也更大
-   投简历时，尽量通过猎头或者朋友了解到目标公司有哪些部门正在招聘，自己更擅长哪个领域的开发工作，不要随意投递。
-   心态端正，不要焦虑，回答问题时语速放缓，条理要清晰

以上所有内容，就是我从面试准备到参加面试的流程和理解，谢谢大家花出宝贵时间观看。
