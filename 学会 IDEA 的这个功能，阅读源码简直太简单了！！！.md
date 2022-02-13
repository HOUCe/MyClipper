# [学会 IDEA 的这个功能，阅读源码简直太简单了！！！](https://mp.weixin.qq.com/s/tRYeIU_HEqZgJSOYeSmltw)

_来源：cnblogs.com/deng-cc/p/6927447.html_

最近正好也没什么可忙的，就回过头来鼓捣过去的知识点，到Servlet部分时，以前学习的时候硬是把从上到下的继承关系和接口实现记得乱七八糟。

这次利用了IDEA的diagram，结果一目了然，也是好用到炸裂，就此分享。

## 1、查看图形形式的继承链

在你想查看的类的标签页内，点击右键，选择 Diagrams，其中有 show 和 show ... Popup，只是前者新建在标签页内，后者以浮窗的形式展示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucibeib4MH66MJdTJ246PV8g58BO4IhWM5KLFEic8uQA3TeYw41uTJ0HXYGrEb3ljpolc4daX8X0tbeA/640?wx_fmt=png&tp=png&wxfrom=5&wx_lazy=1&wx_co=1)

实际上，你也可以从左边的项目目录树中，对你想查看的类点击右键，同样选择Diagrams，效果是一样的：

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucibeib4MH66MJdTJ246PV8g5wDEJa4wl6icfI5MI5rWvshmBzePoicmXMqVghQpbcciaVhG7miangJ7abQ/640?wx_fmt=png&tp=png&wxfrom=5&wx_lazy=1&wx_co=1)

然后你就会得到如下图所示的继承关系图形，以自定义的Servlet为例：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

显而易见的是：

-   **蓝色实线箭头**是指继承关系
    
-   **绿色虚线箭头**是指接口实现关系
    

## 2、优化继承链图形，想我所想

## 2.1 去掉不关心的类

得到的继承关系图形，有些并不是我们想去了解的，比如上图的Object和Serializable，我们只想关心Servlet重要的那几个继承关系，怎么办？

简单，删掉。点击选择你想要删除的类，然后直接使用键盘上的delete键就行了。清理其他类的关系后图形如下：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.2 展示类的详细信息

有人说，诶，这怎么够呢，那继承下来的那些方法我也想看啊？简单，IDEA通通满足你。

在页面点击右键，选择 show categories，根据需要可以展开类中的属性、方法、构造方法等等。当然，第二种方法也可以直接使用上面的工具栏：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

然后你就会得到：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

什么，方法里你还想筛选，比如说想看protected权限及以上范围的？简单，右键选择 Change Visibility Level，根据需要调整即可。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

什么，你嫌图形太小你看不清楚？IDEA也可以满足你，按住键盘的Alt，竟然出现了放大镜，惊不惊喜，意不意外？

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.3 加入其他类到关系中来

当我们还需要查看其他类和当前类是否有继承上的关系的时候，我们可以选择加其加入到当前的继承关系图形中来。学习资料：[Java进阶视频资源](http://mp.weixin.qq.com/s?__biz=MzU2MTI4MjI0MQ==&mid=2247506972&idx=2&sn=790959d9ee2b74e0d0c8a0a5395bc665&chksm=fc79b1b2cb0e38a498933a034926df45b51524b044762583488d498ae2112c06cf35d412fb3b&scene=21#wechat_redirect)

在页面点击右键，选择 Add Class to Diagram，然后输入你想加入的类就可以了：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

例如我们添加了一个Student类，如下图所示。好吧，并没有任何箭头，看来它和当前这几个类以及接口并没有发生什么不可描述的关系：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 2.4 查看具体代码

如果你想查看某个类中，比如某个方法的具体源码，当然，不可能给你展现在图形上了，不然屏幕还不得撑炸？

但是可以利用图形，或者配合IDEA的structure方便快捷地进入某个类的源码进行查看。

双击某个类后，你就可以在其下的方法列表中游走，对于你想查看的方法，选中后点击右键，选择 Jump to Source：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

在进入某个类后，如果还想快速地查看该类的其他方法，还可以利用IDEA提供的structure功能：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

选择左侧栏的structure之后，如上图左侧会展示该类中的所有方法，点击哪个方法，页面内容就会跳转到该方法部分去

 ![](http://mmbiz.qpic.cn/mmbiz_png/Q6nwSK9rE4zp8Z3DFMduStcAD9gQMR6ibKgWAeicIfCSCFXFqGsX9icT5esTUDlocRlZXZkdkgoFPry1b4paQydjA/640?wx_fmt=png&wxfrom=200)   我的外卖领券助手 ， 交易担保 ， 放心买 ，  外卖三倍券  小程序
