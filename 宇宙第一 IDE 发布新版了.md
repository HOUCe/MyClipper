# 宇宙第一 IDE 发布新版了

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247511718&idx=1&sn=e0df3815e1de89dcbb8844a859178e68&chksm=e8790bdcdf0e82ca3baa4a9befc2e300c3b88eca429287cb8327cd04b17e5d0e274795860f77&mpshare=1&scene=1&srcid=1203uv8MV6od7m6FvRIzQsSg&sharer_sharetime=1638486785109&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)点击关注 👉 秦子帅

**👇推荐大家关注一个公众号👇**

 **正文**

## ****大家好，我是秦哥。****

**前言**

Visual Studio 2022 正式版于发布。新版本带有 go-live 许可证，可供生产使用。在 Visual Studio 2019 的基础上，新版集成开发坏境提供了非常多的改进，包括对 64 位、.NET 6 和 C++ 20 的支持，为核心调试器提供更好的性能，并在实时共享会话中支持文本聊天。

发布活动：https://visualstudio.microsoft.com/zh-hans/launch/

下载地址：https://visualstudio.microsoft.com/zh-hans/downloads/

**Visual Studio 2022 的主要功能:**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fj0ROiac4adEvrVVPnnY31RL6xLYW8EdhjtgJKmI4MDlOvT2VVAJkcJ41IiaBicC5Hh66hgcibsnEgaafrGHlRia2UIg%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fj0ROiac4adEvrVVPnnY31RL6xLYW8EdhjJWppgSTJUNcLTSLOnOwicDTmOibRI11M2V5h4xicPWjIyNKAZ9B4FOdiaw%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2Fj0ROiac4adEvrVVPnnY31RL6xLYW8Edhj9VyFCA3Wia5MzO55K7uMqx2G3PIsYrnfdF7YI4AYSUVGPsHsoX6UtaA%2F640%3Fwx_fmt%3Dpng)-

**64 位**

devenv.exe 现在只有 64 位

**Azure Cloud Services**

现已支持 Azure Cloud Service (classic) 和 Azure Cloud Service (extended support) 项目

**C++**

*   v143 构建工具现在可以通过 Visual Studio 安装程序以及独立的构建工具使用。
    
*   当在调试器下运行时，新的热重新加载体验现在可用于本地 C++ 应用程序。它同时支持 MSBuild 和 CMake 项目。更多信息请看"热重载"部分。
    
*   你现在可以在 WSL2 上本地构建和调试，而无需建立 SSH 连接。跨平台的 CMake 项目和基于 MSBuild 的 Linux 项目都被支持。
    
*   Visual Studio 现在支持 CMakePresets.json 中的 buildPresets.target 选项。这允许你在你的 CMake 项目中构建一个目标子集。
    
*   精简了 CMake 项目中的项目菜单，并提供了"删除缓存和重新配置"以及"查看缓存"的选项。
    
*   更新了 CMake 概述页面以支持 CMakePresets.json。
    
*   实施了 /scanDependencies 标志，用于输出 CMake 项目的 C++20 模块依赖关系，如 P1689r3 中所述。这是朝着支持用 CMake 构建基于模块的项目迈出的一步，我们正在努力在以后的版本中完成这一支持。
    
*   现在你可以用 LLDB 从 Visual Studio 调试运行在远程系统上的进程。
    
*   微软把随 Visual Studio 一起发布的 CMake 版本升级到了 3.21 版。有关可用内容的详细信息，请参见 CMake 3.21 发布说明。
    
*   与 Visual Studio 一起提供的 LLVM 工具已经升级到了 LLVM 12。详情请参见 LLVM 发布说明。
    
*   MSVC 工具集现在默认在调试记录中使用 SHA-256 源代码散列。此前，该工具集默认使用 MD5 进行源代码散列。
    
*   使用 C++ 进行游戏开发的工作负载现在可以安装最新的虚幻引擎，并支持 Visual Studio 2022。
    
*   在为导入的模块和头单元的类型提供导航和语法高亮时，对 C++ 智能感应进行了改进。
    
*   通过优化缓存头的使用和符号数据库的访问，改进了 C++ 智能感应的性能，提供了改进的加载时间以进入你的代码。
    
*   适用于 C++ 的 IntelliSense Code Linter 现在是默认开启的，提供即时的 as-you-type 建议和常见代码缺陷的修复建议。
    
*   在开关-fsanitize=fuzzer 下支持 libfuzzer。更多细节见文档。
    
*   我们改进了代码分析工具中的空指针解除引用检测。
    
*   代码分析现在强制要求必须检查带有_Check\_return_或_Must\_inspect\_result_注释的函数的返回值。
    
*   在代码分析中添加了对 gsl::not\_null 的支持。
    
*   在 C++ 移动开发的工作量中更新到 NDK r21 LTS。
    
*   C++ AMP 头文件现在已被废弃。在 C++ 项目中包含<amp.h>会产生构建错误。要消除这些错误，请定义\_SILENCE\_AMP\_DEPRECATION\_WARNINGS。请参阅 AMP 弃用链接以了解更多细节。
    

**调试和诊断**

*   附加到进程对话框的改进
    
*   异常帮助器的改进
    
*   强制运行点击
    
*   内存转储的诊断分析
    
*   微软发布了一种新的断点类型，叫做依赖性断点，它允许你配置一个断点，使其只在另一个断点被首先击中时才被启用。
    
*   为 Extrenal Sources 节点添加了更多的更新，现在你可以在子节点"无源模块"下看到模块，并以 Solution explorer 本身的形式加载符号。
    
*   破解点沟槽的改进
    
*   临时断点
    
*   拖放断点
    
*   解决方案资源管理器中的外部源节点
    
*   附加到流程对话框的改进
    

**个性化设计**

*   为垂直和水平标签添加颜色标签
    
*   增加了主题包，并与 VS Code 主题作者合作，推出了自定义主题集合
    
*   建立了主题转换器，将 VS Code 主题转换到 Visual Studio 2022 中使用。
    
*   增加了将 Visual Studio 主题与 Windows 主题同步的功能
    
*   增加了新的文档管理功能，包括自定义标签宽度，加粗活动文档，以及 docwell 中额外的关闭按钮。
    

**编辑器**

*   增加了子词导航功能
    
*   自动保存现在可以作为一个预览功能使用
    
*   多键复制/粘贴体验
    

**可扩展性**

*   从 Microsoft.VisualStudio.Language.Client 程序集中删除了 API
    
*   VSSDK包含几个突破性的变化，Visual Studio 2019 的扩展在 2022 年将无法使用。更多信息请参见 VSSDK 文档。
    
*   VS SDK 参考程序集不再被安装到 VSSDK\\VisualStudioIntegration\\Common\\Assemblies 文件夹中。如果您的构建依赖于这些程序集，请将您的项目迁移到使用 NuGet 包来代替。对于离线的情况。
    
*   保留一个 org 内的 nuget feed，从那里恢复 nuget 包。
    
*   检查安装文件。
    
*   增加了 ILanguageClient 的突破性变化修复
    

**云服务**

*   Azurite 将被用于 Azure Storage 的本地仿真，而不是旧的、不再积极开发的 Azure Storage 仿真器。
    

**Git 工具**

*   对任何跨越不同存储库的解决方案（即在不同 Git 存储库中托管项目的解决方案）的预览标志下的多存储库支持
    
*   在创建 git 仓库的过程中，现在完全支持发布到 Azure DevOps。
    
*   状态栏的增强，包括从空 VS 查看和打开仓库的新功能，并显示未拉动提交的数量
    
*   Git Changes 窗口的溢出菜单现在可用于仅有本地仓库的额外 git 操作
    
*   统一的 Diff 工具栏，包含添加/删除的行数和可发现的配置选项
    
*   提交细节的改进，包括一个更灵敏和用户友好的用户界面
    

**帮助菜单**

*   在 17.0 版本中，我们重新设计了帮助菜单，包括入门材料和有用的提示/技巧。
    
*   通过添加诸如访问开发者社区、发行说明、Visual Studio 产品路线图和我们的社交媒体页面，提供了与我们开发团队的更多合作。
    
*   另外搜索公众号Java架构师技术回复关键字"idea”获取一份惊喜礼包。
    

**热重载体验**

*   热重载现在可以通过 Visual Studio 调试器向 .NET 开发人员提供，对于许多 .NET 6 应用程序类型，不需要调试器。
    
*   在使用 Visual Studio 调试器时，热重载现在可供 C++ 开发人员使用。
    

**IntelliCode**

*   整行补全可以根据你当前的上下文预测你的下一段 C# 代码，并在你的光标右边以内联建议的形式呈现。
    
*   整行补全现在与 JetBrains ReSharper 的最新版本兼容。请注意，不支持基于 ReSharpers 自定义补全列表项目选择的行补全上下文的更新--如果需要，ReSharper 用户可以选择使用 Visual Studio 本地 IntelliSense 来代替，如这里的文档所示
    

**JavaScript/TypeScript**

*   微软已经发布了一个新的 JavaScript/TypeScript 项目类型，它可以用额外的工具构建独立的 JavaScript/TypeScript 项目。你将能够在 Visual Studio 中使用你电脑上安装的框架版本创建 Angular 和 React 项目。
    
*   JavaScript 和 TypeScript 测试现在可以在 Visual Studio Test Explorer 中进行。
    
*   NPM GUI 可用，所以你现在可以像下载 Nuget 包一样下载 NPM 模块了
    

**.NET 6 SDK**

*   .NET 6 SDK 已包含在 Visual Studio 2022 中。
    

**.NET 生产力**

*   引入参数重构可以将一个新的参数从方法实现转移到其调用者。
    
*   用于数据流分析的跟踪值源
    
*   可以选择在被重新分配的变量下划线
    
*   在生成覆盖物对话框中增加了搜索选项
    
*   XML <code>标签的快速信息现在可以保留空白和 CDATA 块
    
*   查找所有引用窗口现在可以对多目标项目进行分组
    
*   重构以删除 Visual Basic 中重复的类型
    
*   转到实现将不再导航到具有抽象声明的成员，这些成员也被重写了。
    
*   从 Solution Explorer 中同步命名空间以匹配您的文件夹结构
    
*   从 Solution Explorer 中配置后台代码分析
    
*   对于新的 .NET 项目，现在默认启用了 Nullable 引用类型。
    
*   C# 10.0 文件范围的命名空间重构
    
*   现在默认情况下，导航到反编译的源码是打开的。
    
*   重构为优先于类型检查的空值检查
    
*   当一个方法明确抛出异常时，XML 注释现在会自动生成一个<exception>标签
    
*   继承保证金现在是默认启用的。
    

**编程语言**

*   C#10
    

**Razor (ASP.NET Core) 编辑器**

*   减少了用户界面的冻结，提高了解决方案启动时的性能
    
*   在一些解决方案中，语义着色速度加快，达到 2 倍。
    
*   在 Razor 文件中支持 F7（查看代码）。
    
*   Razor 文件中的片段支持，将通过一个标签完成片段会话，而不是按标签-标签。
    
*   当有嵌套的 HTML 和 Razor 组件时，在@code 块中有更好的格式化。
    
*   在 Razor 文件中支持热重新加载
    
*   性能改进
    
*   格式化和缩进的改进
    
*   新的 Razor 编辑器颜色
    
*   TagHelpers 现在是彩色的，支持快速信息分类和完成工具提示
    
*   Razor 结构的角括号突出显示和导航
    
*   评论现在具有自动完成、智能缩进、自动包含评论的延续和块状评论导航功能
    

**远程测试**

*   非常早期的实验性预览，能够在远程环境中运行测试，如 linux 容器、WSL 和通过 SSH 连接。
    

**测试工具支持**

*   在测试资源管理器中显示
    
*   从 17.0 开始的测试平台的新版本将不能运行通用测试和有序测试。这些特定的功能只作为 MSTestv1 早期版本的一部分，不包括在 MSTestv2 中。我们看到这些功能的使用率非常低，而且有序测试现在被认为是与最佳测试实践相违背的。
    
*   在 17.0 中，一些测试经验将不可用，包括创建新的 TestSettings 文件和 TestSettings 编辑器。测试运行将仍然能够使用 TestSettings 文件，然而 TestSettings 被 RunSettings 所取代，我们鼓励用户迁移改善性能和功能。阅读更多。
    
*   Web 负载测试和 Coded UI 测试支持更新。编码 UI 测试和\[Web 负载测试\](基于云的负载测试服务终结 Azure DevOps 博客（microsoft.com）在 2019 年正式废弃。为了尽量减少对用户的影响，在 Visual Studio 2022 中对这些功能的支持是最低的。我们强烈建议用户取消 Coded UI Test 和 Web Load Test。
    
*   UWP 扩展 SDK 的工具箱人口
    
*   UWP 扩展 SDK 现在需要明确声明他们希望出现在工具箱中的类型，在他们的 SdkManifest.xml 文件中列出它们。旧版本的 Visual Studio 的行为没有改变；它们将忽略清单中的控件列表，而是动态地列举 SDK 程序集中的控件类型。
    

**受信任的地点**

*   改进了"信任设置"功能，现在只要在 IDE 中打开不受信任的代码（如文件、项目或文件夹），就会显示警告。
    
*   信任检查现在是在解决方案文件夹级别进行的。
    
*   用户创建的项目会自动添加到信任列表中
    
*   用户可以跳过对 Visual Studio 创建的临时位置的信任检查
    

**更新、LTSC 和部署**

*   通过 Visual Studio 2022，将有多个同时支持的服务基线在秋季和春季发布。更多细节请参考 Visual Studio 发布节奏文档和 Visual Studio 2022 产品生命周期。
    
*   Visual Studio 2022 附带的新安装程序现在可以配置 Visual Studio 产品从哪里获得更新。这允许你从不同的 LTSC 中选择更新，或者，如果你在一个受管理的企业环境中，你可以配置客户端从一个布局中获得其更新。
    
*   配置更新源的能力是 Visual Studio 安装程序附带的新功能，因此该行为也适用于 Visual Studio 的下级版本，如 Visual Studio 2019。有关配置更新渠道的其他信息，请参考 Update Visual Studio 文档。关于使其适用于网络布局的其他信息，请参阅《Visual Studio 管理员指南》。
    
*   IT 管理员现在可以在没有安装 Visual Studio 的情况下报告问题。
    

**用户界面**

*   默认图标已被更新和刷新。
    

**网络工具**

*   发布摘要页面现在有启动/停止远程调试和分析的操作，在"托管"部分的右上角的"..."菜单下。
    
*   连接的服务"页面现在有一个动作来启动存储资源管理器
    
*   .NET 6 附带的"ASP.NET Core Empty"模板正在使用新的"最小 API"范式，我们已经开始为其添加支持。
    
*   Azurite 将被用于 Azure Storage 的本地仿真，而不是旧的、不再积极开发的 Azure Storage 仿真器。
    
*   你可以通过 Visual Studio 中的"连接服务"体验，使用微软身份认证平台为你的 ASP.NET Core 应用程序添加认证。
    

**.NET 框架的 WPF XAML 设计器**

*   当前的 WPF XAML Designer for .NET Framework 被一个新的 WPF XAML Designer for .NET Framework 所取代，它基于用于 WPF XAML Designer for .NET（.NET Core）的相同架构。
    
*   Visual Studio 的体验将看起来是一样的，但第三方控件供应商需要支持新的可扩展性模型，因为以前基于 .design.dll 和 Microsoft.Windows.Design.Extensibility 的模型已经被废弃。
    
*   如果你已经为 .NET（.NET Core）创建了一个 .designtools.dll 扩展，同样的扩展将适用于新的 WPF XAML Designer for .NET Framework。关于如何迁移到新的可扩展性模型的进一步信息，请参考下面的迁移文档。
    

　　**XAML 热重载**

*   XAML Hot Reload 的变化--对应用内的工具栏和设置的微小变化
    

　　**XAML 实时预览**

*   XAML 实时预览现在可用于 WPF、UWP、WinUI 和 Xamarin.Forms 开发人员在 Android 模拟器或作为 UWP 桌面应用程序运行他们的应用程序。实时预览可以捕获正在运行的应用程序的用户界面，并将其带入 Visual Studio 中的一个停靠窗口。
    
*   这使得使用 XAML Hot Reload 来改变应用程序更容易，同时在 Visual Studio 内部看到这些变化，而不需要在运行中的应用程序和 Visual Studio 之间来回切换，同时进行实时 XAML 代码修改。
    
*   欲了解更多信息，请点击上面的链接。
    

**XAML 样本数据**

*   当在 WPF 应用程序中从工具箱中创建 DataGrid、ListBox 和 ListView 控件时，设计时示例数据现在将被默认添加。要禁用这种行为，请取消勾选"在元素创建时自动添加样本数据"，在工具->选项->XAML 设计器下。
    
*   要了解更多关于样本数据的信息，请访问样本数据文档。
    

**改进的 XAML 绑定体验**

*   微软做了很多改进，使数据绑定变得快速和简单，比如从属性检查器快速访问数据绑定对话框，能够从快速操作中设置绑定，能够在数据绑定对话框中选择要绑定的属性。
    

**你还有什么想要补充的吗？**-

 

 

**PS：**欢迎在留言区留下你的观点，一起讨论提高。如果今天的文章让你有新的启发，欢迎转发分享给更多人。-

 

 

>  
> 
> 版权申明：内容来源网络，版权归原创者所有。除非无法确认，我们都会标明作者及出处，如有侵权烦请告知，我们会立即删除并表示歉意。谢谢!
> 
>  

 

 

 

 

**\- END -**

 

 

 

 **【IT搬砖圈】各种黑科技，IT技术博文！** 

 

![](https://image.cubox.pro/article/2021111821555841253/83502.jpg)

 

 

**（更多精彩值得期待……）**

 

## ![](https://image.cubox.pro/article/2021071922493383101/71760.jpg "音符")

 

 

 

##  

 

**推荐我的微信号**

 

 

![](https://image.cubox.pro/article/2021091017551750074/76931.jpg)

 

 

 **加微信送Android，Java,Python资料****，交流学习** 

 

##  

 

我的视频号，已经录制了几十期,有工具推荐，有教育知识、副业赚钱但都是原创用心输出。-

 

## 

![](https://image.cubox.pro/article/2021091017551750013/79339.jpg)

 

 

 

让我知道你在看

 

 

 ![](https://image.cubox.pro/article/2021091017551737567/60935.jpg) 

 

 

 

 

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzIyNTY4NjU0OQ==&mid=2247511718&idx=1&sn=e0df3815e1de89dcbb8844a859178e68&chksm=e8790bdcdf0e82ca3baa4a9befc2e300c3b88eca429287cb8327cd04b17e5d0e274795860f77&mpshare=1&scene=1&srcid=1203uv8MV6od7m6FvRIzQsSg&sharer_sharetime=1638486785109&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)