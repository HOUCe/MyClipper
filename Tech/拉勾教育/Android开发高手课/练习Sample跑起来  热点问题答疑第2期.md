---
created: 2021-10-12
source: https://time.geekbang.org/column/article/69958
author: 张绍文前微信高级工程师，Tinker负责人
---

# [练习Sample跑起来 | 热点问题答疑第2期](https://time.geekbang.org/column/article/69958)


你好，我是孙鹏飞。今天我们基于专栏第 5 期的练习 Sample 以及热点问题，我来给你做答疑。有关上一期答疑，你可以点击这里查看。

为了让同学们可以进行更多的实践，专栏第 5 期 Sample 采用了让你自己实现部分功能的形式，希望可以让你把专栏里讲的原理可以真正用起来。

前面几期已经有同学通过 Pull request 提交了练习作业，这里要给每位参与练习、提交作业的同学点个赞。

第 5 期的作业是根据系统源码来完成一个 CPU 数据的采集工具，并且在结尾我们提供了一个案例让你进行分析。我已经将例子的实现提交到了GitHub上，你可以参考一下。

在文中提到，“当发生 ANR 的时候，Android 系统会打印 CPU 相关的信息到日志中，使用的是ProcessCpuTracker.java”。ProcessCpuTracker 的实现主要依赖于 Linux 里的 /proc 伪文件系统（in-memory pseudo-file system），主要使用到了 /proc/stat、/proc/loadavg、/proc/\[pid\]/stat、/proc/\[pid\]/task 相关的文件来读取数据。在 Linux 中有很多程序都依赖 /proc 下的数据，比如 top、netstat、ifconfig 等，Android 里常用的 procrank、librank、procmem 等也都以此作为数据来源。关于 /proc 目录的结构在 Linux Man Pages 里有很详细的说明，在《Linux/Unix 系统编程手册》这本书里，也有相关的中文说明。

关于 proc 有一些需要说明的地方，在不同的 Linux 内核中，该目录下的内容可能会有所不同，所以如果要使用该目录下的数据，可能需要做一些版本上的兼容处理。并且由于 Linux 内核更新速度较快，文档的更新可能还没有跟上，这就会导致一些数据和文档中说明的不一致，尤其是大量的以空格隔开的数字数据。这些文件其实并不是真正的文件，你用 ls 查看会发现它们的大小都是 0，这些文件都是系统虚拟出来的，读取这些文件并不会涉及文件系统的一系列操作，只有很小的性能开销，而现阶段并没有类似文件系统监听文件修改的回调，所以需要采用轮询的方式来进行数据采集。

下面我们来看一下专栏文章结尾的案例分析。下面是这个示例的日志数据，我会通过分析数据来猜测一下是什么原因引起，并用代码还原这个情景。

usage: CPU usage 5000ms(from 23:23:33.000 to 23:23:38.000):

System TOTAL: 2.1% user + 16% kernel + 9.2% iowait + 0.2% irq + 0.1% softirq + 72% idle

CPU Core: 8

Load Average: 8.74 / 7.74 / 7.36

Process:com.sample.app

50% 23468/com.sample.app(S): 11% user + 38% kernel faults:4965

Threads:

43% 23493/singleThread(R): 6.5% user + 36% kernel faults：3094

3.2% 23485/RenderThread(S): 2.1% user + 1% kernel faults：329

0.3% 23468/.sample.app(S): 0.3% user + 0% kernel faults：6

0.3% 23479/HeapTaskDaemon(S): 0.3% user + 0% kernel faults：982

\\.\\.\\.

上面的示例展示了一段在 5 秒时间内 CPU 的 usage 的情况。初看这个日志，你可以收集到几个重要信息。

1\. 在 System Total 部分 user 占用不多，CPU idle 很高，消耗多在 kernel 和 iowait。

2.CPU 是 8 核的，Load Average 大约也是 8，表示 CPU 并不处于高负载情况。

3\. 在 Process 里展示了这段时间内 sample app 的 CPU 使用情况：user 低，kernel 高，并且有 4965 次 page faults。

4\. 在 Threads 里展示了每个线程的 usage 情况，当前只有 singleThread 处于 R 状态，并且当前线程产生了 3096 次 page faults，其他的线程包括主线程（Sample 日志里可见的）都是处于 S 状态。

根据内核中的线程状态的宏的名字和缩写的对应，R 值代表线程处于 Running 或者 Runnable 状态。Running 状态说明线程当前被某个 Core 执行，Runnable 状态说明线程当前正在处于等待队列中等待某个 Core 空闲下来去执行。从内核里看两个状态没有区别，线程都会持续执行。日志中的其他线程都处于 S 状态，S 状态代表TASK\_INTERRUPTIBLE，发生这种状态是线程主动让出了 CPU，如果线程调用了 sleep 或者其他情况导致了自愿式的上下文切换（Voluntary Context Switches）就会处于 S 状态。常见的发生 S 状态的原因，可能是要等待一个相对较长时间的 I/O 操作或者一个 IPC 操作，如果一个 I/O 要获取的数据不在 Buffer Cache 或者 Page Cache 里，就需要从更慢的存储设备上读取，此时系统会把线程挂起，并放入一个等待 I/O 完成的队列里面，在 I/O 操作完成后产生中断，线程重新回到调度序列中。但只根据文中这个日志，并不能判定是何原因所引起的。

还有就是 SingleThread 的各项指标都相对处于一个很高的情况，而且产生了一些 faults。page faluts 分为三种：minor page fault、major page fault 和 invalid page fault，下面我们来具体分析。

minor page fault 是内核在分配内存的时候采用一种 Lazy 的方式，申请内存的时候并不进行物理内存的分配，直到内存页被使用或者写入数据的时候，内核会收到一个 MMU 抛出的 page fault，此时内核才进行物理内存分配操作，MMU 会将虚拟地址和物理地址进行映射，这种情况产生的 page fault 就是 minor page fault。

major page fault 产生的原因是访问的内存不在虚拟地址空间，也不在物理内存中，需要从慢速设备载入，或者从 Swap 分区读取到物理内存中。需要注意的是，如果系统不支持zRAM来充当 Swap 分区，可以默认 Android 是没有 Swap 分区的，因为在 Android 里不会因为读取 Swap 而发生 major page fault 的情况。另一种情况是 mmap 一个文件后，虚拟内存区域、文件磁盘地址和物理内存做一个映射，在通过地址访问文件数据的时候发现内存中并没有文件数据，进而产生了 major page fault 的错误。

根据 page fault 发生的场景，虚拟页面可能有四种状态：

第一种，未分配；

第二种，已经分配但是未映射到物理内存；

第三种，已经分配并且已经映射到物理内存；

第四种，已经分配并映射到 Swap 分区（在 Android 中此种情况基本不存在）。

通过上面的讲解并结合 page fault 数据，你可以看到 SingleThread 你一共发生了 3094 次 fault，根据每个页大小为 4KB，可以知道在这个过程中 SingleThread 总共分配了大概 12MB 的空间。

下面我们来分析 iowait 数据。既然有 iowait 的占比，就说明在 5 秒内肯定进行了 I/O 操作，并且 iowait 占比还是比较大的，说明当时可能进行了大量的 I/O 操作，或者当时由于其他原因导致 I/O 操作缓慢。

从上面的分析可以猜测一下具体实现，并且在读和写的时候都有可能发生。由于我的手机写的性能要低一些，比较容易复现，所以下面的代码基于写操作实现。

File f = new File(getFilesDir(), "aee.txt");

FileOutputStream fos = new FileOutputStream(f);

byte\[\] data = new byte\[1024 \* 4 \* 3000\];

for (int i = 0; i < 30; i++) {

Arrays.fill(data, (byte) i);

fos.write(data);

}

fos.flush();

fos.close();

上面的代码抓取到的 CPU 数据如下。

E/ProcessCpuTracker: CPU usage from 5187ms to 121ms ago (2018\-12\-28 08:28:27.186 to 2018\-12\-28 08:28:32.252):

40% 24155/com.sample.processtracker(R): 14% user + 26% kernel / faults: 5286 minor

thread stats:

35% 24184/SingleThread(S): 11% user + 24% kernel / faults: 3055 minor

2.1% 24174/RenderThread(S): 1.3% user + 0.7% kernel / faults: 384 minor

1.5% 24155/.processtracker(R): 1.1% user + 0.3% kernel / faults: 95 minor

0.1% 24166/HeapTaskDaemon(S): 0.1% user + 0% kernel / faults: 1070 minor

100% TOTAL(): 3.8% user + 7.8% kernel + 11% iowait + 0.1% irq + 0% softirq + 76% idle

Load: 6.31 / 6.52 / 6.66

可以对比 Sample 中给出的数据，基本一致。

通过上面的说明，你可以如法炮制去分析 ANR 日志中相关的数据来查找性能瓶颈，比如，如果产生大量的 major page fault 其实是不太正常的，或者 iowait 过高就需要关注是否有很密集的 I/O 操作。

## 相关资料

欢迎你点击“请朋友读”，把今天的内容分享给好友，邀请他一起学习。
