# 移动端上，WPS可以怎么集成呢？ - 知乎

source:: https://zhuanlan.zhihu.com/p/156628797

前两篇我们讲了沉淀了32年的WPS PC客户端，可对标Google Docs的金山文档（WPS WebOffice）的集成及二次开发的使用场景和能力，今天再介绍另一款明星产品，一个在Google Play上荣膺最佳应用，在Apple App Store常年霸榜的WPS移动端的集成及二次开发能力。

这里要特别强调一点，只有**WPS移动专业版才具有二次开发和集成能力**，个人版不具备。

集成场景，咱们还是站在业务方的角度讲比较易懂，咱们把集成场景分为三大类，依次为：

1.  业务方是原生App集成WPS移动专业版
2.  业务方集成WPS WebOffice
3.  业务方是H5通过第三方提供SDK集成WPS移动专业版

### 业务方是原生App集成WPS移动专业版

业务方是个原生App，可以利用Android和iOS的系统特性，可以控制WPS移动版，WPS移动版的行为也可以广播通知业务方App。通过WPS对外暴露的接口直接对文档进行打开、保存、另存、打开手绘等功能；可控制I/O，完成业务方文件的可控；基于客户端的集成，对接接口标准化，可控性和灵活性比较强。

WPS移动专业版兼容文字、表格、演示、PDF等共计50几种主流文档格式。业务方App可以通过[AIDL](https://developer.android.com/guide/components/aidl)/[Intent](https://developer.android.com/reference/android/content/Intent)(Android)、[URL Scheme](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app)(iOS)方式启动启动WPS移动版，并传递指定的打开文件参数，让WPS进入不同的模式展示文档。

轻量级的[Intent](https://developer.android.com/reference/android/content/Intent)(Android)、[URL Scheme](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app)(iOS)，对应用中一次操作的动作、动作涉及数据、附加数据进行描述，Android/iOS系统根据此描述信息，找到对应的组件，完成组件的调用。该方式常用与业务方打开Office文档。

另外灵活强大的AIDL对接方式，则提供了更丰富和灵活接口操作文档，方便业务方在文档打开后，进行二次编辑操作。

其实这种方式和WPS PC现在主推的[WPS加载项（jsapi）](https://zhuanlan.zhihu.com/p/148803031)的模式有异曲同工之妙，都是可以做到能力平台化，行为可定义，达到「集成松耦合、业务强关联」。

WPS移动专业版针对移动端使用场景，在安全方面也做了很多探索，主要体现在三方面——打开不落地、显示加水印和截屏可禁止。

-   **打开不落地**，实现了文件在移动设备的预览和编辑不落地，不留痕。接收和下载文档数据，全部放置与内存当中，编辑完成后，可以将数据返回给业务调起方，或者云文档服务器，确保了文档的安全使用。

![](https://pic4.zhimg.com/v2-097836ab385b3ad81cce8894a97cf143_b.jpg)

-   **显示加水印**。可通过WPS移动专业版提供的接口，实现对水印内容、密度、透明度的设置，来控制通过WPS移动专业版预览或编辑文档时的水印显示，提高安全性。
-   **截屏可禁止**。截屏动作根据操作系统给出的权限，在Android上可做到禁止此行为，iOS上是弹出安全提示，以此让文档的输出行为受控。

### 业务方集成WPS WebOffice

业务方不想在移动端安装WPS移动专业版，又想拥有WPS提供的文档处理能力，怎么办？可以通过H5页面来集成WPS WebOffice。这部分集成场景和「[什么叫在线编辑](https://zhuanlan.zhihu.com/p/151193725)」中提到的WebOffice的四种集成场景是一致的，无非就是在移动端使用而已。特别说两点：

1.  集成WebOffice，天然是文档不落地，多端操作体验一致，具备多人协同编辑能力的；
2.  集成WebOffice，对于需要调用本地资源的使用场景是不支持的，如手写签批、电子签章，这类场景推荐集成WPS移动专业版实现。

### 业务方是H5通过第三方提供SDK集成WPS移动专业版

业务方是个H5应用，嵌入到了一些第三方的原生App中，那可以通过这个原生App通过第一种方式来集成WPS移动专业版，再通过暴露出JS-SDK接口，建立与业务方的联系，实现业务方集成WPS移动专业版，这个集成关系图如下：

![](https://pic2.zhimg.com/v2-0ee0107932ed74d440bbb0b74f6d17b5_b.jpg)

相关资料详见此文档（以与政务微信集成为例）。

### 移动端综合集成方案

可以把第一种和第二种集成方案做个组合，就形成了在移动端文档处理的综合集成方案，示意图如下：

![](https://pic4.zhimg.com/v2-99db163695541cd9979cc8c4cf2a7aa3_b.jpg)

-   业务方是个**原生App**
-   内嵌WebView通过H5集成WebOffice，**赋能文档协同能力**
-   利用系统通知广播的方式集成WPS移动专业版，**赋能本地编辑处理能力**
-   文档存储在云文档，**赋能集中存储，随时取用的文档「存管用」能力**

### **上段儿代码：集成实践示例-打开文档**

Android中提供了Intent机制来协助应用间的交互与通讯，Intent负责对应用中一次操作的动作、动作涉及数据、附加数据进行描述，Android则根据此Intent的描述，负责找到对应的组件，将Intent传递给调用的组件，并完成组件的调用。Intent在这里起着一个媒体中介的作用，专门提供组件互相调用的相关信息，实现调用者与被调用者之间的解耦。在WPS Office for Android里面，也支持通过Intent调用的这种轻量级对接方式，打开Office文档，下面是一个打开Word文档的示例：

```
// 基本流程如下：
// 1、设置打开文件参数
// 2、将上述设置绑定到intent对象
// 3、startActivity调起WPS
// 具体性详细代码，可参考http://mo.wps.cn/pc-app/Android/demo_11.5.5.zip

Intent intent = new Intent();
Bundle bundle = new Bundle();

//打开文档参数
// bundle.putString(Define.OPEN_MODE, "ReadOnly");      //只读模式
// bundle.putString(Define.OPEN_MODE, "ReadMode");      //打开直接进入阅读模式
// bundle.putString(Define.OPEN_MODE, "EditMode");      //打开直接进入编辑模式
bundle.putString(Define.OPEN_MODE, "Normal");           //正常模式

//文档记录
// bundle.putBoolean(Define.CLEAR_BUFFER, IsIsClearBuffer); //清除临时文件boolean
// bundle.putBoolean(Define.CLEAR_TRACE, true);         //清除使用记录boolean
// bundle.putBoolean(Define.CLEAR_FILE, IsClearFile);        //删除打开文件boolean

if (context.getPackageManager().getApplicationInfo("com.kingsoft.moffice_pro", 8192))
{
intent.setClassName("com.kingsoft.moffice_pro", cn.wps.moffice.documentmanager.PreStartActivity2);
}

File file = new File(path);
Uri uri = Uri.fromFile(file);
intent.setData(uri);
intent.putExtras(bundle);
String type = "application/msword";
intent.setDataAndType(Uri.fromFile(file), type); 

// 打开文档
try 
{
  startActivity(intent);
}
catch (ActivityNotFoundException e) 
{
  e.printStackTrace();
}
```

发布于 2020-07-05 07:37
