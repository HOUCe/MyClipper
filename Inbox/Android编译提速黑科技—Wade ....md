# Android编译提速黑科技—Wade Plugin

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649871943&idx=1&sn=1a2e5c9fc6873c95812c19d175a9a95f&chksm=83bfdf1cb4c8560a2ad2fbd92d7e4b5ac463a11d129100931c34057c622a2aa97d239e1461ec&mpshare=1&scene=1&srcid=1215oT9kyI6gPfWoestEtBBu&sharer_sharetime=1639531026481&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)潘涛 刘望舒

作者：潘涛

![](https://image.cubox.pro/article/2021120220393213313/89109.jpg)

随着得物App业务高速发展，Android项目的代码量与组件数量迅速增加，项目编译时长也明显升高。今年初增量编译平均耗时接近3.9分钟，严重影响了开发效率，也促使我们探索各种措施缩短编译时间提升开发效率。

## **背景**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQPPuFYfQPObYxYmuw5Hic05WueQmWrTZey7YgY1ZiaTgZVibdtAZ40Kia6Q%2F640%3Fwx_fmt%3Dpng)

四月初通过一系列常规优化，如改造增量注解处理器、 增量Transform、组件化、工程化、优化项目配置等等，耗时缩短到2.3分钟。六月，Wade Plugin 第一版上线，进一步缩短到1.3分钟。八月, Wade第二个大版本上线，最终将增编耗时降低到了0.8分钟。本文主要介绍Wade Plugin的技术原理和实现思路。

**简介**-

Wade Plugin是得物Android自研的Gradle插件，用于提升编译速度。常规优化手段用尽以后, 项目的增编耗时仍需2.3分钟, 其中DexArchiveBuilder、MergeProjectDex、 MergeLibDex、MergeExtDex占了1.7分钟。不难看出，如果要进一步降低耗时, 应该挖掘DexBuild和DexMerge的优化空间。

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQxHrKv4HHGtvjEpgWCZDJ6x2Y3MD8kD6icKh4jhjSjjpRXEJecrgtuzg%2F640%3Fwx_fmt%3Dpng)**-

Wade Plugin通过Hook Android原生的编译流程, 将原生的DexBuildTask替换为WadeDexBuild, 原生的DexMergeTask替换为WadeDexMerge。原生DexBuild平均耗时60秒, WadeDexBuild只需12秒； 原生DexMerge平均耗时42秒, WadeDexMerge只需2秒。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQ5n3Y3MuZYXDlRHTkRrTPnMWl1FTBeYmdbOkmCW9hHVic6antVp8caZA%2F640%3Fwx_fmt%3Dpng)

## 

**WadeDexBuild**

**原理**

调用Dx或D8工具完成Class到Dex的转换这一过程称为DexConvert, 它占了DexBuild大部分耗时。原生DexBuild以Jar和Class为粒度执行DexConvert。得物工程中平均1个Jar包含200+个.class, 相当于增量时每改动一个类会触发200个类执行DexConvert。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQkaN6LSg9h2VJfOzib43dVfwtDxBCvqiaDqzoCFgjO9dF4uLibicliaiaYkPw%2F640%3Fwx_fmt%3Dpng)

理想情况是只有改动的Class参与DexConvert.

**优化方案**

在DexConvert执行前, 解压缩Jar, 以.class为粒度执行DexConvert。并且只有其中变更的.class参与, 未变更的.class执行结果复用上次编译缓存。具体有四种变更类型对应的缓存复用策略: 对于新增的.class, 需要参与DexConvert；改动的.class参与DexConvert；移除的.class不参与DexConvert；未改变的也不参与。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQm8WZiaqibh12pOUSxPNAib7KLpV1IHCo7sXWMo5rMWDXSvXAHqpHG2OyA%2F640%3Fwx_fmt%3Dpng)

其中, 移除.class的情况要特殊处理. 例如Demo.class移除后, 除了相应删除产物Demo.dex, 还要寻找它的内部类的产物Demo$1.dex, Demo$2.dex等。

**主要实现**

**输入(Task Inputs)**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQhwPLwEiaCIjJibhviaT9PGcQibUQLSubYXgU5DxEIXRXobJibya7oBgltuQ%2F640%3Fwx_fmt%3Dpng)

首先要根据Consumer Transform决定参与WadeDexBuild的.class文件路径, 消费Transform Inputs的Transform即Consumer Transform。当接入了一个Consumer Transform, 它的输出路径参与编译; 没有Consumer Transform时, Java Compile、Kotlin Compile的输出路径参与编译; 如有多个Consumer Transforms, 取最后一个Transform的输出路径作为DexBuild输入。

**增量编译触发条件**

触发条件决定了本次编译是否走增量逻辑, 以及上次编译的缓存是否可用。WadeDexBuild的增量条件包括五大类共28条(AGP3和AGP4略有不同)：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQmgqj85w54CibnlNPYGnYdrAE327w5f7y0Ao8mJEwDywJXW6GBBIyehQ%2F640%3Fwx_fmt%3Dpng)

*   **Gradle配置**
    

1.  AndroidJarClasspath
    
2.  DesugaringClasspathClasses
    

3.  ErrorFormatMode
    
4.  MinSdkVersion
    

5.  Dexer
    
6.  UseGradleWorkers
    

7.  InBufferSize
    
8.  Debuggable
    

9.  Java8LangSupportType
    
10.  ProjectVariant
    

11.  NumberOfBuckets
    
12.  DxNoOptimizeFlagPresent
    

*   **Wade配置**
    

13.  WadeExtension.scope
    
14.  WadeExtension.duplicateClass
    

15.  WadeExtension.dexBucketSize
    
16.  WadeExtension.jarBucketSize
    

*   **Wade缓存**
    

17.  ProjectWorkspaceDir
    
18.  SubProjectWorkspaceDir
    

19.  ExternalLibWorkspaceDir
    
20.  MixedScopeWorkspaceDir
    

*   **输入文件**
    

21.  ProjectClasses
    
22.  SubProjectClasses
    

23.  ExternalLibClasses
    
24.  MixedScopeClasses
    

*   **产物文件**
    

25.  ProjectOutputDex
    
26.  SubProjectOutputDex
    

27.  ExternalLibOutputDex
    
28.  MixedScopeOutputDex
    

其中Gradle配置相关的条件和原生的触发条件相似。

触发全量编译的情况, 例如Gradle配置中的Dexer由D8 Dexer改为DX Dexer, 上次编译缓存肯定无法复用, 需要重新完整编译。

触发增量编译的情况, 例如修改了一个Kotlin类, 导致输入文件中的MixedScopeClasses有变化, 此时编译缓存应可复用, 则触发增量编译。

### **Dex Convert**

WadeDexBuild关键步骤是将原生Dex Convert由Jar为粒度转换为Class为粒度执行。首先解压缩Jar, 解压后的.class写入缓存目录, 再将参与上次编译的Class与参与本次编译的Class文件逐个对比, 只有新增和变更的Class参与Dex Convert, 移除和未改变的直接删除或沿用对应缓存。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQ90iaPSKPZrsa7mJ7KcCnhsbIu0z6mznwPUUywoqkpVQSLjx1gtoCIeg%2F640%3Fwx_fmt%3Dpng)

**性能优化**

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQxhDMNf0mibKbPgh4ut5eCniamMzczWTxLYOpDSRMJjbKH6JrsNP9FZUw%2F640%3Fwx_fmt%3Dpng)**

Dex Convert粒度由Jar转换为Class后耗时明显降低。但项目中共有423个Jar, 解压后83000+个Class, 导致Dex Convert前解压缩和文件对比两个步骤非常耗时。对这两步的优化主要有三方面。

*   **ForkJoinPool**
    

用ForkJoinPool替代传统的ExecutorService做并发, 因为它的Work Steeling算法特别适合小文件, 任务数特别多的场景, 能够最大化利用CPU空闲时间。

*   **mmap**
    

文件对比是I/O密集型任务, 普通文件流的读写速度较慢。Wade Plugin所有I/O操作都用mmap实现, 包括读、写、拷贝等。文件流替换为mmap对整体速度提升有很明显的效果。

*   **CRC-32代替MD5**
    

对比两文件是否相同的常规做法是先比较文件长度, 再校验文件MD5是否一致。由于Class数量太多, 计算MD5的耗时非常可观。用CRC-32算法计算文件Hash, 作为Checksum来代替MD5能减少文件对比的时间。

CRC-32计算的Checksum可靠性不如MD5, 理论上会有Hash碰撞, 导致修改Class修改后被误判为未修改, 接着使用缓存而非最新文件参与编译, 反映到产物APK上意味着这次修改无效。但是实际发生概率极低, 整体来看值得牺牲理论上的正确性来保证每次编译的效率。

*   **优化效果**
    

优化后解压缩、写缓存平均耗时5700ms, 文件对比耗时得益于CRC-32算法只需10ms, DexBuild整体耗时从原生的60秒降低到12秒。

**![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQDB5wtdElElsvnib2u7RsGRSvujRtBAiaT3qJS92IRbROUIyWvibYAAOlQ%2F640%3Fwx_fmt%3Dpng)**-

## **WadeDexMerge**

**优化方案**

DexMerge通过合并.dex文件来降低APK内Dex文件数量和体积, 提升安装速度和首次运行速度。原生DexMerge的缺点是不支持增量编译, 耗时和Dex文件数量成正比, 得物项目的DexMerge耗时在30～60秒之间。

对于代码量少, 类总数不多的项目可以不执行DexMerge。AGP本身也有自动跳过DexMergingTask的逻辑, 当MinSDKVersion>23时, Dex数量小于500个不会执行DexMerge, MinSDKVersion<23时, Dex数量小于50个则自动跳过DexMerge。

Hook DexMergingTask可以做到忽略AGP的Dex数量阈值强行跳过DexMerge。但对于Dex数非常多的工程, 强行跳过DexMerge的副作用明显, 在得物App上强行跳过会导致包体积增加40M左右、安装APK耗时增加15秒、首次启动耗时增加约10秒。

WadeDexMerge支持了强行跳过DexMerge与增量Merge两种策略, 默认使用增量Merge。跳过DexMerge的实现比较简单, 只需注意随后的PackageTask只识别.dex, 而不能识别.jar, 要先处理DexBuild产物中的.jar文件, 再和.dex产物一起拷贝到PackageTask的inputDir即可, 其中inputDir可以通过反射PackageAndroidArtifact.getDexFolders()获得。这里主要介绍WadeDexMerge增量编译的实现。

**主要实现**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQNRtl6R3PhNCb6zibJ6N7LPzIhlfYBTmJMfhXL6EYV0SQh2jFmoBvzog%2F640%3Fwx_fmt%3Dpng)

DexMerge输入文件有.jar和.dex, 输出.dex文件。增量实现的核心是对输入文件作分桶, 只对变更的桶Merge, 其他桶复用缓存。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQId3hiacPfkLkJUzhtmNkh515Iuk7nlgOc0ltBsx4z54c5oCLcibWXRTQ%2F640%3Fwx_fmt%3Dpng)-

假设本次编译只有Bucket0中一个文件发生变更, 其他Bucket均无变化, 那么只需对Bucket0做Merge。分桶后, 需要找出本次编译相比于上次编译变更了哪些文件以及它们的变更类型。这个场景类似于经典算法题“如何找出两个数组中不相同的元素?”，因此可以用快慢指针来计算文件变更。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQgaNkianQCZfxy8eNSpkg6fKOibDQic78RH8lTjcp2eVvYXAvE4ldoqImQ%2F640%3Fwx_fmt%3Dpng)

如图，慢指针指向上次编译的文件数组, 快指针指向本次编译的文件数组, 对比两个指针的文件, 如果相同则快指针指向下一个文件, 直到找到不同, 此时慢指针指向下一个文件, 再开始下一轮对比。伪代码如下：

    long fast = 0

桶总数和桶内文件数(Bucket Size)直接影响到增量效果。理论上, 分桶越多越好, 如果有100个Bucket, 相当于增量只需1/100的全量Merge时间。但Bucket越多意味着APK内.dex越多, 又会影响到包体积、安装时间和首次启动耗时。经过多次试验, Bucket总数在50～100个时综合效果最好, Merge耗时降低明显, 副作用也不大。目前得物工程中共有66个Bucket, 其中Jar类型23个, Dex类型43个。

**高可用**

在高可用建设方面, 主要通过数据统计、建立编译情况监控、编译指标周报及时获取大盘情况和发现问题; 兼容不同AGP和Gradle版本以提高插件的兼容性; 持续监控编译异常并迭代修复问题提高稳定性。

**七大指标**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQkF9SOdQ07FBicWVoLRW0xBdodJGmgDGPreUW0SXjmqo9BeuTdYlGyFg%2F640%3Fwx_fmt%3Dpng)

七个指标反映团队的编译总体情况：

*   增量编译耗时
    
*   平均编译耗时
    

*   全量编译耗时
    
*   增量编译耗时50分位值
    

*   增量占比
    
*   编译成功率
    

*   人均编译总时长
    

指标的计算依赖埋点数据上报, 埋点中部分字段的值较难获取。例如本次编译的JavaCompileTask是否为增量, 需通过对AGP和Gradle插桩实现, 有三处Hook点可以切入。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQcGH0bPIkBicxLFnia41Ggk27qhhaHAMORnqtYUdcNmB398cLJicWXHNJQ%2F640%3Fwx_fmt%3Dpng)

Wade早期版本使用方案一, 实际使用发现Hook Gradle的类兼容性较差。目前使用方案二, Hook AGP的`com.android.build.gradle.tasks.JavaCompileCreationAction`类, 注入`WadeJavaCompile`类代替原生的`org.gradle.api.tasks.compile.JavaCompile`类。`WadeJavacCompile`是`JavaCompile`的包装类, 重写`compile()`取到Javac的增量标识`inputs.isIncremental`. 伪代码如下:

    public class WadeJavaCompile extends JavaCompile {

对AGP原生类的Hook过程大致可分为3步, 获取Gradle的`VisitableURLClassLoader`, 用ASM或Javassist编辑目标类的字节码, 反射调用`ClassLoader.defineClass()`加载编辑后的字节码。

Gradle进程和Gradle Daemon进程一般常驻后台, Android Studio打开后第一次编译会触发加载AGP类的字节码, 之后再编译都不会触发类加载, 所以只有一次Hook机会, 必须保证Hook的字节码比AGP"抢先"加载到`VisitableURLClassLoader`。因此, Wade插件接入要求在Root Project中`apply wade plugin`, 以确保Hook代码能在App Project的`apply android plugin`之前执行。

**兼容性**

主要兼容了AGP3和AGP4、Gradle5和Gradle6两套版本。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQS7BOrnXYJZvuvzicuGujeZY1zV6CVnRJa0glBLw1LsTuzuWXqADpAlw%2F640%3Fwx_fmt%3Dpng)

插件中的关键步骤如增量编译触发条件、反射获取Consumer Transform、WadeDexMergeTask等都针对不同版本分别做了适配。

**稳定性**

实际使用过程中遇到了各种疑难杂症, 这里列出前10个常见异常。

1.  java.io.IOException: The input doesn't contain any classes. Did you specify the proper '-injars' options?
    
2.  java.io.FileNotFoundException: /Users/panes/app/build/intermediates/compile\_and\_runtime\_not\_namespaced\_r\_class\_jar/debug/R.jar (No such file or directory)
    

3.  Caused by: com.android.tools.r8.utils.b: Error:YeezyCompleteListener.class, Type com.xxx is defined multiple times
    
4.  Caused by: org.gradle.api.UncheckedIOException:java.util.zip.ZipException: error in opening zip file
    

5.  Caused by: com.android.tools.r8.utils.b: Error: Class content provided for type descriptor xxx.r actually defines class com.xxx.R
    
6.  A failure occurred while executing com.android.build.gradle.internal.tasks.Workers$ActionFacade
    

7.  com.android.builder.dexing.DexArchiveMergerException: Error while merging dex archives: Type com.xxx.R is defined multiple times
    
8.  base.apk code is missing
    

9.  Archive is not readable : /Users/panes/android/app/build/intermediates/mixed\_scope\_dex\_archive/developerDebug/out/c6795cc73f81ff9c1c0b5d0adb06b1b4161c540cbf761ba11415aae4856b11b4\_4.jar
    
10.  Could not determine dependencies of app:wadeInputChangesInspect
    

经过近30个版本的迭代, 这些问题都已解决。最近版本v2.6.4上线至今经历6800次编译, 异常次数4次。

**基准测试**

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FAAQtmjCc74Bv8iaV4r6ZsQERWTEQtkJtQjjanGLRGJZjZXZhhenY9NPJo8ECINXvogFxOTHAVotk23R0x44zwrw%2F640%3Fwx_fmt%3Dpng)

Benchmark跑分显示, 10次增量编译(只改动一行代码)的平均耗时14.4秒, 10次无量编译(代码不变)平均耗时6.2秒。跑分时清理后台任务、关闭了其他占用资源的进程, 但实际编译环境比理想环境复杂得多, 基准测试只用于验证理论是否有效。

### 

**总结**

Wade Plugin开发过程中困难重重, 重写Android原生的编译流程做到既大幅提升速度又保证稳定可靠并非易事。其中还有更多细节未介绍到, 如增编时识别热点代码、复用文件变更计算结果、Hook PackageTask做Apk内文件兜底防止出包异常。同时也期待后续版本能有更多提升。

![](https://image.cubox.pro/article/2021072814541436152/32739.jpg)

-

##  

 

• [耗时2年，Android进阶三部曲第三部《Android进阶指北》出版！](http://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649855723&idx=1&sn=98fcd0dadac3b3e9d50cd2d517558f9a&chksm=83bf9fb0b4c816a6c1b2fed4a14372144836ef45a51221c6d70120bd89239c79a9bf4363c310&scene=21#wechat_redirect)

 

• [『BATcoder』做了多年安卓还没编译过源码？一个视频带你玩转！](http://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649859294&idx=1&sn=480ba6d8efeb39401f70455748d99771&chksm=83bfad85b4c82493487fbb9b43c4a016ab423a10e329560e22823e84e7d42fa8739d3eabaa94&scene=21#wechat_redirect)

 

• [『BATcoder』我去！安装Ubuntu还有坑？](http://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649859294&idx=1&sn=480ba6d8efeb39401f70455748d99771&chksm=83bfad85b4c82493487fbb9b43c4a016ab423a10e329560e22823e84e7d42fa8739d3eabaa94&scene=21#wechat_redirect)

 

• [重生！进阶三部曲第一部《Android进阶之光》第2版 出版！](http://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649865174&idx=1&sn=82dee0996110598a16b277b7afa15e42&chksm=83bfb28db4c83b9b3ceb95f76a7ff1637c9b986f235f2c40132cac0bf9cac417cd214f4d819f&scene=21#wechat_redirect)

 

 

### ** BATcoder技术**群，让一部分人先进大厂

 

 

大家好，我是刘望舒，腾讯最具价值专家TVP，著有三本业内知名畅销书，连续四年蝉联电子工业出版社年度优秀作者，百度百科收录的资深技术专家。

 

想要**加入 ****BATcoder技术群，公号回复****`BAT`**** 即可。**

 

### 为了防止失联，欢迎关注我的小号

 

 **  微信改了推送机制，真爱请星标本公号👇** 

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzAxMTg2MjA2OA==&mid=2649871943&idx=1&sn=1a2e5c9fc6873c95812c19d175a9a95f&chksm=83bfdf1cb4c8560a2ad2fbd92d7e4b5ac463a11d129100931c34057c622a2aa97d239e1461ec&mpshare=1&scene=1&srcid=1215oT9kyI6gPfWoestEtBBu&sharer_sharetime=1639531026481&sharer_shareid=b7c991d3cd23094f535ad602a652c37b#rd)