---
created: 2021-10-13
source: https://xiaozhuanlan.com/topic/9816742350
author: 
---

# [重学安卓：从被误解到 “真香” 的 Jetpack DataBinding － 小专栏](https://xiaozhuanlan.com/topic/9816742350)


> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> Note 2021.5.25 **重要提示**：
> 
> 阅读本文的最佳时机是，**您已吃过** 阅读 “源码” 或 “源码分析文” 时 **找不到头绪的苦**。
> 
> 您还没吃过苦，那您先不要着急阅读本文。您得吃过苦，才会有体会。
> 
> 在您吃够这方面的苦后，您才有机会发现，本文正是专用于解决 “如何找到正确打开方式” 的困扰。
> 
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 DataBinding 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 DataBinding 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

很高兴见到你！

前面几期，我们在深度思考的帮助下，通过分析 **背景状况** 和 **身世使命**，分别掌握了 Lifecycle、LiveData、ViewModel 的存在缘由、职责边界、工作目标。

相信阅读完这 3 篇的小伙伴，对 **视图控制 之 状态管理** 的 **标准化开发模式** 已形成深刻的印象。

毫不夸张地说，在标准化开发模式的加持下，软件开发效率会 因为不可预期的错误被减少，而超大幅度地提升。

而且只要确立了这样的开发模式，往后软件开发便可以不假思索地按部就班，省去了大量因模棱两可、犹豫不决而耽误的时间！

## 被不假思索地抵制的 DataBinding

原本 视图控制 的标准开发模式，到此为止就要宣告完结，

> 没想到在和一位 “曾任腾讯 QQ 8 年工龄核心员工 的 现任深圳南山某自动化代码生产工具” CEO 的技术攀谈中，在他再三推荐 DataBinding 后，我对 DataBinding 重新报以重视，

并且在真正地经过深度思考以后，才发现，DataBinding 不是我和许多人原本想当然以为的那样。

相信许多人对 DataBinding 的印象仅仅停留在：

![](https://images.xiaozhuanlan.com/photo/2020/af0f071c4297caeae54bb8bbe93c8cec.png)

一看 xml 中居然还要写 三元表达式 “这样的逻辑”，便草草地断定：DataBinding 不是个好东西，在声明式编程中书写 UI 逻辑，既不可调试，也不便于察觉和追踪，万一出现问题就麻烦了。

事实果真是这样吗？

## 文章目录一览

-   前言
-   被不假思索地抵制的 DataBinding
-   DataBinding 的目标只有三个
-   DataBinding 问世前的混沌世界
-   DataBinding 为什么能解决这三个问题？
-   **造成人们误认为 DataBinding 是在 xml 中写 UI 逻辑的原因**
-   DataBinding 结合 LiveData 的使用
-   **Note 2019.12.12 加餐**：
    -   LiveData 和 ObservableField 的本质区别
-   **Note 2020.10.21 加餐：**
    -   对 BindingAdapter 存在缘由的解析
-   BindingAdapter 在解决 Drawable 复用问题上的绝妙使用
-   **Note 2020.4.26 加餐**：
    -   DataBinding 参数语法为何如此设计
-   **Note 2020.5.6 加餐**：
    -   BindingMethod 与 BindingAdapter 的本质区别
-   **Note 2020.12.11 加餐：**
    -   ViewBinding 的本质、与 DataBinding 的区别，以及适用场景
    -   **DataBinding 的注意事项 和 屡试不爽的排错技巧**
    -   **Note 2020.8.3 加餐：**
    -   注意自定义视图包名的书写规范
-   **Note 2020.7.27 加餐**：
    -   现有条件下解决 视图调用一致性问题 的最优解
-   综上

## DataBinding 的目标只有三个

同此前的所有文章一样，我写作的目的有且只有一个：帮助读者快速明确状况 —— 究竟是什么原因，导致某项技术的存在、某项技术究竟是为解决什么问题而存在。

所以本文也是一样开门见山 —— DataBinding 的存在，是为了实现唯一的核心目标：

> **通过 基于适配器模式 的 “数据驱动” 思想，规避视图调用的一致性问题**。
> 
> 并且数据驱动 顺带免去了 因调用视图对象 而存在的大量冗余的判空处理。
> 
> 并且数据驱动 顺带免去了 为调用视图对象 而存在的大量样板代码。

如果光是阅读了以上几点，你还是不太理解的话，那接下来我就来介绍 99% 的网文都不曾为你明确的真实状况。

## DataBinding 问世前的混沌世界

例如 3 年前我 自主设计并开发的一款日记软件，该软件包含横、竖屏布局。

![](https://images.xiaozhuanlan.com/photo/2019/2a9293016fe09e13a382d6978a18bd99.png)

该 calendar 日历控件只存在于横屏布局中。

因此如果是以往的开发模式，我们需要在每个 Fragment 刷新 calendar 状态的地方，去逐个调用 calendar 控件，手动 set 新的状态，并且 set 前还要判断该控件存不存在。

> 那么这就造成一致性的问题：因为人类不是机器，人类一定会有疏忽的时候。特别是项目中包含数十个横竖屏不同布局方案、而每个页面又在多个方法中调用多个控件的情况，当后期项目改动，或换个同事接手时，**很容易因疏忽了某些控件的存在与否，而埋下 “空指针异常” 的隐患**。

同理，为了拿到控件最终的状态，我们仍然是通过 调用控件的方式去获取状态，如此，若该控件不存在，可能造成空指针异常；而如果大量判空，代码就不优雅、十分冗余。

> 注：对 “一致性” 的概念不熟悉的朋友可参见[《架构组件 “一致性” 概念》](https://xiaozhuanlan.com/topic/9340256871)篇的完整解析。

除了一致性问题，还有个很明显的问题，就是每新建一个页面，都需要为各种控件 findViewById —— 这些令人难以忍受的样板代码。

何况我从不使用 ButterKnife，因为它仅仅是为了解决样板代码，并且还需要额外地在代码中书写注解，看上去十分蹩脚。

那怎么办呢？

## DataBinding 为什么能解决这三个问题？

首先，数据驱动 意味着，控件的状态被分离到 ViewModel 中管理，并且 **ViewModel 这一层只需负责状态变量本身的变化**，至于该变量在布局中究竟被哪些视图绑定、当前有没有视图来绑定，ViewModel 不用管。

那控件是如何做到被通知且更新状态的呢？

DataBinding 是通过 适配器模式和观察者模式 来管理控件刷新状态。

当状态变量发生变化时，需要开发者手动地完成状态的更新。这将通知 DataBinding 中绑定该变量的控件刷新数据。

![](https://images.xiaozhuanlan.com/photo/2020/0022960c3ea7e5c61fd1d515df483f51.png)

在 BaseObservable 子类中，为 getter 赋予了 [@Bindable](https://xiaozhuanlan.com/u/Bindable) 注解的，在编译后会生成与之相关的 BR 变量，于是 **通过在 setter 中调用 notifyPropertyChanged 方法，可以通知 DataBinding 去刷新绑定该状态的控件**。

> 具体的代码可参考：在 Android 项目结构树状图中，通过 Java（Generate）节点 查看 目录下生成的 DataBindingImpl 文件（例如《重学安卓》项目生成的 FragmentTestDatabindingObserveBindingImpl），其中 executeBindings 方法，就是数据刷新的逻辑所在。

**executeBindings 会依据 [@Bindable](https://xiaozhuanlan.com/u/Bindable) 生成的 dirtyFlags 来区分当前是哪个 BR 被通知变化，从而只更新与之相关的控件**，这么设计的缘由很容易理解：

> 1.为了 **“精准投放”**，而不是所有控件都重绘，从而维持良好的性能。
> 
> 2.当多个控件绑定了同一个状态变量时，这些控件因为 dirtyFlags 一致的缘故，都会被通知，而无需开发者手动在 Java 代码中逐个调用控件去刷新。

但 Observable 的做法 反而滋生了另一种一致性问题：

例如状态变量对应的类中，某些成员要是遗漏了 `notifyPropertyChanged`，这个成员的通知刷新就会不起效。

所以说，数据驱动思想是好的，实现了视图与状态的分离。就是实现上有点不尽人意，居然需要开发者手动去通知。

那怎么办呢？**于是 Google 又设计了一种 更为简便的状态声明方式**：

通过 ObservableField。

于是你只需在 ViewModel 中 **分别声明该页面所包含的状态**，然后在布局中与控件一对一地绑定就 OK 了。

![](https://images.xiaozhuanlan.com/photo/2020/bc46d7a841a95dd9a159b4f25006f8f1.png)

如图，每个 ObservableField 都是持有 基本数据类型 或 String，来 **作为单一的 State 去绑定 布局中对应控件的属性**，从而当从外界为 ObservableField set 新的值、也即 **state 发生变化时，能精准地只通知绑定了的控件发生重绘**。

再就是双向绑定，双向绑定的存在 主要是为了 避免直接调用控件实例来获取数据，来避免视图调用的一致性问题。

比如登录的场景，用户名、密码输入完毕点击 “提交”，那么在双向绑定的帮助下，我们只需通过 State-ViewModel 拿到与 用户名、密码 输入框发生双向绑定的 ObservableField 即可。

双向绑定的书写方式是在普通绑定的基础上加个等号，例如：

```
android:text="@={vm.name}"
```

所以，目前为止，相信大家在 Fragment 等视图控制器中对控件视图的判空，已经趋近于 0 了，因为数据驱动，完全不需要手动调用视图。

所以光是数据驱动，就已经解决了 findViewById 的问题、并且一口气把最开始提到的 3 个目标全解决了。

## 人们误认为 DataBinding 是在 xml 中写 UI 逻辑的原因

回到最初，文章开头提到的那种写法，是 官方文档 给出的具有误导性的 糟糕示例。

![](https://images.xiaozhuanlan.com/photo/2020/746f0ae19a2204024c1800f7a2b5e532.png)

建议别这么写。状态值应尽可能地 直接反映预期结果，而不是作为条件在 xml 中发生逻辑判断。

![](https://images.xiaozhuanlan.com/photo/2020/fe92684a5f1f639664f206458436b660.png)

![](https://images.xiaozhuanlan.com/photo/2020/21dde9cd571d0f12c70ade45c937fb27.png)

并且值得再次强调的是，**DataBinding 不负责 UI 逻辑**，UI 逻辑原本在哪里写，现在还是在哪里写，只不过，UI 逻辑中原本调用控件实例去刷新状态的方式，现在改成了数据驱动，从而规避了“视图调用的一致性问题”。

DataBinding 的核心目标 就仅仅是解决这个问题而已。

简言之，**DataBinding 在布局中的表达式，只是作为 UI 逻辑末端的 状态改变，而并不涉及错综复杂的 UI 逻辑本身**。明确了 DataBinding 的 **职责边界**，无论 末端赋值 的花样再多，也不再会迷乱你双眼。

## DataBinding 结合 LiveData 的使用

LiveData 是标准化开发中不可或缺的一员，通过 LiveData，可以让新手老手都轻易地写出遵循 “唯一可信源 数据分发” 的编码原则，从而避免 90% 不可预期的错误。

从 Android Studio 3.1 起已正式支持 LiveData DataBindingImpl 代码的生成。使用时就是简单地将 ViewModel 中的 ObservableField 替换为 MutableLiveData，并在视图控制器中为 DataBinding 绑定生命周期 Owner：`mBinding.setLifecycleOwner(this);` 就完事了。

![](https://images.xiaozhuanlan.com/photo/2020/190dca4f1d8942916a9f33116e82b51e.png)

关于 LiveData 本身的知识，可以参见[《LiveData 鲜为人知的 身世背景 和 独特使命》](https://xiaozhuanlan.com/topic/0168753249)

### **Note 2019.12.12 加餐**：

#### LiveData 和 ObservableField 的本质区别

既然 ObservableField 和 LiveData 都可以使用，那它俩选择哪个比较好呢？其实它俩各有各的特点，ObservableField 的特点是支持防抖，在 set 的数据不变的情况下，可以避免不必要的重复绘制。LiveData 则没有防抖的特性。所以可以根据场合来选择合适的方案。具体可见评论区 16楼的补充。

以及，现如今我们已确立，数据按场景划分，主要分为 state、request、callback 这三种。**state 场景下的数据，专用于视图绑定**，无论使用 ObservableField 还是 LiveData，都建议只携带一个 “原子类型”，例如 Integer、String 等，如此可以遵循 “开闭原则”，每个状态都是独立的，未来需求发生变动时，只需对单一状态进行增加和删除，而不必修改复合实体。

对于 request 场景（UI 层向数据层请求数据）的数据，通常使用 LiveData，但携带的是 复杂的 非原子类型的 实体类，所以通常在其回调中，手动为上述 state 逐个赋予新的属性值。

![](https://images.xiaozhuanlan.com/photo/2020/1a600ddd915da9e31ef37fd4abb64ec0.png)

具体可参考《最佳实践》的 `ui.state`、`ui.callback` 包下，三种场景 ViewModel 持有的数据的使用。

## Note 2020.10.21 加餐：

### 对 BindingAdapter 存在缘由的解析

BindingAdapter 的存在主要是为了实现一些以往只有拿到控件实例才能做的事，也即它和 “双向绑定” 设计的出发点一样，是以顺应 “规避视图调用的一致性问题” 这个核心目的为前提的、为解决某些场景下的问题而存在的 **延伸设计**。

![](https://images.xiaozhuanlan.com/photo/2020/4e8b55fa63801e4f8299e848d6a9cd60.jpeg)

仔细观察可发现，在自定义的 BindingAdapter 静态方法中，第一个参数对应的是 与该方法发生绑定的视图类型，例如 ImageView，也即 BindingAdapter 的这种 “传参设计” 确保了方法内部使用的视图实例是存在的、不为 null，因为 xml 中声明过了的视图，就发生绑定了，没有声明，就没发生绑定，从而没执行到该方法了，也即 无论何种情况都不会造成 “视图引用的 null 安全问题”。

所以这个设计是不是很巧妙呢~

与此同时，**这样的设计刚好也额外起到了 “代码复用” 的作用**，也即只要定义过一次 BindingAdapter 方法，后续遇到类似的场景和需求就可以直接在 xml 中套用 BindingAdapter 属性了。也许早期的时候需要下点功夫，准备各种 BindingAdapter 方法，然而这是显而易见的 **复利模型**：越是后期，因避免 “重复代码或一致性问题” 而节省下的时间 将是指数级增多。

## BindingAdapter 在解决 Drawable 复用问题上的绝妙使用

主要是涉及到 DataBinding 中 BindingAdapter 的使用。

说到 Drawable，我们都知道，旧时候，当项目中存在圆角需求时，为了布局预览，尽管 drawable 之间仅仅只是圆角半径或背景颜色的不同，我们还是不得不为每个布局都准备一份 drawable xml。久而久之，项目资源目录中存在了数十个这样的 xml 文件。

BindingAdapter 的出现，巧妙地解决了这个问题，使得 Drawable 既可以通过 Java 代码的方式定义而做到复用，又可以在布局中预览。

具体的使用可以参考这个库，有人已经实践过了。

> [https://github.com/whataa/noDrawable](https://github.com/whataa/noDrawable)

![](https://images.xiaozhuanlan.com/photo/2019/2712cdb7e137c6e73ac1eef453105d65.png)

并且除了 Drawable，BindingAdapter 还有许多其他的妙用，等着你去发掘。

## **Note 2020.4.26 加餐**：

### DataBinding 参数语法为何如此设计

> 有小伙伴询问，为什么 DataBinding 绑定的值要设计为在 xml 中声明 `@{ }` 而不是普通的写法，原因如下：

因为 xml 是一种声明式编程，DataBinding 是在 xml layout 基础上的外挂，所以实现上仍然是通过对 xml 的解析来转译为 java 代码。那么 xml 中的 `@{ }` 就很好理解，就是告知此处是留给 DataBinding 的表现机会 —— 在编译时根据 `@{ }` 中的内容生成代码。

`@{ }` 的取值类型 通常取决于 xml 属性的定义，对于控件本身的属性，可在控件的源码中查看；对于基于 BindingAdapter 封装的 binding 属性，可参考 DataBinding bindingadapter 包下的默认实现。

![](https://images.xiaozhuanlan.com/photo/2020/f2d9d4832aa7ad3121433e997b6725a2.png)

> Note 2020.4.27：在 “误认为 DataBinding 在 xml 中写逻辑” 的一节中我们提到，DataBinding 存在 “在 xml 中写逻辑” 这样的过度设计是有原因的：

在上一条补充中我们提到了，xml 中属性的取值类型 取决于控件或 BindingAdapter 对属性的定义。

对于 Visibility，会发现，在 ViewBindingAdapter 中没有与之相关的方法，我想可能是因为 Google 考虑到 Visibility 是三值的情况，包括 visible、gone，以及 invisible，因而采取直接在 xml 中写逻辑、在编译时直接 copy 这段逻辑生成代码。

很显然，这种过度设计 为软件工程埋下了安全隐患，因而目前在项目中都是十分克制地 —— **弱水三千 只取一瓢** —— 只取用作为 DataBinding 核心特点的 “数据驱动” 来点到为止地解决 视图调用的一致性问题，以免额外引入其他不可预期的错误。

至于 Visibility，我通常是考虑封装一个 visible vs gone 的二元对立 boolean 属性，而 invisible 的占位特性 完全可以通过 ConstraintLayout 的 goneMargin 来代替。

## **Note 2020.5.6 加餐**：

### BindingMethod 与 BindingAdapter 的本质区别

> 有小伙伴询问，BindingMethod 和 BindingAdapter 的本质区别是什么呢？

其实很简单，**BindingMethod 专注于解决 “现有属性名与方法名不一致” 的问题**，而不是 BindingAdapter 这种另外自定义一个属性。

如果这样说还不理解的话，那么这里再明示一下 BindingMethod 和 BindingAdapter 的共同背景 —— 就是上上个 Note 中补充的，DataBinding 是根据 xml `@{ }` 内容来编译时生成 java 代码的，

正常情况下，控件定义的 xml 属性或 BindingAdapter，和方法名会对的上，所以生成的 java 代码也是能运行的，但由于历史问题，存在极个别的对不上的情况，比如 ImageView 的 `android:tint` 属性本该对应 setTint 方法，但实际上并没有 setTint，而是 setImageTintList，那么这时候就可以借助 BindingMethod —— **它就只是在 “DataBinding 在编译时生成代码” 这个背景下，帮助做个方法名转换**。

BindingMethod 的具体用法可见 [官方文档](https://developer.android.google.cn/topic/libraries/data-binding/binding-adapters)，这里不做累述。

（感谢读者 [@骑蜗牛去看大海](https://xiaozhuanlan.com/u/5376375709) 补充的状况。总的来说，BindingMethod 的使用和管理并不简便，因为需要另写一个类继承原控件、从而基于该类作注解（在软件工程的背景下，容易造成一致性问题，比如同事不记得或不知道已有该类，而又重复写个）。所以目前项目中一般优先使用 BindingAdapter 来自定义属性，具体可见《最佳实践》项目 ui.base.binding 包下对 BindingAdapter 的使用）

## Note 2020.12.11 加餐：

### ViewBinding 的本质、与 DataBinding 的区别，以及适用场景

> 媒体对 ViewBinding 作用的宣传有点脱离实际，比如 ViewBinding 的作用仅仅是**用于取代 findViewById 或 ButterKnife —— 主要是为了解决 获取视图实例时 类型转换的安全问题**，它自身其实并不能够 “解决 null 安全问题”。
> 
> 如要说解决 null 安全问题，那也需强调是在 kotlin 的背景下解决，因为截至 2020 年，绝大多数企业仍然在使用 Java 开发或维护 Android App。

ViewBinding 的存在，主要是通过 “自动化生成中间代码” 来 **规避 视图实例化时的 “类型转换安全问题”**，

和 DataBinding 的共同点在于通过 “**自动化生成中间代码**” 来隐藏诸如 “视图实例初始化” 等样板代码，以此规避不可预期的错误，

并且，“人如其名”，ViewBinding 和 DataBinding 的区别就在于前者是 “视图绑定”，后者是 “数据绑定”，也即前者不像后者是通过 “可观察数据” 通知所绑定视图实例的方式 来规避 null 安全一致性问题，

因而 **在 Java 下，对视图调用一致性问题的解决，只能依靠 DataBinding**，ViewBinding 光靠自身不足以解决。

而如果是在 kotlin 下，你便多了 ViewBinding 这个选项，因为 kotlin 语言特性使得 **只要是 Java 中被声明了 [@Nullable](https://xiaozhuanlan.com/u/Nullable) 的变量，在 kotlin 中调用时 IDE 会给出显眼的提示要求做判空操作**，而它的判空又是如此简单 —— 一个 `?` 搞定 —— ViewBinding 潜在的 null 安全设计，其实就是围绕这个来做的，ViewBinding 只负责 根据布局控件差异 自动为中间代码添加 [@Nullable](https://xiaozhuanlan.com/u/Nullable) 的声明，

> 划重点 👆 👆 👆

![](https://images.xiaozhuanlan.com/photo/2020/2bd0e746d8a0557aeb9ba57492c3fd38.jpeg)

如图，“横屏布局” 比 “竖屏布局” 少了个 tv\_title\_3 控件，IDE 在开发者代码调用 tv\_title\_3 控件时就能直接检查出并提示，如此开发者随手补个 `?` 就行（比如 `mBinding.tvTitle3?.text = “title3”`）

这样是不是就很方便了呢。

当然很多时候出于复用的考虑，我们也会有 BindingAdapter 那种高内聚低耦合的设计需求，为此在 kotlin 下可以通过 “拓展函数” 来满足。

总之，ViewBinding 是一款适用于 kotlin 语境的 UI 框架，如果你坚持留在 Java 或是 MVVM 开发模式，那么 DataBinding 是你不二之选。

## DataBinding 的注意事项 和 屡试不爽的排错技巧

DataBinding 对应的视图控制器，例如 Fragment 的类名发生了变化，那么 DataBinding 布局中引用的视图控制器的内部类，可能被抹去，需要在视图控制器类名发生变化后手动补充：

![](https://images.xiaozhuanlan.com/photo/2020/08f5670730d7d43381df5fb61fc672ba.png)

会变成

![](https://images.xiaozhuanlan.com/photo/2020/8fc1e53e6bcba6a75efabc3ee227af75.png)

造成这个现象的原因暂不知。

那我是怎么获知这个消息的呢？直接通过 Android Studio 的 EventLog 是看不出的。而且 EventLog 常常放出 “假消息”，让开发者无法真正觉察问题所在。

> 这里介绍一个好招。

在 Android Studio 的 Terminal 中，输入 `gradlew build`（MacOS 下 `./gradlew build`），通过这个来编译，就能精准地打印出问题所在。屡试不爽！

再就是，DataBinding 适合在主干工程（例如 App 模块）中使用，不要在第三方库中使用，因为 DataBinding 的状况是，如果 Library Module 使用了 DataBinding，那么 Application Module 也得使用。

而第三方库是多项目共享的，甚至可能开源给互联网上其他开发者使用的，因而 DataBinding 务必不在 Library 中使用。

此外，DataBinding 需要 root view 包含 layout/布局名\_0，比如 `layout/activity_main_0` 这样的 tag，所以对于对话框等场景，需要在 DataBindingUtil.bind 之前，手动为 inflate 出的 root view setTag。

最后，如在 Adapter item 的 xml 布局中绑定状态，需在 adapter 的 onBindViewHolder 回调末尾执行 binding.executePendingBindings()，这么做的缘由可详见 读者空大 在评论区 9 楼的解析。

## Note 2020.8.3 加餐：

### 注意自定义视图包名的书写规范

根据《Note 2020.4.26 加餐：DataBinding 参数语法为何如此设计》一节我们得以知晓，DataBinding 本质上是一种模板驱动的设计，也即通过事先约定的声明式语法，来生成目标文件。

因而当你在使用自定义控件时，务必注意包名中不能含有大写开头，这是 Java 规则如此，同时模板引擎也是基于 Java 规则来生成文件。

如果你在布局中引入了例如 `<com.kunminx.Test.TestView>` 这样的节点，是会发生 “找不到类” 的情况的，因为模板引擎会误认为这个 TestView 是 Test 类的内部类，也即生成的文件中它是会 `public Test.TestView testView;` 这样写的。

因而正确的做法是遵循 Java 规则，将包名全部改为小写，例如 `com.kunminx.test.TestView`。

## Note 2020.07.27 加餐：

### 现有条件下解决 视图调用一致性问题 的最优解

#### DataBinding 严格模式

DataBinding 严格模式 是我基于对 “**数据驱动 UI 框架的本质即解决视图调用一致性问题**” 的独家理解，而在 2020.04.17 自创的 DataBinding 开发模式。

DataBinding 严格模式通过将 mBinding 实例限制在基类，避免开发者通过 mBinding 拿到 View 实例，从而 **彻底规避** 视图调用的一致性问题、让视图调用的 **安全系数** 与基于函数式编程思想的 Jetpack Compose 持平（理论依据详见[《是 事关软件工程安全 的 数据驱动 UI 框架 扫盲干货》](https://xiaozhuanlan.com/topic/2356748910)）。

> 具体可以参考[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目的 DataBindingFragment 类。（目前已抽取到远程仓库，可在项目中通过双击 Shift 来搜索）

当然，这样也使传统的属性动画等操作无法编写。

好消息是，基于 ConstraintLayout 的 Motion 动画可以完美解决这一问题 —— **一行代码不用写，全在 xml 中搞定**。并且 Motion 动画将动画的制作难度和成本降低了 90%。具体可参考我们近期组织的 MotionChallenge 活动（提供一学就会的 Motion 动画教程）

[https://github.com/Jetpack-Missionary/MotionChallenge](https://github.com/Jetpack-Missionary/MotionChallenge)

## 综上

DataBinding 的核心目标是通过 “数据驱动” 思想，规避视图调用的一致性问题。

并且因为数据驱动的缘故，无形中免除了样板代码的书写，更是避免了代码中存在大量冗余的判空处理。

**可谓一箭三雕**。

DataBinding 不是用声明式编程的方式来编写 UI 逻辑，它的职责边界仅限于通过数据驱动来规避 “视图调用的一致性问题”，明确了这一点，就不会被作为末端的 状态赋值表达式 迷乱双眼。

而 DataBinding 的 BindingAdapter 是个百宝箱，等着大家去发掘。

> **快捷访问：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **Tip：**  
> · 考虑到移动端的阅读体验，已将代码改为图片展示。网页/小程序点击图片可放大查阅。  
> · Jetpack MVVM 系列文章会不定期增量更新，更新的内容会提供 Note 标记，可通过 ctrl + F（Windows）或 command + F（MacOS）搜索 “Note” 快速检索和查阅。

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。

> [往期回顾](https://kunminx.gitbook.io/relearn-android/new_moments/zhong-xue-an-zhuo-liang-zhou-nian-hui-gu-yu-zhan-wang)，**[专栏目录](https://kunminx.gitbook.io/relearn-android/category)**，**[更新动态](https://kunminx.gitbook.io/relearn-android/new_moments)**，[优惠政策](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#you-hui-zheng-ce)，[版权须知](https://kunminx.gitbook.io/relearn-android/ban-quan-sheng-ming#ban-quan-xu-zhi)
> 
> 温馨提示：如果这是第一次接触《重学安卓》，可通过上述链接来访问和快速了解《重学安卓》专栏、获取它的目录、试读内容，以及了解它的最新动态 和 发展状况。
> 
> 截至目前，专栏已对 **体系化文章** 做了 1970 余次修订，数十位群友告诉我 受专栏的启发 他们也开启了写作之路。群里不定期会有小伙伴讨论适配问题、分享原创的开源库 和 提供内推机会，订阅后可随时进群交流。

·

> Note 2021.5.25 **重要提示**：
> 
> 阅读本文的最佳时机是，**您已吃过** 阅读 “源码” 或 “源码分析文” 时 **找不到头绪的苦**。
> 
> 您还没吃过苦，那您先不要着急阅读本文。您得吃过苦，才会有体会。
> 
> 在您吃够这方面的苦后，您才有机会发现，本文正是专用于解决 “如何找到正确打开方式” 的困扰。
> 
> 我们绝不通篇贴源码，而是基于广泛的实践和反思，在累积过大量样本 乃至足以排除掉所有干扰信息后，**点到为止地揭露 DataBinding 框架最为核心的本质**，方便您理解其真实的存在意义，乃至可以笃信地将其用在项目中。
> 
> 关于 DataBinding 在项目中的实践，请另行查阅[《GitHub : Jetpack-MVVM-Best-Practice》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)源码以及[《架构模式实践》篇](https://xiaozhuanlan.com/topic/8204519736) 的完整解析。

## 前言

很高兴见到你！

前面几期，我们在深度思考的帮助下，通过分析 **背景状况** 和 **身世使命**，分别掌握了 Lifecycle、LiveData、ViewModel 的存在缘由、职责边界、工作目标。

相信阅读完这 3 篇的小伙伴，对 **视图控制 之 状态管理** 的 **标准化开发模式** 已形成深刻的印象。

毫不夸张地说，在标准化开发模式的加持下，软件开发效率会 因为不可预期的错误被减少，而超大幅度地提升。

而且只要确立了这样的开发模式，往后软件开发便可以不假思索地按部就班，省去了大量因模棱两可、犹豫不决而耽误的时间！

## 被不假思索地抵制的 DataBinding

原本 视图控制 的标准开发模式，到此为止就要宣告完结，

> 没想到在和一位 “曾任腾讯 QQ 8 年工龄核心员工 的 现任深圳南山某自动化代码生产工具” CEO 的技术攀谈中，在他再三推荐 DataBinding 后，我对 DataBinding 重新报以重视，

并且在真正地经过深度思考以后，才发现，DataBinding 不是我和许多人原本想当然以为的那样。

相信许多人对 DataBinding 的印象仅仅停留在：

![](https://images.xiaozhuanlan.com/photo/2020/af0f071c4297caeae54bb8bbe93c8cec.png)

一看 xml 中居然还要写 三元表达式 “这样的逻辑”，便草草地断定：DataBinding 不是个好东西，在声明式编程中书写 UI 逻辑，既不可调试，也不便于察觉和追踪，万一出现问题就麻烦了。

事实果真是这样吗？

## 文章目录一览

-   前言
-   被不假思索地抵制的 DataBinding
-   DataBinding 的目标只有三个
-   DataBinding 问世前的混沌世界
-   DataBinding 为什么能解决这三个问题？
-   **造成人们误认为 DataBinding 是在 xml 中写 UI 逻辑的原因**
-   DataBinding 结合 LiveData 的使用
-   **Note 2019.12.12 加餐**：
    -   LiveData 和 ObservableField 的本质区别
-   **Note 2020.10.21 加餐：**
    -   对 BindingAdapter 存在缘由的解析
-   BindingAdapter 在解决 Drawable 复用问题上的绝妙使用
-   **Note 2020.4.26 加餐**：
    -   DataBinding 参数语法为何如此设计
-   **Note 2020.5.6 加餐**：
    -   BindingMethod 与 BindingAdapter 的本质区别
-   **Note 2020.12.11 加餐：**
    -   ViewBinding 的本质、与 DataBinding 的区别，以及适用场景
    -   **DataBinding 的注意事项 和 屡试不爽的排错技巧**
    -   **Note 2020.8.3 加餐：**
    -   注意自定义视图包名的书写规范
-   **Note 2020.7.27 加餐**：
    -   现有条件下解决 视图调用一致性问题 的最优解
-   综上

## DataBinding 的目标只有三个

同此前的所有文章一样，我写作的目的有且只有一个：帮助读者快速明确状况 —— 究竟是什么原因，导致某项技术的存在、某项技术究竟是为解决什么问题而存在。

所以本文也是一样开门见山 —— DataBinding 的存在，是为了实现唯一的核心目标：

> **通过 基于适配器模式 的 “数据驱动” 思想，规避视图调用的一致性问题**。
> 
> 并且数据驱动 顺带免去了 因调用视图对象 而存在的大量冗余的判空处理。
> 
> 并且数据驱动 顺带免去了 为调用视图对象 而存在的大量样板代码。

如果光是阅读了以上几点，你还是不太理解的话，那接下来我就来介绍 99% 的网文都不曾为你明确的真实状况。

## DataBinding 问世前的混沌世界

例如 3 年前我 自主设计并开发的一款日记软件，该软件包含横、竖屏布局。

![](https://images.xiaozhuanlan.com/photo/2019/2a9293016fe09e13a382d6978a18bd99.png)

该 calendar 日历控件只存在于横屏布局中。

因此如果是以往的开发模式，我们需要在每个 Fragment 刷新 calendar 状态的地方，去逐个调用 calendar 控件，手动 set 新的状态，并且 set 前还要判断该控件存不存在。

> 那么这就造成一致性的问题：因为人类不是机器，人类一定会有疏忽的时候。特别是项目中包含数十个横竖屏不同布局方案、而每个页面又在多个方法中调用多个控件的情况，当后期项目改动，或换个同事接手时，**很容易因疏忽了某些控件的存在与否，而埋下 “空指针异常” 的隐患**。

同理，为了拿到控件最终的状态，我们仍然是通过 调用控件的方式去获取状态，如此，若该控件不存在，可能造成空指针异常；而如果大量判空，代码就不优雅、十分冗余。

> 注：对 “一致性” 的概念不熟悉的朋友可参见[《架构组件 “一致性” 概念》](https://xiaozhuanlan.com/topic/9340256871)篇的完整解析。

除了一致性问题，还有个很明显的问题，就是每新建一个页面，都需要为各种控件 findViewById —— 这些令人难以忍受的样板代码。

何况我从不使用 ButterKnife，因为它仅仅是为了解决样板代码，并且还需要额外地在代码中书写注解，看上去十分蹩脚。

那怎么办呢？

## DataBinding 为什么能解决这三个问题？

首先，数据驱动 意味着，控件的状态被分离到 ViewModel 中管理，并且 **ViewModel 这一层只需负责状态变量本身的变化**，至于该变量在布局中究竟被哪些视图绑定、当前有没有视图来绑定，ViewModel 不用管。

那控件是如何做到被通知且更新状态的呢？

DataBinding 是通过 适配器模式和观察者模式 来管理控件刷新状态。

当状态变量发生变化时，需要开发者手动地完成状态的更新。这将通知 DataBinding 中绑定该变量的控件刷新数据。

![](https://images.xiaozhuanlan.com/photo/2020/0022960c3ea7e5c61fd1d515df483f51.png)

在 BaseObservable 子类中，为 getter 赋予了 [@Bindable](https://xiaozhuanlan.com/u/Bindable) 注解的，在编译后会生成与之相关的 BR 变量，于是 **通过在 setter 中调用 notifyPropertyChanged 方法，可以通知 DataBinding 去刷新绑定该状态的控件**。

> 具体的代码可参考：在 Android 项目结构树状图中，通过 Java（Generate）节点 查看 目录下生成的 DataBindingImpl 文件（例如《重学安卓》项目生成的 FragmentTestDatabindingObserveBindingImpl），其中 executeBindings 方法，就是数据刷新的逻辑所在。

**executeBindings 会依据 [@Bindable](https://xiaozhuanlan.com/u/Bindable) 生成的 dirtyFlags 来区分当前是哪个 BR 被通知变化，从而只更新与之相关的控件**，这么设计的缘由很容易理解：

> 1.为了 **“精准投放”**，而不是所有控件都重绘，从而维持良好的性能。
> 
> 2.当多个控件绑定了同一个状态变量时，这些控件因为 dirtyFlags 一致的缘故，都会被通知，而无需开发者手动在 Java 代码中逐个调用控件去刷新。

但 Observable 的做法 反而滋生了另一种一致性问题：

例如状态变量对应的类中，某些成员要是遗漏了 `notifyPropertyChanged`，这个成员的通知刷新就会不起效。

所以说，数据驱动思想是好的，实现了视图与状态的分离。就是实现上有点不尽人意，居然需要开发者手动去通知。

那怎么办呢？**于是 Google 又设计了一种 更为简便的状态声明方式**：

通过 ObservableField。

于是你只需在 ViewModel 中 **分别声明该页面所包含的状态**，然后在布局中与控件一对一地绑定就 OK 了。

![](https://images.xiaozhuanlan.com/photo/2020/bc46d7a841a95dd9a159b4f25006f8f1.png)

如图，每个 ObservableField 都是持有 基本数据类型 或 String，来 **作为单一的 State 去绑定 布局中对应控件的属性**，从而当从外界为 ObservableField set 新的值、也即 **state 发生变化时，能精准地只通知绑定了的控件发生重绘**。

再就是双向绑定，双向绑定的存在 主要是为了 避免直接调用控件实例来获取数据，来避免视图调用的一致性问题。

比如登录的场景，用户名、密码输入完毕点击 “提交”，那么在双向绑定的帮助下，我们只需通过 State-ViewModel 拿到与 用户名、密码 输入框发生双向绑定的 ObservableField 即可。

双向绑定的书写方式是在普通绑定的基础上加个等号，例如：

```
android:text="@={vm.name}"
```

所以，目前为止，相信大家在 Fragment 等视图控制器中对控件视图的判空，已经趋近于 0 了，因为数据驱动，完全不需要手动调用视图。

所以光是数据驱动，就已经解决了 findViewById 的问题、并且一口气把最开始提到的 3 个目标全解决了。

## 人们误认为 DataBinding 是在 xml 中写 UI 逻辑的原因

回到最初，文章开头提到的那种写法，是 官方文档 给出的具有误导性的 糟糕示例。

![](https://images.xiaozhuanlan.com/photo/2020/746f0ae19a2204024c1800f7a2b5e532.png)

建议别这么写。状态值应尽可能地 直接反映预期结果，而不是作为条件在 xml 中发生逻辑判断。

![](https://images.xiaozhuanlan.com/photo/2020/fe92684a5f1f639664f206458436b660.png)

![](https://images.xiaozhuanlan.com/photo/2020/21dde9cd571d0f12c70ade45c937fb27.png)

并且值得再次强调的是，**DataBinding 不负责 UI 逻辑**，UI 逻辑原本在哪里写，现在还是在哪里写，只不过，UI 逻辑中原本调用控件实例去刷新状态的方式，现在改成了数据驱动，从而规避了“视图调用的一致性问题”。

DataBinding 的核心目标 就仅仅是解决这个问题而已。

简言之，**DataBinding 在布局中的表达式，只是作为 UI 逻辑末端的 状态改变，而并不涉及错综复杂的 UI 逻辑本身**。明确了 DataBinding 的 **职责边界**，无论 末端赋值 的花样再多，也不再会迷乱你双眼。

## DataBinding 结合 LiveData 的使用

LiveData 是标准化开发中不可或缺的一员，通过 LiveData，可以让新手老手都轻易地写出遵循 “唯一可信源 数据分发” 的编码原则，从而避免 90% 不可预期的错误。

从 Android Studio 3.1 起已正式支持 LiveData DataBindingImpl 代码的生成。使用时就是简单地将 ViewModel 中的 ObservableField 替换为 MutableLiveData，并在视图控制器中为 DataBinding 绑定生命周期 Owner：`mBinding.setLifecycleOwner(this);` 就完事了。

![](https://images.xiaozhuanlan.com/photo/2020/190dca4f1d8942916a9f33116e82b51e.png)

关于 LiveData 本身的知识，可以参见[《LiveData 鲜为人知的 身世背景 和 独特使命》](https://xiaozhuanlan.com/topic/0168753249)

### **Note 2019.12.12 加餐**：

#### LiveData 和 ObservableField 的本质区别

既然 ObservableField 和 LiveData 都可以使用，那它俩选择哪个比较好呢？其实它俩各有各的特点，ObservableField 的特点是支持防抖，在 set 的数据不变的情况下，可以避免不必要的重复绘制。LiveData 则没有防抖的特性。所以可以根据场合来选择合适的方案。具体可见评论区 16楼的补充。

以及，现如今我们已确立，数据按场景划分，主要分为 state、request、callback 这三种。**state 场景下的数据，专用于视图绑定**，无论使用 ObservableField 还是 LiveData，都建议只携带一个 “原子类型”，例如 Integer、String 等，如此可以遵循 “开闭原则”，每个状态都是独立的，未来需求发生变动时，只需对单一状态进行增加和删除，而不必修改复合实体。

对于 request 场景（UI 层向数据层请求数据）的数据，通常使用 LiveData，但携带的是 复杂的 非原子类型的 实体类，所以通常在其回调中，手动为上述 state 逐个赋予新的属性值。

![](https://images.xiaozhuanlan.com/photo/2020/1a600ddd915da9e31ef37fd4abb64ec0.png)

具体可参考《最佳实践》的 `ui.state`、`ui.callback` 包下，三种场景 ViewModel 持有的数据的使用。

## Note 2020.10.21 加餐：

### 对 BindingAdapter 存在缘由的解析

BindingAdapter 的存在主要是为了实现一些以往只有拿到控件实例才能做的事，也即它和 “双向绑定” 设计的出发点一样，是以顺应 “规避视图调用的一致性问题” 这个核心目的为前提的、为解决某些场景下的问题而存在的 **延伸设计**。

![](https://images.xiaozhuanlan.com/photo/2020/4e8b55fa63801e4f8299e848d6a9cd60.jpeg)

仔细观察可发现，在自定义的 BindingAdapter 静态方法中，第一个参数对应的是 与该方法发生绑定的视图类型，例如 ImageView，也即 BindingAdapter 的这种 “传参设计” 确保了方法内部使用的视图实例是存在的、不为 null，因为 xml 中声明过了的视图，就发生绑定了，没有声明，就没发生绑定，从而没执行到该方法了，也即 无论何种情况都不会造成 “视图引用的 null 安全问题”。

所以这个设计是不是很巧妙呢~

与此同时，**这样的设计刚好也额外起到了 “代码复用” 的作用**，也即只要定义过一次 BindingAdapter 方法，后续遇到类似的场景和需求就可以直接在 xml 中套用 BindingAdapter 属性了。也许早期的时候需要下点功夫，准备各种 BindingAdapter 方法，然而这是显而易见的 **复利模型**：越是后期，因避免 “重复代码或一致性问题” 而节省下的时间 将是指数级增多。

## BindingAdapter 在解决 Drawable 复用问题上的绝妙使用

主要是涉及到 DataBinding 中 BindingAdapter 的使用。

说到 Drawable，我们都知道，旧时候，当项目中存在圆角需求时，为了布局预览，尽管 drawable 之间仅仅只是圆角半径或背景颜色的不同，我们还是不得不为每个布局都准备一份 drawable xml。久而久之，项目资源目录中存在了数十个这样的 xml 文件。

BindingAdapter 的出现，巧妙地解决了这个问题，使得 Drawable 既可以通过 Java 代码的方式定义而做到复用，又可以在布局中预览。

具体的使用可以参考这个库，有人已经实践过了。

> [https://github.com/whataa/noDrawable](https://github.com/whataa/noDrawable)

![](https://images.xiaozhuanlan.com/photo/2019/2712cdb7e137c6e73ac1eef453105d65.png)

并且除了 Drawable，BindingAdapter 还有许多其他的妙用，等着你去发掘。

## **Note 2020.4.26 加餐**：

### DataBinding 参数语法为何如此设计

> 有小伙伴询问，为什么 DataBinding 绑定的值要设计为在 xml 中声明 `@{ }` 而不是普通的写法，原因如下：

因为 xml 是一种声明式编程，DataBinding 是在 xml layout 基础上的外挂，所以实现上仍然是通过对 xml 的解析来转译为 java 代码。那么 xml 中的 `@{ }` 就很好理解，就是告知此处是留给 DataBinding 的表现机会 —— 在编译时根据 `@{ }` 中的内容生成代码。

`@{ }` 的取值类型 通常取决于 xml 属性的定义，对于控件本身的属性，可在控件的源码中查看；对于基于 BindingAdapter 封装的 binding 属性，可参考 DataBinding bindingadapter 包下的默认实现。

![](https://images.xiaozhuanlan.com/photo/2020/f2d9d4832aa7ad3121433e997b6725a2.png)

> Note 2020.4.27：在 “误认为 DataBinding 在 xml 中写逻辑” 的一节中我们提到，DataBinding 存在 “在 xml 中写逻辑” 这样的过度设计是有原因的：

在上一条补充中我们提到了，xml 中属性的取值类型 取决于控件或 BindingAdapter 对属性的定义。

对于 Visibility，会发现，在 ViewBindingAdapter 中没有与之相关的方法，我想可能是因为 Google 考虑到 Visibility 是三值的情况，包括 visible、gone，以及 invisible，因而采取直接在 xml 中写逻辑、在编译时直接 copy 这段逻辑生成代码。

很显然，这种过度设计 为软件工程埋下了安全隐患，因而目前在项目中都是十分克制地 —— **弱水三千 只取一瓢** —— 只取用作为 DataBinding 核心特点的 “数据驱动” 来点到为止地解决 视图调用的一致性问题，以免额外引入其他不可预期的错误。

至于 Visibility，我通常是考虑封装一个 visible vs gone 的二元对立 boolean 属性，而 invisible 的占位特性 完全可以通过 ConstraintLayout 的 goneMargin 来代替。

## **Note 2020.5.6 加餐**：

### BindingMethod 与 BindingAdapter 的本质区别

> 有小伙伴询问，BindingMethod 和 BindingAdapter 的本质区别是什么呢？

其实很简单，**BindingMethod 专注于解决 “现有属性名与方法名不一致” 的问题**，而不是 BindingAdapter 这种另外自定义一个属性。

如果这样说还不理解的话，那么这里再明示一下 BindingMethod 和 BindingAdapter 的共同背景 —— 就是上上个 Note 中补充的，DataBinding 是根据 xml `@{ }` 内容来编译时生成 java 代码的，

正常情况下，控件定义的 xml 属性或 BindingAdapter，和方法名会对的上，所以生成的 java 代码也是能运行的，但由于历史问题，存在极个别的对不上的情况，比如 ImageView 的 `android:tint` 属性本该对应 setTint 方法，但实际上并没有 setTint，而是 setImageTintList，那么这时候就可以借助 BindingMethod —— **它就只是在 “DataBinding 在编译时生成代码” 这个背景下，帮助做个方法名转换**。

BindingMethod 的具体用法可见 [官方文档](https://developer.android.google.cn/topic/libraries/data-binding/binding-adapters)，这里不做累述。

（感谢读者 [@骑蜗牛去看大海](https://xiaozhuanlan.com/u/5376375709) 补充的状况。总的来说，BindingMethod 的使用和管理并不简便，因为需要另写一个类继承原控件、从而基于该类作注解（在软件工程的背景下，容易造成一致性问题，比如同事不记得或不知道已有该类，而又重复写个）。所以目前项目中一般优先使用 BindingAdapter 来自定义属性，具体可见《最佳实践》项目 ui.base.binding 包下对 BindingAdapter 的使用）

## Note 2020.12.11 加餐：

### ViewBinding 的本质、与 DataBinding 的区别，以及适用场景

> 媒体对 ViewBinding 作用的宣传有点脱离实际，比如 ViewBinding 的作用仅仅是**用于取代 findViewById 或 ButterKnife —— 主要是为了解决 获取视图实例时 类型转换的安全问题**，它自身其实并不能够 “解决 null 安全问题”。
> 
> 如要说解决 null 安全问题，那也需强调是在 kotlin 的背景下解决，因为截至 2020 年，绝大多数企业仍然在使用 Java 开发或维护 Android App。

ViewBinding 的存在，主要是通过 “自动化生成中间代码” 来 **规避 视图实例化时的 “类型转换安全问题”**，

和 DataBinding 的共同点在于通过 “**自动化生成中间代码**” 来隐藏诸如 “视图实例初始化” 等样板代码，以此规避不可预期的错误，

并且，“人如其名”，ViewBinding 和 DataBinding 的区别就在于前者是 “视图绑定”，后者是 “数据绑定”，也即前者不像后者是通过 “可观察数据” 通知所绑定视图实例的方式 来规避 null 安全一致性问题，

因而 **在 Java 下，对视图调用一致性问题的解决，只能依靠 DataBinding**，ViewBinding 光靠自身不足以解决。

而如果是在 kotlin 下，你便多了 ViewBinding 这个选项，因为 kotlin 语言特性使得 **只要是 Java 中被声明了 [@Nullable](https://xiaozhuanlan.com/u/Nullable) 的变量，在 kotlin 中调用时 IDE 会给出显眼的提示要求做判空操作**，而它的判空又是如此简单 —— 一个 `?` 搞定 —— ViewBinding 潜在的 null 安全设计，其实就是围绕这个来做的，ViewBinding 只负责 根据布局控件差异 自动为中间代码添加 [@Nullable](https://xiaozhuanlan.com/u/Nullable) 的声明，

> 划重点 👆 👆 👆

![](https://images.xiaozhuanlan.com/photo/2020/2bd0e746d8a0557aeb9ba57492c3fd38.jpeg)

如图，“横屏布局” 比 “竖屏布局” 少了个 tv\_title\_3 控件，IDE 在开发者代码调用 tv\_title\_3 控件时就能直接检查出并提示，如此开发者随手补个 `?` 就行（比如 `mBinding.tvTitle3?.text = “title3”`）

这样是不是就很方便了呢。

当然很多时候出于复用的考虑，我们也会有 BindingAdapter 那种高内聚低耦合的设计需求，为此在 kotlin 下可以通过 “拓展函数” 来满足。

总之，ViewBinding 是一款适用于 kotlin 语境的 UI 框架，如果你坚持留在 Java 或是 MVVM 开发模式，那么 DataBinding 是你不二之选。

## DataBinding 的注意事项 和 屡试不爽的排错技巧

DataBinding 对应的视图控制器，例如 Fragment 的类名发生了变化，那么 DataBinding 布局中引用的视图控制器的内部类，可能被抹去，需要在视图控制器类名发生变化后手动补充：

![](https://images.xiaozhuanlan.com/photo/2020/08f5670730d7d43381df5fb61fc672ba.png)

会变成

![](https://images.xiaozhuanlan.com/photo/2020/8fc1e53e6bcba6a75efabc3ee227af75.png)

造成这个现象的原因暂不知。

那我是怎么获知这个消息的呢？直接通过 Android Studio 的 EventLog 是看不出的。而且 EventLog 常常放出 “假消息”，让开发者无法真正觉察问题所在。

> 这里介绍一个好招。

在 Android Studio 的 Terminal 中，输入 `gradlew build`（MacOS 下 `./gradlew build`），通过这个来编译，就能精准地打印出问题所在。屡试不爽！

再就是，DataBinding 适合在主干工程（例如 App 模块）中使用，不要在第三方库中使用，因为 DataBinding 的状况是，如果 Library Module 使用了 DataBinding，那么 Application Module 也得使用。

而第三方库是多项目共享的，甚至可能开源给互联网上其他开发者使用的，因而 DataBinding 务必不在 Library 中使用。

此外，DataBinding 需要 root view 包含 layout/布局名\_0，比如 `layout/activity_main_0` 这样的 tag，所以对于对话框等场景，需要在 DataBindingUtil.bind 之前，手动为 inflate 出的 root view setTag。

最后，如在 Adapter item 的 xml 布局中绑定状态，需在 adapter 的 onBindViewHolder 回调末尾执行 binding.executePendingBindings()，这么做的缘由可详见 读者空大 在评论区 9 楼的解析。

## Note 2020.8.3 加餐：

### 注意自定义视图包名的书写规范

根据《Note 2020.4.26 加餐：DataBinding 参数语法为何如此设计》一节我们得以知晓，DataBinding 本质上是一种模板驱动的设计，也即通过事先约定的声明式语法，来生成目标文件。

因而当你在使用自定义控件时，务必注意包名中不能含有大写开头，这是 Java 规则如此，同时模板引擎也是基于 Java 规则来生成文件。

如果你在布局中引入了例如 `<com.kunminx.Test.TestView>` 这样的节点，是会发生 “找不到类” 的情况的，因为模板引擎会误认为这个 TestView 是 Test 类的内部类，也即生成的文件中它是会 `public Test.TestView testView;` 这样写的。

因而正确的做法是遵循 Java 规则，将包名全部改为小写，例如 `com.kunminx.test.TestView`。

## Note 2020.07.27 加餐：

### 现有条件下解决 视图调用一致性问题 的最优解

#### DataBinding 严格模式

DataBinding 严格模式 是我基于对 “**数据驱动 UI 框架的本质即解决视图调用一致性问题**” 的独家理解，而在 2020.04.17 自创的 DataBinding 开发模式。

DataBinding 严格模式通过将 mBinding 实例限制在基类，避免开发者通过 mBinding 拿到 View 实例，从而 **彻底规避** 视图调用的一致性问题、让视图调用的 **安全系数** 与基于函数式编程思想的 Jetpack Compose 持平（理论依据详见[《是 事关软件工程安全 的 数据驱动 UI 框架 扫盲干货》](https://xiaozhuanlan.com/topic/2356748910)）。

> 具体可以参考[《最佳实践》](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)项目的 DataBindingFragment 类。（目前已抽取到远程仓库，可在项目中通过双击 Shift 来搜索）

当然，这样也使传统的属性动画等操作无法编写。

好消息是，基于 ConstraintLayout 的 Motion 动画可以完美解决这一问题 —— **一行代码不用写，全在 xml 中搞定**。并且 Motion 动画将动画的制作难度和成本降低了 90%。具体可参考我们近期组织的 MotionChallenge 活动（提供一学就会的 Motion 动画教程）

[https://github.com/Jetpack-Missionary/MotionChallenge](https://github.com/Jetpack-Missionary/MotionChallenge)

## 综上

DataBinding 的核心目标是通过 “数据驱动” 思想，规避视图调用的一致性问题。

并且因为数据驱动的缘故，无形中免除了样板代码的书写，更是避免了代码中存在大量冗余的判空处理。

**可谓一箭三雕**。

DataBinding 不是用声明式编程的方式来编写 UI 逻辑，它的职责边界仅限于通过数据驱动来规避 “视图调用的一致性问题”，明确了这一点，就不会被作为末端的 状态赋值表达式 迷乱双眼。

而 DataBinding 的 BindingAdapter 是个百宝箱，等着大家去发掘。

> **快捷访问：**
> 
> **[基本功：是随时随地可受用的 深度思考原则](https://xiaozhuanlan.com/topic/9837051426)**
> 
> [GitHub : 重学安卓 配套项目](https://github.com/KunMinX/Relearn-Android)
> 
> [GitHub : Jetpack-MVVM-Best-Practice](https://github.com/KunMinX/Jetpack-MVVM-Best-Practice)
> 
> **Tip：**  
> · 考虑到移动端的阅读体验，已将代码改为图片展示。网页/小程序点击图片可放大查阅。  
> · Jetpack MVVM 系列文章会不定期增量更新，更新的内容会提供 Note 标记，可通过 ctrl + F（Windows）或 command + F（MacOS）搜索 “Note” 快速检索和查阅。

## 版权声明

> Copyright © 2019-present KunMinX

**本文为专栏私有内容，谢绝转载**。

读者订阅了专栏，即享有了专栏文章的阅读权。

文章 **原创的引言、切入点、思路、结论** 凝聚了作者 KunMinX 本人的心血 及与参与者互动演化的结果，作者本人及作者在文中提名的有效贡献者 对相应的成果享有所有权。

**当您 借鉴或引用文中的引言、切入点、思路、结论 进行二次创作并打算发行时，须注明链接出处**，否则我们保留追责的权利。对此如有疑虑请及时沟通，或在发行前将作品交由本人核对。

当您看见有人 [洗稿](https://baike.baidu.com/item/%E6%B4%97%E7%A8%BF/20862574?fr=aladdin)、[剽窃](https://baike.baidu.com/item/%E5%89%BD%E7%AA%83/9398357?fr=aladdin)、未经授权转载本专栏的文章内容时，请及时向本人举报。

最后感谢您对专栏内容的阅读和喜欢。