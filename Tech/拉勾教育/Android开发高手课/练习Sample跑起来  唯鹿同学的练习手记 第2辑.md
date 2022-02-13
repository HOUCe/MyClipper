---
created: 2021-10-12
source: https://time.geekbang.org/column/article/69958
author: 张绍文前微信高级工程师，Tinker负责人
---

# [练习Sample跑起来 | 唯鹿同学的练习手记 第2辑](https://time.geekbang.org/column/article/69958)


唯鹿 2019-03-05

你好，我是唯鹿。

接着上篇练习手记，今天练习 6～8、12、17、19 这六期内容（主要针对有课后 Sample 练习的），相比 1～5 期轻松了很多。

该项目展示了使用 PLT Hook 技术来获取 Atrace 的日志，可以学习到 systrace 的一些底层机制。

没有什么问题，项目直接可以运行起来。运行项目后点击开启 Atrace 日志，然后就可以在 Logcat 日志中查看到捕获的日志，如下：

11:40:07.031 8537-8552/com.dodola.atrace I/HOOOOOOOOK: ========= install systrace hoook =========

11:40:07.034 8537\-8537/com.dodola.atrace I/HOOOOOOOOK: ========= B|8537|Record View

11:40:07.034 8537\-8552/com.dodola.atrace I/HOOOOOOOOK: ========= B|8537|DrawFrame

11:40:07.035 8537\-8552/com.dodola.atrace I/HOOOOOOOOK: ========= B|8537|syncFrameState

\========= B|8537|prepareTree

\========= E

\========= E

\========= B|8537|eglBeginFrame

\========= E

\========= B|8537|computeOrdering

\========= E

\========= B|8537|flush drawing commands

\========= E

11:40:07.036 8537\-8552/com.dodola.atrace I/HOOOOOOOOK: ========= B|8537|eglSwapBuffersWithDamageKHR

\========= B|8537|setSurfaceDamage

\========= E

11:40:07.042 8537\-8552/com.dodola.atrace I/HOOOOOOOOK: ========= B|8537|queueBuffer

\========= E

11:40:07.043 8537\-8552/com.dodola.atrace I/HOOOOOOOOK: ========= B|8537|dequeueBuffer

\========= E

\========= E

\========= E

通过 B|事件和 E|事件是成对出现的，这样就可以计算出应用执行每个事件使用的时间。那么上面的 Log 中 View 的 draw() 方法显示使用了 9ms。

这里实现方法是使用了Profilo的 PLT Hook 来 hook libc.so 的write与\_\_write\_chk方法。libc 是 C 的基础库函数，为什么要 hook 这些方法，需要我们补充 C、Linux 相关知识。

同理Chapter06-plus展示了如何使用 PLT Hook 技术来获取线程创建的堆栈，README 有详细的实现步骤介绍，我就不赘述了。

这个 Sample 是学习如何给代码加入 Trace Tag，大家可以将这个代码运用到自己的项目中，然后利用 systrace 查看结果。这就是所谓的 systrace + 函数插桩。

操作步骤：

使用 Android Studio 打开工程 Chapter07。

运行 Gradle Task :systrace-gradle-plugin:buildAndPublishToLocalMaven编译 plugin 插件。

使用 Android Studio 单独打开工程 systrace-sample-android。

编译运行 App（插桩后的 class 文件在目录Chapter07/systrace-sample-android/app/build/systrace\_output/classes中查看）。

对比一下插桩效果，插桩前：

![](https://static001.geekbang.org/resource/image/65/a5/6587c2a9e0d4cae3b5336cbf9ef91da5.jpeg)

插桩后:

![](https://static001.geekbang.org/resource/image/08/13/0895158565b8548d86d2e076325c0713.jpeg)

可以看到在方法执行前后插入了 TraceTag，这样的话beginSection方法和endSection方法之间的代码就会被追踪。

public class TraceTag {

@TargetApi(Build.VERSION\_CODES.JELLY\_BEAN\_MR2)

public static void i(String name) {

Trace.beginSection(name);

}

@TargetApi(Build.VERSION\_CODES.JELLY\_BEAN\_MR2)

public static void o() {

Trace.endSection();

}

其实 Support-Compat 库中也有类似的一个TraceCompat，项目中可以直接使用。

然后运行项目，打开 systrace：

python $ANDROID\_HOME/platform-tools/systrace/systrace.py gfx view wm am pm ss dalvik app sched -b 90960 -a com.sample.systrace -o test.log.html

![](https://static001.geekbang.org/resource/image/d1/0e/d17230ca7e70b399101b1ce66dd35e0e.jpeg)

最后打开生成的 test.log.html 文件就可以查看 systrace 记录：

![](https://static001.geekbang.org/resource/image/e0/4b/e093fc650bfeafd053eb845a1b1d9c4b.jpeg)

当然，这一步我们也可以使用 SDK 中的 Monitor，效果是一样的。

使用 systrace + 函数插桩的方式，我们就可以很方便地观察每个方法的耗时，从而针对耗时的方法进行优化，尤其是 Application 的启动优化。

该项目展示了关闭掉虚拟机的 class verify 后对性能的影响。

在加载类的过程有一个 verify class 的步骤，它需要校验方法的每一个指令，是一个比较耗时的操作。这个例子就是通过 Hook 去掉 verify 这个步骤。该例子尽量在 Dalvik 下执行，在 ART 下的效果并不明显。

去除校验代码（可以参看阿里的Atlas）：

AndroidRuntime runtime = AndroidRuntime.getInstance();

runtime.init(this.getApplicationContext(), true);

runtime.setVerificationEnabled(false);

具体运行效果这里我就不展示了，直接运行体验就可以了。

通过复写 Application 的getSharedPreferences替换系统SharedPreferences的实现，核心的优化在于修改了 Apply 的实现，将多个 Apply 方法在内存中合并，而不是多次提交。

修改SharedPreferencesImpl的 Apply 部分如下：

public void apply() {

final MemoryCommitResult mcr = commitToMemory();

boolean hasDiskWritesInFlight = false;

synchronized (SharedPreferencesImpl.this) {

hasDiskWritesInFlight = mDiskWritesInFlight > 0;

}

if (!hasDiskWritesInFlight) {

final Runnable awaitCommit = new Runnable() {

public void run() {

try {

mcr.writtenToDiskLatch.await();

} catch (InterruptedException ignored) {

}

}

};

QueuedWork.add(awaitCommit);

Runnable postWriteRunnable = new Runnable() {

public void run() {

awaitCommit.run();

QueuedWork.remove(awaitCommit);

}

};

SharedPreferencesImpl.this.enqueueDiskWrite(mcr, postWriteRunnable);

}

notifyListeners(mcr);

这个是全面解析 SQLite 的资料，有兴趣的可以下载看看。

该项目展示了如何使用 PLT Hook 技术来获取网络请求相关信息。

通过 PLT Hook，代理 Socket 相关的几个重要函数：

\* 直接 hook 内存中的所有so，但是需要排除掉socket相关方法本身定义的libc(不然会出现循坏)

\* plt hook

\*/

void hookLoadedLibs() {

ALOG("hook\_plt\_method");

hook\_plt\_method\_all\_lib("libc.so", "send", (hook\_func) &socket\_send\_hook);

hook\_plt\_method\_all\_lib("libc.so", "recv", (hook\_func) &socket\_recv\_hook);

hook\_plt\_method\_all\_lib("libc.so", "sendto", (hook\_func) &socket\_sendto\_hook);

hook\_plt\_method\_all\_lib("libc.so", "recvfrom", (hook\_func) &socket\_recvfrom\_hook);

hook\_plt\_method\_all\_lib("libc.so", "connect", (hook\_func) &socket\_connect\_hook);

}

int hook\_plt\_method\_all\_lib(const char\* exclueLibname, const char\* name, hook\_func hook) {

if (refresh\_shared\_libs()) {

return \-1;

}

int failures = 0;

for (auto const& lib : allSharedLibs()) {

if (strcmp(lib.first.c\_str(), exclueLibname) != 0) {

failures += hook\_plt\_method(lib.first.c\_str(), name, hook);

}

}

return failures;

}

17:08:28.347 12145\-12163/com.dodola.socket E/HOOOOOOOOK: socket\_connect\_hook sa\_family: 10

17:08:28.349 12145\-12163/com.dodola.socket E/HOOOOOOOOK: stack:com.dodola.socket.SocketHook.getStack(SocketHook.java:13)

java.net.PlainSocketImpl.socketConnect(Native Method)

java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:334)

java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:196)

java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:178)

java.net.SocksSocketImpl.connect(SocksSocketImpl.java:356)

java.net.Socket.connect(Socket.java:586)

com.android.okhttp.internal.Platform.connectSocket(Platform.java:113)

com.android.okhttp.Connection.connectSocket(Connection.java:196)

com.android.okhttp.Connection.connect(Connection.java:172)

com.android.okhttp.Connection.connectAndSetOwner(Connection.java:367)

com.android.okhttp.OkHttpClient$1.connectAndSetOwner(OkHttpClient.java:130)

com.android.okhttp.internal.http.HttpEngine.connect(HttpEngine.java:329)

com.android.okhttp.internal.http.HttpEngine.sendRequest(HttpEngine.java:246)

com.android.okhttp.internal.huc.HttpURLConnectionImpl.execute(HttpURLConnection

AF\_INET6 ipv6 IP===>183.232.231.173:443

socket\_connect\_hook sa\_family: 1

Ignore local socket connect

02\-07 17:08:28.637 12145\-12163/com.dodola.socket E/HOOOOOOOOK: respond:<!DOCTYPE html>

<html><!--STATUS OK--><head><meta charset="utf-8"\><title>百度一下,你就知道</title>

可以看到我们获取到了网络请求的相关信息。

最后，我们可以通过 Connect 函数的 hook，实现很多需求，例如：

使用 Java Hook 实现 Alarm、WakeLock 与 GPS 的耗电监控。

实现原理

实现核心代码：

Object oldObj = mHostContext.getSystemService(Context.XXX\_SERVICE);

Class<?> clazz = oldObj.getClass();

Field field = clazz.getDeclaredField("mService");

field.setAccessible(true);

final Object mService = field.get(oldObj);

setProxyObj(mService);

Object newObj = Proxy.newProxyInstance(this.getClass().getClassLoader(), mService.getClass().getInterfaces(), this);

field.set(oldObj, newObj)

写了几个调用方法去触发，通过判断对应的方法名来做堆栈信息的输出。

输出的堆栈信息如下：

![](https://static001.geekbang.org/resource/image/43/b7/43e081eacdef050b78b71043c87acdb7.png)

当然，强大的 Studio 在 3.2 后也有了强大的耗电量分析器，同样可以监测到这些信息，如下图所示（我使用的 Studio 版本为 3.3）。

![](https://static001.geekbang.org/resource/image/82/3d/82b2939991e4c9c729017255fb1cb73d.png)

实现不足之处：

可能兼容性上不是特别完善（期待老师的标准答案）。

没有按照耗电监控的规则去做一些业务处理。

心得体会：

本身并不复杂，只是为了找到 Hook 点，看了对应的 Service 源码耗费了一些时间，对于它们的工作流程有了更深的认识。

平时也很少使用动态代理，这回查漏补缺，一次用了个爽。

这个作业前前后后用了一天时间，之前作业还有一些同学提供 PR，所以相对轻松些，但这次没有参考，走了点弯路，不过收获也是巨大的。我就不细说了，感兴趣的话可以参考我的实现。完整代码参见GitHub，仅供参考。

参考

![unpreview](https://static001.geekbang.org/resource/image/d0/d3/d0c4b5596074fea8cba12cyya9c297d3.png)

© 版权归极客邦科技所有，未经许可不得传播售卖。 页面已增加防盗追踪，如有侵权极客邦将依法追究其法律责任。
