# Android 多种支付方式的优雅实现

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650841069&idx=1&sn=e359bbbb29a6ec9c49279b2d7bd3afb2&chksm=80b74b73b7c0c265aa3b61b74573dc176987243933d4b4495c23687e42ba6f74995cf93c46e2&mpshare=1&scene=1&srcid=1223GWqHDXOq795bxnbdyrc7&sharer_sharetime=1640227421661&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)零先生 鸿洋

本文作者

作者：**零先生**

链接：

https://juejin.cn/post/7034033212284207140

本文由作者授权发布。

_1_

场景

App 的支付流程，添加多种支付方式，不同的支付方式，对应的操作不一样，有的会跳转到一个新的webview，有的会调用系统浏览器，有的会进去一个新的表单页面，等等。

并且可以添加的支付方式也是不确定的，由后台动态下发。

如下图所示：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2FMOu2ZNAwZwOcrdZL7viaxyJFbic69ajGK1AAHiceQ9x24JSKLj5mLQqlGTn5tTicHyX4yCu239HibBpp1doUjeIHr9A%2F640%3Fwx_fmt%3Dother)

根据上图 ui 理一下执行流程：

1\. 点击不同的添加支付方式 item。

2\. 进入相对应的添加支付方式流程（表单页面、webview、弹框之类的）。

3\. 在第三方回调里面根据不同的支付方式执行不同的操作。

4\. 调用后台接口查询添加是否成功。

5\. 根据接口结果展示不同的成功或者失败的ui。

_2_

以前的实现方式

用一个 Activity 承载，上述所有的流程都在 Activity 中。Activity 包含了列表展示、多种支付方式的实现和 ui。

伪代码如下：

    class AddPaymentListActivity : AppCompatActivity(R.layout.activity_add_card) {    private val addPaymentViewModel : AddPaymentViewModel = ...    override fun onCreate(savedInstanceState: Bundle?) {        super.onCreate(savedInstanceState)        addPaymentViewModel.checkPaymentStatusLiveData.observer(this) { isSuccess ->            // 从后台结果判断是否添加成功            if (isSuccess) {                addCardSuccess(paymentType)            } else {                addCardFailed(paymentType)            }        }    }    private fun clickItem(paymentType: PaymentType) {        when (paymentType) {            PaymentType.ADD_GOOGLE_PAY -> //执行添加谷歌支付流程            PaymentType.ADD_PAY_PEL-> //执行添加PayPel支付流程            PaymentType.ADD_ALI_PAY-> //执行添加支付宝支付流程            PaymentType.ADD_STRIPE-> //执行添加Stripe支付流程        }    }    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {        super.onActivityResult(requestCode, resultCode, data)        when (resultCode) {            PaymentType.ADD_GOOGLE_PAY -> {                // 根据第三方回调的结果，拿到key                // 根据key调用后台的Api接口查询是否添加成功            }            PaymentType.ADD_PAY_PEL -> // 同上            // ...        }    }    private fun addCardSuccess(paymentType: PaymentType){        when (paymentType) {            PaymentType.ADD_GOOGLE_PAY -> // 添加对应的支付方式成功，展示成功的ui，然后执行下一步操作            PaymentType.ADD_PAY_PEL-> // 同上            // ...        }    }    private fun addCardFailed(paymentType: PaymentType){        when (paymentType) {            PaymentType.ADD_GOOGLE_PAY -> // 添加对应的支付方式失败，展示失败的ui            PaymentType.ADD_PAY_PEL-> // 同上            // ...        }    }    enum class PaymentType {        ADD_GOOGLE_PAY, ADD_PAY_PEL, ADD_ALI_PAY, ADD_STRIPE    }}

-

虽然看起来根据 paymentType 来判断，逻辑条理也还过得去，但是实际上复杂度远远不止如此。

• 不同的支付方式跳转的页面相差很大。

• 结果的回调获取也相差很大，并不是所有的都在onActivityResult中。

• 成功和失败实际上也不能统一来处理，里面包含很多的 if...else...判断。

• 如果支付方式是后台动态下发的，处理起来判断逻辑就更多了。

此外，最大的问题：扩展性问题。

当新来一种支付方式，例如微信支付之类的，改动代码就很大了，基本就是将整个Activity中的代码都要改动。可以说上面这种方式的可扩展性为零，就是简单的将代码都揉在一起。

_3_

优化后的代码

要想实现高内聚低耦合，最简单的就是套用常见的设计模式，回想一下，发现策略模式+简单工厂模式非常这种适合这种场景。

先看下优化后的代码：

    class AddPlatformActivity : BaseActivity() {    private var addPayPlatform: IAddPayPlatform? = null    private fun addPlatform(payPlatform: String) {        // 将后台返回的支付平台字符串变成枚举类        val platform: PayPlatform = getPayPlatform(payPlatform) ?: return        addPayPlatform = AddPayPlatformFactory.getCurrentPlatform(this, platform)        addPayPlatform?.getLoadingLiveData()?.observe(this@AddPlatformActivity) { showLoading ->                if (showLoading) startLoading() else stopLoading()            }        addPayPlatform?.addPayPlatform(AddCardParameter(platform))    }    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {        super.onActivityResult(requestCode, resultCode, data)        // 将onActivityResult的回调转接到需要监听的策略类里面        addPayPlatform?.thirdAuthenticationCallback(requestCode, resultCode, data)    }}

_4_

策略模式

引用菜鸟教程上简单介绍下：

_https://www.runoob.com/design-pattern/strategy-pattern.html_

**意图**： 定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。

**主要解决**： 在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。

**何时使用**： 一个系统有许多许多类，而区分它们的只是他们直接的行为。

**如何解决**： 将这些算法封装成一个一个的类，任意地替换。

**关键代码**： 实现同一个接口。

**优点**： 1、算法可以自由切换。2、避免使用多重条件判断。3、扩展性良好。

**缺点**： 1、策略类会增多。2、所有策略类都需要对外暴露。

**使用场景**： 1、如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。2、一个系统需要动态地在几种算法中选择一种。3、如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现。

_5_

需要实现的目标

### **5.1 解耦宿主 Activity**

现在宿主Activity中代码太重了，包含多种支付方式实现，还有列表ui的展示，网络请求等。

现在目标是将 Activity 中的代码拆分开来，让宿主 Activity 变得小而轻。

如果产品说新增一种支付方式，只需要改动很少的代码，就可以轻而易举的实现。

### **5.2 抽取成独立的模块**

因为公司中有可能存在多个项目，支付模块的分层应该处于可复用的层级，以后很有可能将其封装成一个独立的 mouble，给不同的项目使用。

现在代码全在 Activity 中，以后若是抽取 mouble 的话，相当于整个需求重做。

### **5.3 组件黑盒**

"组件黑盒"这个名词是我自己的一个定义。大致意思：

> 将一个 View 或者一个类进行高度封装，尽可能少的暴露 public 方法给外部使用，自成一体。
> 
> 业务方在使用时，可以直接黑盒使用某个业务组件，不必关心其中的逻辑。
> 
> 业务方只需要一个简单的操作，例如点击按钮调用方法，然后逻辑都在组件内部实现，组件内处理外部事件的所有操作，例如：Loading、请求网络、成功或者失败。
> 
> 业务方都不需要知道组件内部的操作，做到宿主和组件的隔离。

> 当然这种处理也是要分场景考虑的，其中一个重点就是这个组件是偏业务还是偏功能，也就是是否要将业务逻辑统一包进组件，想清楚这个问题后，才能去开发一个业务组件。 摘自xu'yi'sheng博客。
> 
> _https://xuyisheng.top/author/xuyisheng/_

因为添加支付方式是一个偏业务的功能，我的设计思路是：

外部 Activity 点击添加对应的支付方式，将支付方式的枚举类型和支付方式有关的参数通过传递，然后不同的策略类组件执行自己的添加逻辑，再通过一层回调将第三方支付的回调从 Activity 中转接过来，每个策略类内部处理自己的回调操作，具体的策略类自己维护成功或者失败的ui。

_6_

具体实现

### **6.1 定义顶层策略接口**

    interface IAddPayPlatform {    fun addPayPlatform(param: AddCardParameter)    fun thirdAuthenticationCallback(requestCode: Int?, resultCode: Int?, data: Intent?)    fun addCardFailed(message: String?)    fun addCardSuccess()}

-

### **6.2 通用支付参数类**

    open class AddCardParameter(val platform: PayPlatform)class AddStripeParameter(val card: Card, val setPrimaryCard: Boolean, platform: PayPlatform)    : AddCardParameter(platform = PayPlatform.Stripe)

-

因为有很多种添加支付方式，不同的支付方式对应的参数都不一样。

所以先创建一个通用的卡片参数基类AddCardParameter, 不同的支付方式去实现不同的具体参数。这样的话策略接口就可以只要写一个添加卡片的方法addPayPlatform(param: AddCardParameter)。

### **6.3 Loading 的处理**

因为我想实现的是黑盒组件的效果，所有添加卡片的loading也是封装在每一个策略实现类里面的。

Loading的出现和消失这里有几种常见的实现方式：

• 传递BaseActivity的引用,因为我的loading有关的方法是放在BaseActivity中，这种方式简单但是会耦合BaseActivity。

• 使用消息事件总线，例如EventBus之类的，这种方式解耦强，但是消息事件不好控制，还要添加多余的依赖库。

• 使用LiveData，在策略的通用接口中添加一个方法返回Loading的LiveData, 让宿主Activity自己去实现。

    interface IAddPayPlatform {    // ...    fun getLoadingLiveData(): LiveData<Boolean>?}

-

### **6.4 提取BaseAddPayStrategy**

因为每一个添加卡的策略会存在很多相同的代码，这里我抽取一个BaseAddPayStrategy来存放模板代码。

需要实现黑盒组件的效果，宿主Activity中都不需要去关注添加支付方式是不是存在网络请求这一个过程，所以网络请求也分装在每一个策略实现类里面。

    abstract class BaseAddPayStrategy(val activity: AppCompatActivity, val platform: PayPlatform) : IAddPayPlatform {  private val loadingLiveData = SingleLiveData<Boolean>()  protected val startActivityIntentLiveData = SingleLiveData<Intent>()  override fun getLoadingLiveData(): LiveData<Boolean> = loadingLiveData  protected fun startLoading() = loadingLiveData.setValue(true)  protected fun stopLoading() = loadingLiveData.setValue(false)  private fun reloadWallet() {      startLoading()      // 添加卡片完成后，重新刷新钱包数据  }  override fun addCardSuccess() {      reloadWallet()  }  override fun addCardFailed(message: String?) {      stopLoading()      if (isEWalletPlatform(platform)) showAddEWalletFailedView() else showAddPhysicalCardFailedView(message)  }   /**    * 添加实体卡片失败展示ui    */  private fun showAddPhysicalCardFailedView(message: String?) {       showSaveErrorDialog(activity, message)  }   /**    * 添加实体卡片成功展示ui    */  private fun showAddPhysicalCardSuccessView() {      showCreditCardAdded(activity) {          activity.setResult(Activity.RESULT_OK)          activity.finish()      }  }  private fun showAddEWalletSucceedView() {      // 添加电子钱包成功后的执行      activity.setResult(Activity.RESULT_OK)      activity.finish()  }  private fun showAddEWalletFailedView() {      // 添加电子钱包失败后的执行  }  // ---默认空实现，有些支付方式不需要这些方法---  override fun thirdAuthenticationCallback(requestCode: Int?, resultCode: Int?, data: Intent?) = Unit  override fun getStartActivityIntent(): LiveData<Intent> = startActivityIntentLiveData}

-

### **6.5 具体的策略类实现**

通过传递过来的AppCompatActivity引用获取添加卡片的ViewModel实例AddPaymentViewModel,然后通过AddPaymentViewModel去调用网络请求查询添加卡片是否成功。

    class AddXXXPayStrategy(activity: AppCompatActivity) : BaseAddPayStrategy(activity, PayPlatform.XXX) {  protected val addPaymentViewModel: AddPaymentViewModel by lazy {      ViewModelProvider(activity).get(AddPaymentViewModel::class.java)  }  init {      addPaymentViewModel.eWalletAuthorizeLiveData.observeState(activity) {          onSuccess { addCardSuccess()}          onError { addCardFailed(it.detailed) }      }  }  override fun thirdAuthenticationCallback(requestCode: Int?, resultCode: Int?, result: Intent?) {      val uri: Uri = result?.data ?: return      if (uri.host == "www.xxx.com") {          uri.getQueryParameter("transactionId")?.let {              addPaymentViewModel.confirmEWalletAuthorize(platform.name, it)          }      }  }  override fun addPayPlatform(param: AddCardParameter) {      startLoading()      addPaymentViewModel.addXXXCard(param)  }}

_7_

简单工厂进行优化

因为我不想在Activity中去引用每一个具体的策略类，只想引用抽象接口类IAddPayPlatform, 这里通过一个简单工厂来优化。-

    object AddPayPlatformFactory {    fun setCurrentPlatform(activity: AppCompatActivity, payPlatform: PayPlatform): IAddPayPlatform? {        return when (payPlatform) {            PayPlatform.STRIPE -> AddStripeStrategy(activity)            PayPlatform.PAYPAL -> AddPayPalStrategy(activity)            PayPlatform.LINEPAY -> AddLinePayStrategy(activity)            PayPlatform.GOOGLEPAY -> AddGooglePayStrategy(activity)            PayPlatform.RAPYD -> AddRapydStrategy(activity)            else -> null        }    }}

_8_

再增加一种支付方式

如果再增加一种支付方式，宿主Activity中的代码都可以不要改动，只需要新建一个新的策略类，实现顶层策略接口即可。

这样，不管是删除还是新增一种支付方式，维护起来就很容易了。

策略模式的好处就显而易见了。

* * *

最后推荐一下我做的网站，玩Android: _wanandroid.com_ ，包含详尽的知识体系、好用的工具，还有本公众号文章合集，欢迎体验和收藏！

推荐阅读：-

[Android 这个知识点一定要知道，官方也改了 2 次](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650841068&idx=1&sn=18f7a3842f2541553ee4c29918de17c0&chksm=80b74b72b7c0c2649824da978cfa753ccbc9fae166e9ca2ad7b9e3f50280957c3ae202f1b83e&scene=21#wechat_redirect)-

[我又来了，大厂基础架构组的苦与乐，2021年度总结！](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650841057&idx=1&sn=b2736a6812a088d069b77b91ffe22ab4&chksm=80b74b7fb7c0c2693c16786b3c358f04b064496b412b70beb0ae7b68e668952b02e931e7b164&scene=21#wechat_redirect)-

[玩转RxJava3，领略设计之美！](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650840910&idx=1&sn=13efeb62eb1efa171d271a7bff281fd5&chksm=80b74bd0b7c0c2c6cc4f2c2f95b99a2e600cbf84e48dda03db9d11ebac894a59f4d20146ac22&scene=21#wechat_redirect)-

**点击 **关注我的公众号-

如果你想要跟大家分享你的文章，欢迎投稿~

┏(＾0＾)┛明天见！

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650841069&idx=1&sn=e359bbbb29a6ec9c49279b2d7bd3afb2&chksm=80b74b73b7c0c265aa3b61b74573dc176987243933d4b4495c23687e42ba6f74995cf93c46e2&mpshare=1&scene=1&srcid=1223GWqHDXOq795bxnbdyrc7&sharer_sharetime=1640227421661&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)