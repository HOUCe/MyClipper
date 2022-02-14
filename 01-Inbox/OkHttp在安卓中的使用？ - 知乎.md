# [OkHttp在安卓中的使用？ - 知乎](https://www.zhihu.com/question/36989864/answer/2298081774)

2015/11/16

\-----------------------------------------------------------------------------------------------------------------

[lsxiao/ZhihuDailyRRD · GitHub](https://github.com/lsxiao/ZhihuDailyRRD)

2015/11/6 15:20

\-----------------------------------------------------------------------------------------------------------------

忙得很，抽空规范了下注释，小改了下布局，把5.0的视差嵌套滚动弄上去了，加上了日报的配图。

![](https://pic3.zhimg.com/50/726eb33671c3828f2a6e15ff23977192_720w.jpg?source=1940ef5c)

2015/11/4 00:33

\-----------------------------------------------------------------------------------------------------------------

一个RRD的例子基本上写好了，只有两个简单的，查看当日新闻列表，和查看详细新闻功能(没有主题日报)。再改改就放GitHub

![](https://pic1.zhimg.com/50/77bc9cc58883af6164e72fe5281f28b7_720w.jpg?source=1940ef5c)

2015/11/4 00:33

\-----------------------------------------------------------------------------------------------------------------

列表弄好了，颜色布局还要调一调。

![](https://pic1.zhimg.com/50/4aef4e8bd1eab68b4aa49249d72b91d9_720w.jpg?source=1940ef5c)

2015/11/3 22:49

\-----------------------------------------------------------------------------------------------------------------

调试了下Api，已经可以正常工作了，开始撸日报列表了。

![](https://pic1.zhimg.com/50/81fea27758003ab7c07e6caa010250d6_720w.jpg?source=1940ef5c)

2015/11/3 23:30

\-----------------------------------------------------------------------------------------------------------------

想了想，就用RRD(Retrofit.RxJava.Dagger2)来写个简单的第三方知乎日报客户端吧。

工程已经建好了，撸了点基类，每天下班了我会抽时间撸一下的。

![](https://pic2.zhimg.com/50/63e4a15ed1aa9a911c9f28174d8b7016_720w.jpg?source=1940ef5c)

2015/11/3 20:50

\-----------------------------------------------------------------------------------------------------------------

评论有要完整例子的，这周我写个简单的例子放GitHub上面吧。

知乎上我就不写了，编辑工具太难用了，不适合贴大量代码。

2015/11/2 11:29

\-----------------------------------------------------------------------------------------------------------------

感谢

[Max安珀](http://www.zhihu.com/people/maxan-po)

的回复

OkHttp是Square开源Android版Http客户端。除了OkHttp，Android自带HttpURLConnection和HttpClient。

而Retrofit则在Http客户端外继续封装了一层，让你从服务器获取数据更加方便，所以说二者不矛盾。

Retrofit 2.0后默认使用的是OkHttp，2.0之前是可选的。

2015/10/31 21:14 更新由上而下，最新的编辑在最上面，所以你可能需要从下往上看了。

\-----------------------------------------------------------------------------------------------------------------

前两周左右，团队开了新项目，而且我们团队自己的框架正好迭代了新版本，集成了基于事件流的响应式编程框架RxJava。

所以我们Android组开了一个会，会上组长提出了这个项目要用RxJava配合Retrofit和Dagger2开发的要求。

真不巧，我所处的项目小组是第一个吃螃蟹的人(虽然Android组很大，但是项目组就只有4个人)，而且我才入职三个月，第一次听到Rx还是听导师随口说起的。

虽然我在读大学的时候见过Dagger这几个字母，但是我根本没摸过它，我大学也就是用过ButterKnife的水平，更别说Dagger2了。

当时我就感觉很碉啊，简直就是碉堡了，因为又有新的Android技术可以让我折腾了，果然没有来错地方啊！

RxJava国内高质量的文章还是有的。

比如这一篇：

『匠心写作』里面的『

[给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)

』，是目前就职于Flipboard的『扔物线』写的，通俗易懂。

但目前Dagger2在国内的中文资料还是很少，网上搜来搜去也就那几篇，而且质量不高，感觉写的人语文都学得不怎么好。

所以我们组长亲自动手，写好了RxJava+Retrofit+Dagger2的配合使用文档，并且弄好了项目的架构设计（简直就是良心）。

当项目遇上了RxJava+Retrofit+Dagger2的巧妙结合，根本让你欲罢不能，个中好处要用过才知道。

注【我不会讲解Dagger2+RxJava+Retrofit它们分别的详细使用方法，很占篇幅】

那么我先举个从服务器获取用户数据的例子，看看如何用RxJava+Retrofit+Dagger2的方式来写：

基本方式使用Retrofit和RxJava

```
//User Service
Service service = new ServiceImpl();

//创建Observable对象
Observable<User> observable= Observable.create(
new Observable.OnSubscribe<User>() {
    @Override
    public void call(Subscriber<? super User> subscriber) {
        try {
            User user =service.getUser("username","password")
            subscriber.onNext(user);
            subscriber.onCompleted();
        } catch (Exception e) {
            subscriber.onError(e);
        }
    }
});

//绑定订阅者
observable.subscribeOn(Schedulers.io())//在IO线程进行网络请求，获取数据
.observeOn(AndroidSchedulers.mainThread())//在UI线程操作数据
.subscribe(new Action1<User>() {
    @Override
    public void call(User user) {
        //将userInfo数据显示到UI上
    }
});

```

上面的代码在ServiceImpl类的getUser方法里面使用了Retrofit获取服务器数据。

获取数据是在RxAndroid提供的IO线程进行的，显示数据是在UI线程进行的。

这就是基本的RxJava的用法了。

现在我们来看看上面的代码有什么缺点：

(1). service实例可以是单例模式，我们不需要在每处需要请求的地方都去重新实例化一个新的对象。

所以我们只需要实例化一次service对象，然后通过Dagger2把这个对象注入给一个全局变量(比如BaseActivity的成员变量)

这样我们可以在任何地方获取到它。

(2).还有创建Observable对象太麻烦了，其实Retrofit支持直接返回Observable类型的对象。

\--------------------------------------------------------------------------------------------------------------

下面开始修改我们的代码：

```
getService()//取得全局Service
.getUser("username","password")//调用返回Observable类型对象的方法
.subscribeOn(Schedulers.io())//在IO线程进行网络请求，获取数据
.observeOn(AndroidSchedulers.mainThread())//在UI线程操作数据
.subscribe(new Action1<User>() {
    @Override
    public void call(User user) {
        //将userInfo数据显示到UI上
    }
});
```

代码是不是一下子减少了很多，之前的实例化Service和新建Observable的代码都分散到了Dagger2的组件和模块中，代码的耦合度降低。

但是我们的Service可能不止一个啊，这个时候你可以继续抽象一层DataLayer层出来。

通过一个DataLayer对象可以获取所有的Service，这些Service都是单例的，且通过Dagger2注入。

我们可以在Application里面初始化DataLayer对象，用单例模式，然后注入给BaseActivity，BaseFragment等等基类的成员变量。

这样你在Fragment/Activity里面直接调用

getDataLayer().getUserService().getUser().subscribeOn.....

有没有觉得这种链式的调用方式很方便，很清晰，有一种碉堡的感觉？！

依赖关系图：

![](https://pic2.zhimg.com/50/0f9b63df9b9878637fc3a7b8cf6eca30_720w.jpg?source=1940ef5c)

2015/10/31 18:19

\--------------------------------------------------------------------------------------------------------------

昨天很忙，没时间讲Rx+Retrofit+Dagger2怎么配合开发的，今天先回家，吃了饭就开更。

2015/10/30

\--------------------------------------------------------------------------------------------------------------

Retrofit支持返回Observale对象，这意味着我们可以使用RxAndroid(RxJava)来处理网络请求的问题。

在RxAndroid提供的IO线程里面进行网络数据的请求，在主线程进行数据的显示。

正好我们项目也是这么做的，同时我们还使用了Dagger2来进行解耦操作。

今天有点忙，下午再来说说怎么用Dagger2和Retrofit以及Rx很好的配合开发。

![](https://pic2.zhimg.com/50/8e765f0816c042d18bff0f4226a06a5f_720w.jpg?source=1940ef5c)
