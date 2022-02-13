# 深度解读Jetpack框架的基石，AppCompat

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650260812&idx=1&sn=560b1205a2cca6515be99fe80a4cf3ba&chksm=88633423bf14bd3537b3389d7bcc12ce63e0d84a70eaf3c31b8dd7716eeec5bb6b30886bd6f0&mpshare=1&scene=1&srcid=1224pJCT0pk2FAhFBmTpzBkO&sharer_sharetime=1640305715770&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)ELC1020 郭霖

![](https://image.cubox.pro/article/2021072009535636873/30903.jpg)-

/ 今日科技快讯 /

12月23日早间，腾讯控股在港交所发布公告称，以实物分派京东集团股份的方式宣派中期股息。京东集团也在早间发布公告，确认此事。此次派发后，腾讯控股对京东集团的持股比例将由17%降至2.3%，不再是京东的第一大股东。与此同时，腾讯总裁刘炽平也将退出京东集团董事会。

/ 作者简介 /

周五愉快，圣诞节愉快，我们下周再见。

本篇文章转自TechMerger的博客，文章主要分享了他对Jetpack中AppCompat模块的理解和分析，相信会对大家有所帮助！

原文地址：

> https://juejin.cn/post/6965860527897575437

/ 前言 /

为了能够让低版本的Android系统能够运行新特性，AppCompat框架自Support时代就已推出。但随着AndroidX的一统江湖，AppCompat的相关类则一并迁移到了AndroidX库里。

Android开发者应该都不陌生，在Android Studio上创建的项目默认采用AppCompatActivity作为Activity的基类。可以说，这个类是整个AppCompat框架里最重要的类，也是我们今天研究AppCompat的起点。

/ AppCompatActivity /

其间接继承自Activity，之间还继承了其他Activity特色类,可以使得低版本上运行的Activity也能拥有ToolBar和暗黑主题等新功能。

> AppCompatActivity extends FragmentActivity extends ComponentActivity extends ComponentActivity extends Activity\*

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fv1LbPPWiaSt6A5XcqN4XQuvicgY2p1HhGYib8WMf7LyGCA1Nx4vMcRicoz9mibnkWzyYt1aicn5jsEpN1yGcpiawzsicXw%2F640%3Fwx_fmt%3Dpng)

先来感受一下AppCompatActivity和Activity在UI上的表现。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fv1LbPPWiaSt6A5XcqN4XQuvicgY2p1HhGYBNia5ibRDqecCxNFaV4mzS2PKCHibZGMRW7fMMxiaJxNF09Fgu88DFClJA%2F640%3Fwx_fmt%3Dpng)

从对比图上看并没有太大区别，但从UI的树形图上看是有些区别的。比如AppCompatActivity的content区域的上方多了一个LinearLayout和ViewStub控件，再比如AppCompatActivity下面的是AppCompatTextView而不是TextView。

那这些差异是如何实现的，有什么用意？

谈到AppCompatActivity实现的话不得不提幕后的大管家AppCompatDelegate类，其承载了AppCompatActivity几乎所有的实现工作。

比如AppCompatActivity复写了setContentView()的逻辑，交由大管家AppCompatDelegate去实现其特有的UI结构。

### **AppCompatDelegate**

重点介绍下大管家的头号工作：setContentView()，具体分为如下几个小任务。

*   ensureSubDecor()确保ActionBar的特有UI结构创建完毕
    
*   removeAllViews()确保ContentView的所有Child全部被移除干净
    
*   inflate()将画面的内容布局解析并添加到ContentView下
    

第一步ensureSubDecor()的内容比较多,又分为几个子任务，包括调用createSubDecor()创建ActionBar特有布局,setWindowTitle()将Activity标题反映到ToolBar上以及applyFixedSizeWindow()去调整DecorView尺寸。

核心内容在于createSubDecor()这个子任务。它需要确保ActionBar的特有布局创建出来并和Window的DecorView产生联系。

**1\. ensureWindow()**

获取Activity所属的Window引用并添加window相关回调

**2\. getDecorView()**

告知Window去创建DecorView，这里要提一下PhoneWindow的generateLayout()，其将依据主题的创建不同的布局结构，比如AppCompatActivity的话将解析screen\_simple.xml得到DecorView的基本结构，其包括根布局LinearLayout，用来映射actionmode布局的viewstub以及承载App内容的id为ContentView

**3\. inflate()**

获取ActionBar的布局，主要是abc\_screen\_toolbar.xml和abc\_screen\_content\_include.xml两个文件

**4\. removeViewAt()和addView()**

将ContentView的子View迁移至ActionBar布局下。具体方法是将其所有child移除并add到ActionBar布局下id为action\_bar\_activity\_content的ViewGroup下面，并将原有ContentView的id置空，同时将该目标ViewGroup的id设置为Content。意味着它将成为AppCompatActivity画面承载内容区域的父布局

### **公开的API**

除了setContentView()在打造布局结构上的差异，AppCompatActivity还提供了些Activity所没有的API供开发者使用。

*   getSupportActionBar() 用以获取AppCompat特有的ActionBar组件供开发者定制ActionBar
    
*   getDelegate() 获取AppCompatActivity内部实现的大管家AppCompatDelegate的实例(实际上将通过静态的create()获取实现类AppCompatDelegateImpl的实例)
    
*   getDrawerToggleDelegate() 获取抽屉导航布局DrawerLayout的代理类ActionBarDrawableToggleImpl的实例,用来和ActionBar进行UI的交互
    
*   onNightModeChanged() 不同于配置了uiMode的外部配置变更后才能收到主题变化的通知，本API可以在暗黑主题的适配模式(比如跟随系统设置模式和跟随电量设置模式等)发生变化后得到回调，可利用这个时机做些补充处理
    

### **使用上的注意**

AppCompatActivity的注释上有如下说明，推荐采用Theme.AppCompat主题。

> You can add an ActionBar to your activity when running on API level 7 or higher by extending this class for your activity and setting the activity theme to Theme.AppCompat or a similar theme.

经过验证如果我们使用了别的主题就会得到如下的crash。

> You need to use a Theme.AppCompat theme (or descendant) with this activity.

原理在于上面自己的大管家AppCompatDelegate在创建ActionBar布局的时候有意地确保Activity是否采用了AppCompatTheme主题，尤其是如果没有指定AppCompat定义的windowActionBar的属性的话，将抛出如上的异常。

 

  // AppCompatThemeImpl.java-
 private ViewGroup createSubDecor() {-
 TypedArray a = mContext.obtainStyledAttributes(R.styleable.AppCompatTheme);

 if (!a.hasValue(R.styleable.AppCompatTheme\_windowActionBar)) {-
 a.recycle();-
 throw new IllegalStateException(-
 "You need to use a Theme.AppCompat theme (or descendant) with this activity.");-
 }-
 ...-
 }

 

至于为什么用异常来确保AppCompatTheme的采用，因为后续的处理跟AppCompatTheme息息相关，如果没有采用后面的很多处理将失效。

/ AppCompatDialog /

除了使用极高的AppCompatActivity以外，AppCompatDialog的曝光率也不低。其实现原理和AppCompatActivity企划一致，都是依赖大管家AppCompatDelegate进行实现。一样是为了在Dialog的基础上扩展出新ToolBar和暗黑主题的支持。

/ AppCompatTheme /

前面提到的AppCompatTheme主要分为两个主题。

**Theme.AppCompat**

继承自Base.V7.Theme.AppCompat主题，指定AppCompatViewInflater为widget等class的解析类，并设置AppCompatTheme所定义的基本属性，其顶级主题仍旧是老牌的主题Theme.Holo

**Theme.AppCompat.DayNight**

能够自动适配暗黑主题。其继承自Base.V7.Theme.AppCompat.Light，与Theme.AppCompat的区别主要在于其默认情况下采用了light系的主题，比如colorPrimary采用primary\_material\_light，而Theme.AppCompat则采用primary\_material\_dark颜色

App采用了该主题就可以自动适配暗黑模式，这是如何做到的？

/ Dark Theme 暗黑模式 /

AppCompatActivity在绑定BaseContext的时候会通过AppCompatDelegate的applyDayNight()去解析App设置的暗黑主题模式并做出一些相应的配置工作。

比如常用的**跟随省电模式**，其指的是设备的省电模式开启后将自动进入暗黑主题，降低功耗。反之关闭之后返回到白天主题。

具体实现是AppCompatDelegate将注册监听省电模式变化的广播(ACTION\_POWER\_SAVE\_MODE\_CHANGED)。当省电模式开启/关闭时，广播接收器将自动回调updateForNightMode()去更新对应的主题。

 

  private boolean applyDayNight(final boolean allowRecreation) {-
 ...-
 @NightMode final int nightMode = calculateNightMode();-
 @ApplyableNightMode final int modeToApply = mapNightMode(nightMode);-
 final boolean applied = updateForNightMode(modeToApply, allowRecreation);-
 ...-
 if (nightMode == MODE\_NIGHT\_AUTO\_BATTERY) {-
 // 注册监听省电模式的广播接收器-
 getAutoBatteryNightModeManager().setup();-
 }-
 ...-
 }

 abstract class AutoNightModeManager {-
 ...-
 void setup() {-
 ...-
 if (mReceiver == null) {-
 mReceiver = new BroadcastReceiver() {-
 @Override-
 public void onReceive(Context context, Intent intent) {-
 // 省电模式变化后的回调-
 onChange();-
 }-
 };-
 }-
 mContext.registerReceiver(mReceiver, filter);-
 }-
 ...-
 }

 private class AutoBatteryNightModeManager extends AutoNightModeManager {-
 ...-
 @Override-
 public void onChange() {-
 // 省电模式变化后回调主题切换方法更新主题-
 applyDayNight();-
 }

 @Override-
 IntentFilter createIntentFilterForBroadcastReceiver() {-
 if (Build.VERSION.SDK\_INT >= 21) {-
 IntentFilter filter = new IntentFilter();-
 filter.addAction(PowerManager.ACTION\_POWER\_SAVE\_MODE\_CHANGED);-
 return filter;-
 }-
 return null;-
 }-
 }

 

更新主题的处理则是如下关键代码。

 

  private boolean updateForNightMode(final int mode, final boolean allowRecreation) {-
 ...-
 // 如果Activity的BaseContext尚未初始化则直接适配新的主题值-
 if ((sAlwaysOverrideConfiguration || newNightMode != applicationNightMode)-
 && !mBaseContextAttached-
 ...) {-
 ...-
 try {-
 ...-
 ((android.view.ContextThemeWrapper) mHost).applyOverrideConfiguration(conf);-
 handled = true;-
 ...-
 }-
 }

 final int currentNightMode = mContext.getResources().getConfiguration().uiMode-
 & Configuration.UI\_MODE\_NIGHT\_MASK;

 // 如果Activity的BaseContext已经创建，-
 // 且App没有声明要处理暗黑主题变化的话，将重绘Activity-
 if (!handled-
 ...) {-
 ActivityCompat.recreate((Activity) mHost);-
 handled = true;-
 }

 // 假使App声明了处理暗黑主题变化的话，-
 // 那么将新的主题值更新到Configuration的uiMode属性-
 // 并回调Activity#onConfigurationChanged()，等待App的自行处理-
 if (!handled && currentNightMode != newNightMode) {-
 ...-
 updateResourcesConfigurationForNightMode(newNightMode, activityHandlingUiMode);-
 handled = true;-
 }

 // 最后检查是否要通知App暗黑主题模式发生变化-
 // (注意这里指的是App设置的暗黑主题切换的策略发生变更，-
 // 比如由跟随系统设置变更为固定暗黑模式等)-
 if (handled && mHost instanceof AppCompatActivity) {-
 ((AppCompatActivity) mHost).onNightModeChanged(mode);-
 }-
 ...-
 }

 

细心的开发者可能会注意到我们平常在AppCompatActivity的布局里使用的控件，最终得到的类名称里会多上AppCompat的前缀。比如声明的是TextView控件最后得到的是AppCompatTextView类的实例。这是怎么做到的，为什么这么做？这就离不开ppCompatViewInflater的默默付出。

/ AppCompatViewInflater /

核心功能就是将布局里的控件切换为AppCompat版本。在调用LayoutInflater解析App布局的阶段，大管家AppCompatDelegate将调用AppCompatViewInflater将布局中的控件逐个替换。

 

  final View createView(View parent, final String name, @NonNull Context context...) {-
 ...-
 switch (name) {-
 case "TextView":-
 view = createTextView(context, attrs);-
 verifyNotNull(view, name);-
 break;-
 case "ImageView":-
 view = createImageView(context, attrs);-
 verifyNotNull(view, name);-
 break;-
 ...-
 }-
 ...-
 return view;-
 }

 protected AppCompatTextView createTextView(Context context, AttributeSet attrs) {-
 return new AppCompatTextView(context, attrs);-
 }

 

除了上面提到的AppCompatTextView，AppCompat的widget目录下有很多为了兼容新特性扩展的控件。以AppCompatTextView和另一个常用的AppCompatImageView来一探究竟。

/ AppCompatTextView /

由代码注释就可以看出来该控件在TextView的基础上增加了**Dynamic Tint**和**Auto Size**两大特性。

先看下这两特性大体是什么效果。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fv1LbPPWiaSt6A5XcqN4XQuvicgY2p1HhGYjz2ibfZmukDz3riajTwnj85339vygcLnqMc2p83t3qN8iapCRrIplrGjw%2F640%3Fwx_fmt%3Dpng)

可以看到第二个TextView对背景着上了更深的绿色，并对icon着上了白色，使得它内部的icon和文字相较第一个TextView看起来更清楚。这是通过AppCompatTextView提供的backgroundTint和drawableTint属性实现的，这种给背景和icon动态着色的功能就是**Dynamic Tint**特性。

另外可以看到最下面TextView的文本内容正好铺满整个屏幕没有在末尾出现省略，而上面那个TextView的字体尺寸较大且在尾部用省略号表示。这种自动适配字体尺寸的效果同样是依赖AppCompatTextView提供的相关属性来完成。此为**Auto Size**特性。

### **Dynamic Tint**

主要依赖AppCompatBackgroundHelper和AppCompatDrawableManager实现，包括反映静态配置和动态修改的Tint属性。

主要经历这几步：

1\. loadFromAttributes() 解析布局里配置的Tint属性，核心处理在于能够将设置的Tint资源解析成ColorStateList实例。

 

 // ColorStateListInflaterCompat.java-
 private static ColorStateList inflate(Resources r, XmlPullParser parser) {-
 ...-
 while ((type = parser.next()) != XmlPullParser.END\_DOCUMENT-
 && ((depth = parser.getDepth()) >= innerDepth || type != XmlPullParser.END\_TAG)) {-
 ...-
 final int color = modulateColorAlpha(baseColor, alphaMod);-
 colorList = GrowingArrayUtils.append(colorList, listSize, color);-
 stateSpecList = GrowingArrayUtils.append(stateSpecList, listSize, stateSpec);-
 listSize++;-
 }-
 ...-
 return new ColorStateList(stateSpecs, colors);-
 }-
 

2\. setInternalBackgroundTint()和applySupportBackgroundTint() 负责管理和区分Tint颜色的取自静态配置的属性还是外部动态配置的参数

3\. tintDrawable()负责着色，本质在于调用Drawable#setColorFilter()去刷新颜色的绘制

 

  // ResourceManagerInternal.java-
 static void tintDrawable(Drawable drawable, TintInfo tint, int\[\] state) {-
 ...-
 if (tint.mHasTintList || tint.mHasTintMode) {-
 drawable.setColorFilter(createTintFilter(-
 tint.mHasTintList ? tint.mTintList : null,-
 tint.mHasTintMode ? tint.mTintMode : DEFAULT\_MODE,-
 state));-
 } else {-
 drawable.clearColorFilter();-
 }-
 ...-
 }-
 

### **Auto Size**

需要解决的问题是对Text内容依据最大宽度和当前size计算自适应的最佳字体尺寸，依赖AppCompatTextHelper和AppCompatTextViewAutoSizeHelper实现。

1\. 解析AutoSize相关属性的配置并设定是否需要自动适配字体尺寸的Flag

 

 // AppCompatTextViewAutoSizeHelper.java-
 void loadFromAttributes(AttributeSet attrs, int defStyleAttr) {-
 ...-
 if (a.hasValue(R.styleable.AppCompatTextView\_autoSizeTextType)) {-
 mAutoSizeTextType = a.getInt(R.styleable.AppCompatTextView\_autoSizeTextType,-
 TextViewCompat.AUTO\_SIZE\_TEXT\_TYPE\_NONE);-
 }-
 ...-
 if (supportsAutoSizeText()) {-
 if (mAutoSizeTextType == TextViewCompat.AUTO\_SIZE\_TEXT\_TYPE\_UNIFORM) {-
 ...-
 setupAutoSizeText();-
 }-
 ...-
 }-
 }

 private boolean setupAutoSizeText() {-
 if (supportsAutoSizeText()-
 && mAutoSizeTextType == TextViewCompat.AUTO\_SIZE\_TEXT\_TYPE\_UNIFORM) {-
 ...-
 if (!mHasPresetAutoSizeValues || mAutoSizeTextSizesInPx.length == 0) {-
 ...-
 for (int i = 0; i < autoSizeValuesLength; i++) {-
 autoSizeTextSizesInPx\[i\] = Math.round(-
 mAutoSizeMinTextSizeInPx + (i \* mAutoSizeStepGranularityInPx));-
 }-
 mAutoSizeTextSizesInPx = cleanupAutoSizePresetSizes(autoSizeTextSizesInPx);-
 }-
 mNeedsAutoSizeText = true;-
 }-
 ...-
 }

 

2\. 在文本内容初始化或变化的时候计算合适的字体尺寸并反映到UI上。

 

 // AppCompatTextView.java-
 protected void onTextChanged(CharSequence text, int start, int lengthBefore, int lengthAfter) {-
 ...-
 if (mTextHelper != null && !PLATFORM\_SUPPORTS\_AUTOSIZE && mTextHelper.isAutoSizeEnabled()) {-
 mTextHelper.autoSizeText();-
 }-
 }

// AppCompatTextHelper.java-
 void autoSizeText() {-
 mAutoSizeTextHelper.autoSizeText();-
 }

// AppCompatTextViewAutoSizeHelper.java-
 void autoSizeText() {-
 ...-
 if (mNeedsAutoSizeText) {-
 ...-
 synchronized (TEMP\_RECTF) {-
 ...-
 // 计算最佳size-
 final float optimalTextSize = findLargestTextSizeWhichFits(TEMP\_RECTF);-
 // 如果和预设的size不一致的话更新size-
 if (optimalTextSize != mTextView.getTextSize()) {-
 setTextSizeInternal(TypedValue.COMPLEX\_UNIT\_PX, optimalTextSize);-
 }-
 }-
 }-
 ...-
 }

 

/ AppCompatImageView /

和AppCompatTextView一样扩展了针对background和src的**Dynamic Tint**功能。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fv1LbPPWiaSt6A5XcqN4XQuvicgY2p1HhGYnnvmyViaM2ywg9B8RnjjFVDNAYuiaZPWInP5ZuMfW08zCvVAXXzohqXQ%2F640%3Fwx_fmt%3Dpng)

与AppCompatTextView不同的是AppCompatImageView对icon着色采用的属性不是**attr#drawableTint**是**attr#tint**。由AppCompatImageHelper和ImageViewCompat类实现,原理大同小异，不再赘述。

/ 辅助类 /

AppCompat框架的开发人员在实现AppCompat扩展控件等特性的时候用到很多辅助类，大家可以自行研究下其细节，学习下一些巧妙的实现思路。

*   AppCompatBackgroundHelper
    
*   AppCompatDrawableManager
    
*   AppCompatTextHelper
    
*   AppCompatTextViewAutoSizeHelper
    
*   AppCompatTextClassifierHelper
    
*   AppCompatResources
    
*   AppCompatImageHelper
    
*   ...-
    

/ 类图 /

最后上一下AppCompat框架的简易类图，帮助大家有个整体上的认识。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz%2Fv1LbPPWiaSt6A5XcqN4XQuvicgY2p1HhGYryCIZ2Tso9icK86XRibK3yvCQgKYwtT9oAZByhicibj7Ap9ZjWbERIejVw%2F640%3Fwx_fmt%3Dother)

/ 结语 /

可以看到AppCompat框架整体比较简单，因此也容易被大家忽视。但作为Jetpack系列里的基石，了解一下很有必要。

推荐阅读：

[我的新书，《第一行代码 第3版》已出版！](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650248955&idx=1&sn=0c5237154c4c8de2ca635f8a578aa701&chksm=88636794bf14ee823e8c11854b5c014e49a4af425c2947e7c62f3ce139062b5560b4c44e3d4f&scene=21#wechat_redirect)-

[详解DataStore，SharedPreferences终结者](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650260456&idx=1&sn=a78d51360dda67d203e3535f0b24f359&chksm=88633287bf14bb91111268b4ad60e5a55f249fbf962e0ae1d87a459a63b19bcfabf4f6ce8a3a&scene=21#wechat_redirect)-

[JetPack Compose 主题配色太少怎么办?](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650259910&idx=1&sn=725707bbbe6be06fa9ace1f11571d743&chksm=886330a9bf14b9bf507f17cd2b3944435bdabafe3ef74e6eaac69702afa9d88a4fb770ab3cf9&scene=21#wechat_redirect)-

欢迎关注我的公众号

学习技术或投稿

![](https://image.cubox.pro/article/2021072009535640474/30817.jpg)

![](https://image.cubox.pro/article/2021072009535689275/85652.jpg)

长按上图，识别图中二维码即可关注

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzA5MzI3NjE2MA==&mid=2650260812&idx=1&sn=560b1205a2cca6515be99fe80a4cf3ba&chksm=88633423bf14bd3537b3389d7bcc12ce63e0d84a70eaf3c31b8dd7716eeec5bb6b30886bd6f0&mpshare=1&scene=1&srcid=1224pJCT0pk2FAhFBmTpzBkO&sharer_sharetime=1640305715770&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)