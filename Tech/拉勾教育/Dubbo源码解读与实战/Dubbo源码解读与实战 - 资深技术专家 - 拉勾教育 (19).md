---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256
author: 
---

# [Dubbo源码解读与实战 - 资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256)


在[第 17 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4269)中，我们详细介绍了 dubbo-remoting-api 模块中 Transporter 相关的核心抽象接口，本课时将继续介绍 dubbo-remoting-api 模块的其他内容。这里我们依旧从 Transporter 层的 RemotingServer、Client、Channel、ChannelHandler 等核心接口出发，介绍这些核心接口的实现。

### AbstractPeer 抽象类

首先，我们来看 AbstractPeer 这个抽象类，它同时实现了 Endpoint 接口和 ChannelHandler 接口，如下图所示，它也是 AbstractChannel、AbstractEndpoint 抽象类的父类。

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/58/F3/Ciqc1F9wb8eAHyD_AAFkwn8xp18694.png)

AbstractPeer 继承关系

> Netty 中也有 ChannelHandler、Channel 等接口，但无特殊说明的情况下，这里的接口指的都是 Dubbo 中定义的接口。如果涉及 Netty 中的接口，会进行特殊说明。

AbstractPeer 中有四个字段：一个是表示该端点自身的 URL 类型的字段，还有两个 Boolean 类型的字段（closing 和 closed）用来记录当前端点的状态，这三个字段都与 Endpoint 接口相关；第四个字段指向了一个 ChannelHandler 对象，AbstractPeer 对 ChannelHandler 接口的所有实现，都是委托给了这个 ChannelHandler 对象。从上面的继承关系图中，我们可以得出这样一个结论：AbstractChannel、AbstractServer、AbstractClient 都是要关联一个 ChannelHandler 对象的。

### AbstractEndpoint 抽象类

我们顺着上图的继承关系继续向下看，AbstractEndpoint 继承了 AbstractPeer 这个抽象类。AbstractEndpoint 中维护了一个 Codec2 对象（codec 字段）和两个超时时间（timeout 字段和 connectTimeout 字段），在 AbstractEndpoint 的构造方法中会根据传入的 URL 初始化这三个字段：

```
public AbstractEndpoint(URL url, ChannelHandler handler) {
super(url, handler); 
this.codec = getChannelCodec(url);
this.timeout = url.getPositiveParameter(TIMEOUT_KEY,
         DEFAULT_TIMEOUT);
this.connectTimeout = url.getPositiveParameter(
    Constants.CONNECT_TIMEOUT_KEY, Constants.DEFAULT_CONNECT_TIMEOUT);
}
```

在[第 17 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4269)介绍 Codec2 接口的时候提到它是一个 SPI 扩展点，这里的 AbstractEndpoint.getChannelCodec() 方法就是基于 Dubbo SPI 选择其扩展实现的，具体实现如下：

```
protected static Codec2 getChannelCodec(URL url) {
    String codecName = url.getParameter(Constants.CODEC_KEY, "telnet");
if (ExtensionLoader.getExtensionLoader(Codec2.class).hasExtension(codecName)) { // 通过ExtensionLoader加载并实例化Codec2的具体扩展实现
return ExtensionLoader.getExtensionLoader(Codec2.class).getExtension(codecName);
    } else { 
    }
}
```

另外，AbstractEndpoint 还实现了 Resetable 接口（只有一个 reset() 方法需要实现），虽然 AbstractEndpoint 中的 reset() 方法比较长，但是逻辑非常简单，就是根据传入的 URL 参数重置 AbstractEndpoint 的三个字段。下面是重置 codec 字段的代码片段，还是调用 getChannelCodec() 方法实现的：

```
public void reset(URL url) {
try {
if (url.hasParameter(Constants.CODEC_KEY)) {
this.codec = getChannelCodec(url);
        }
    } catch (Throwable t) {
        logger.error(t.getMessage(), t);
    }
}
```

### Server 继承路线分析

AbstractServer 和 AbstractClient 都实现了 AbstractEndpoint 抽象类，我们先来看 AbstractServer 的实现。AbstractServer 在继承了 AbstractEndpoint 的同时，还实现了 RemotingServer 接口，如下图所示：

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/58/F3/Ciqc1F9wb-iAMAgtAACJWi59iSc812.png)

AbstractServer 继承关系图

**AbstractServer 是对服务端的抽象，实现了服务端的公共逻辑**。AbstractServer 的核心字段有下面几个。

-   localAddress、bindAddress（InetSocketAddress 类型）：分别对应该 Server 的本地地址和绑定的地址，都是从 URL 中的参数中获取。bindAddress 默认值与 localAddress 一致。
    
-   accepts（int 类型）：该 Server 能接收的最大连接数，从 URL 的 accepts 参数中获取，默认值为 0，表示没有限制。
    
-   executorRepository（ExecutorRepository 类型）：负责管理线程池，后面我们会深入介绍 ExecutorRepository 的具体实现。
    
-   executor（ExecutorService 类型）：当前 Server 关联的线程池，由上面的 ExecutorRepository 创建并管理。
    

在 AbstractServer 的构造方法中会根据传入的 URL初始化上述字段，并调用 doOpen() 这个抽象方法完成该 Server 的启动，具体实现如下：

```
public AbstractServer(URL url, ChannelHandler handler) {
super(url, handler); 
    localAddress = getUrl().toInetSocketAddress();
    String bindIp = getUrl().getParameter(Constants.BIND_IP_KEY, getUrl().getHost());
int bindPort = getUrl().getParameter(Constants.BIND_PORT_KEY, getUrl().getPort());
if (url.getParameter(ANYHOST_KEY, false) || NetUtils.isInvalidLocalHost(bindIp)) {
        bindIp = ANYHOST_VALUE;
    }
    bindAddress = new InetSocketAddress(bindIp, bindPort);
this.accepts = url.getParameter(ACCEPTS_KEY, DEFAULT_ACCEPTS);
this.idleTimeout = url.getParameter(IDLE_TIMEOUT_KEY, DEFAULT_IDLE_TIMEOUT);
try {
        doOpen(); 
    } catch (Throwable t) {
throw new RemotingException("...");
    }
    executor = executorRepository.createExecutorIfAbsent(url);
}
```

#### ExecutorRepository

在继续分析 AbstractServer 的具体实现类之前，我们先来了解一下 ExecutorRepository 这个接口。

ExecutorRepository 负责创建并管理 Dubbo 中的线程池，该接口虽然是个 SPI 扩展点，但是只有一个默认实现—— DefaultExecutorRepository。在该默认实现中维护了一个 ConcurrentMap<String, ConcurrentMap<Integer, ExecutorService>> 集合（data 字段）缓存已有的线程池，第一层 Key 值表示线程池属于 Provider 端还是 Consumer 端，第二层 Key 值表示线程池关联服务的端口。

DefaultExecutorRepository.createExecutorIfAbsent() 方法会根据 URL 参数创建相应的线程池并缓存在合适的位置，具体实现如下：

```
public synchronized ExecutorService createExecutorIfAbsent(URL url) {
    String componentKey = EXECUTOR_SERVICE_COMPONENT_KEY;
if (CONSUMER_SIDE.equalsIgnoreCase(url.getParameter(SIDE_KEY))) {
        componentKey = CONSUMER_SIDE; 
    }
    Map<Integer, ExecutorService> executors = data.computeIfAbsent(componentKey, k -> new ConcurrentHashMap<>());
    Integer portKey = url.getPort();
    ExecutorService executor = executors.computeIfAbsent(portKey, k -> createExecutor(url));
return executor;
}
```

在 createExecutor() 方法中，会通过 Dubbo SPI 查找 ThreadPool 接口的扩展实现，并调用其 getExecutor() 方法创建线程池。ThreadPool 接口被 @SPI 注解修饰，默认使用 FixedThreadPool 实现，但是 ThreadPool 接口中的 getExecutor() 方法被 @Adaptive 注解修饰，动态生成的适配器类会优先根据 URL 中的 threadpool 参数选择 ThreadPool 的扩展实现。ThreadPool 接口的实现类如下图所示：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/58/FE/CgqCHl9wcBeAYMZ1AABRTGzl5uY627.png)

ThreadPool 继承关系图

不同实现会根据 URL 参数创建不同特性的线程池，这里以**CacheThreadPool**为例进行分析：

```
public Executor getExecutor(URL url) {
    String name = url.getParameter(THREAD_NAME_KEY, DEFAULT_THREAD_NAME);
int cores = url.getParameter(CORE_THREADS_KEY, DEFAULT_CORE_THREADS);
int threads = url.getParameter(THREADS_KEY, Integer.MAX_VALUE);
int queues = url.getParameter(QUEUES_KEY, DEFAULT_QUEUES);
int alive = url.getParameter(ALIVE_KEY, DEFAULT_ALIVE);
return new ThreadPoolExecutor(cores, threads, alive, TimeUnit.MILLISECONDS,
            queues == 0 ? new SynchronousQueue<Runnable>() :
                    (queues < 0 ? new LinkedBlockingQueue<Runnable>()
                            : new LinkedBlockingQueue<Runnable>(queues)),
new NamedInternalThreadFactory(name, true), new AbortPolicyWithReport(name, url));
}
```

再简单说一下其他 ThreadPool 实现创建的线程池。

-   **LimitedThreadPool**：与 CacheThreadPool 一样，可以指定核心线程数、最大线程数以及缓冲队列长度。区别在于，LimitedThreadPool 创建的线程池的非核心线程不会被回收。
    
-   **FixedThreadPool**：核心线程数和最大线程数一致，且不会被回收。
    

上述三种类型的线程池都是基于 JDK ThreadPoolExecutor 线程池，在核心线程全部被占用的时候，会优先将任务放到缓冲队列中缓存，在缓冲队列满了之后，才会尝试创建新线程来处理任务。

EagerThreadPool 创建的线程池是 EagerThreadPoolExecutor（继承了 JDK 提供的 ThreadPoolExecutor），使用的队列是 TaskQueue（继承了LinkedBlockingQueue）。该线程池与 ThreadPoolExecutor 不同的是：在线程数没有达到最大线程数的前提下，EagerThreadPoolExecutor 会优先创建线程来执行任务，而不是放到缓冲队列中；当线程数达到最大值时，EagerThreadPoolExecutor 会将任务放入缓冲队列，等待空闲线程。

EagerThreadPoolExecutor 覆盖了 ThreadPoolExecutor 中的两个方法：execute() 方法和 afterExecute() 方法，具体实现如下，我们可以看到其中维护了一个 submittedTaskCount 字段（AtomicInteger 类型），用来记录当前在线程池中的任务总数（正在线程中执行的任务数+队列中等待的任务数）。

```
public void execute(Runnable command) {
    submittedTaskCount.incrementAndGet(); 
try {
super.execute(command); 
    } catch (RejectedExecutionException rx) {
final TaskQueue queue = (TaskQueue) super.getQueue();
try {
if (!queue.retryOffer(command, 0, TimeUnit.MILLISECONDS)) {
                submittedTaskCount.decrementAndGet();
throw new RejectedExecutionException("Queue capacity is full.", rx);
            }
        } catch (InterruptedException x) {
            submittedTaskCount.decrementAndGet();
throw new RejectedExecutionException(x);
        }
    } catch (Throwable t) { 
        submittedTaskCount.decrementAndGet();
throw t;
    }
}
protected void afterExecute(Runnable r, Throwable t) {
    submittedTaskCount.decrementAndGet(); 
}
```

看到这里，你可能会有些疑惑：没有看到优先创建线程执行任务的逻辑啊。其实重点在关联的 TaskQueue 实现中，它覆盖了 LinkedBlockingQueue.offer() 方法，会判断线程池的 submittedTaskCount 值是否已经达到最大线程数，如果未超过，则会返回 false，迫使线程池创建新线程来执行任务。示例代码如下：

```
public boolean offer(Runnable runnable) {
int currentPoolThreadSize = executor.getPoolSize();
if (executor.getSubmittedTaskCount() < currentPoolThreadSize) {
return super.offer(runnable);
    }
if (currentPoolThreadSize < executor.getMaximumPoolSize()) {
return false;
    }
return super.offer(runnable);
}
```

线程池最后一个相关的小细节是 AbortPolicyWithReport ，它继承了 ThreadPoolExecutor.AbortPolicy，覆盖的 rejectedExecution 方法中会输出包含线程池相关信息的 WARN 级别日志，然后进行 dumpJStack() 方法，最后才会抛出RejectedExecutionException 异常。

我们回到 Server 的继承线上，下面来看基于 Netty 4 实现的 NettyServer，它继承了前文介绍的 AbstractServer，实现了 doOpen() 方法和 doClose() 方法。这里重点看 doOpen() 方法，如下所示：

```
protected void doOpen() throws Throwable {
    bootstrap = new ServerBootstrap(); 
    bossGroup = NettyEventLoopFactory.eventLoopGroup(1, "NettyServerBoss");
    workerGroup = NettyEventLoopFactory.eventLoopGroup(
            getUrl().getPositiveParameter(IO_THREADS_KEY, Constants.DEFAULT_IO_THREADS),
"NettyServerWorker");
final NettyServerHandler nettyServerHandler = new NettyServerHandler(getUrl(), this);
    channels = nettyServerHandler.getChannels();
    bootstrap.group(bossGroup, workerGroup)
            .channel(NettyEventLoopFactory.serverSocketChannelClass())
            .option(ChannelOption.SO_REUSEADDR, Boolean.TRUE)
            .childOption(ChannelOption.TCP_NODELAY, Boolean.TRUE)
            .childOption(ChannelOption.ALLOCATOR, PooledByteBufAllocator.DEFAULT)
            .childHandler(new ChannelInitializer<SocketChannel>() {
@Override
protected void initChannel(SocketChannel ch) throws Exception {
int idleTimeout = UrlUtils.getIdleTimeout(getUrl());
                    NettyCodecAdapter adapter = new NettyCodecAdapter(getCodec(), getUrl(), NettyServer.this);
                    ch.pipeline()
                            .addLast("decoder", adapter.getDecoder())
                            .addLast("encoder", adapter.getEncoder())
                            .addLast("server-idle-handler", new IdleStateHandler(0, 0, idleTimeout, MILLISECONDS))
                            .addLast("handler", nettyServerHandler);
                }
            });
    ChannelFuture channelFuture = bootstrap.bind(getBindAddress());
    channelFuture.syncUninterruptibly(); 
    channel = channelFuture.channel();
}
```

看完 NettyServer 实现的 doOpen() 方法之后，你会发现它和简易版 RPC 框架中启动一个 Netty 的 Server 端基本流程类似：初始化 ServerBootstrap、创建 Boss EventLoopGroup 和 Worker EventLoopGroup、创建 ChannelInitializer 指定如何初始化 Channel 上的 ChannelHandler 等一系列 Netty 使用的标准化流程。

其实在 Transporter 这一层看，功能的不同其实就是注册在 Channel 上的 ChannelHandler 不同，通过 doOpen() 方法得到的 Server 端结构如下：

![5.png](https://s0.lgstatic.com/i/image/M00/59/E4/Ciqc1F9y4LaAIHSsAADBytWDQ3U695.png)

NettyServer 模型

#### 核心 ChannelHandler

下面我们来逐个看看这四个 ChannelHandler 的核心功能。

首先是**decoder 和 encoder**，它们都是 NettyCodecAdapter 的内部类，如下图所示，分别继承了 Netty 中的 ByteToMessageDecoder 和 MessageToByteEncoder：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/58/FE/CgqCHl9wcESANfPCAABDUdzhtNU066.png)

还记得 AbstractEndpoint 抽象类中的 codec 字段（Codec2 类型）吗？InternalDecoder 和 InternalEncoder 会将真正的编解码功能委托给 NettyServer 关联的这个 Codec2 对象去处理，这里以 InternalDecoder 为例进行分析：

```
private class InternalDecoder extends ByteToMessageDecoder {
protected void decode(ChannelHandlerContext ctx, ByteBuf input, List<Object> out) throws Exception {
        ChannelBuffer message = new NettyBackedChannelBuffer(input);
        NettyChannel channel = NettyChannel.getOrAddChannel(ctx.channel(), url, handler);
do {
int saveReaderIndex = message.readerIndex();
            Object msg = codec.decode(channel, message);
if (msg == Codec2.DecodeResult.NEED_MORE_INPUT) {
                message.readerIndex(saveReaderIndex);
break;
            } else {
if (msg != null) { 
                    out.add(msg);
                }
            }
        } while (message.readable());
    }
}
```

你是不是发现 InternalDecoder 的实现与我们简易版 RPC 的 Decoder 实现非常相似呢？

InternalEncoder 的具体实现就不再展开讲解了，你若感兴趣可以翻看源码进行研究和分析。

接下来是**IdleStateHandler**，它是 Netty 提供的一个工具型 ChannelHandler，用于定时心跳请求的功能或是自动关闭长时间空闲连接的功能。它的原理到底是怎样的呢？在 IdleStateHandler 中通过 lastReadTime、lastWriteTime 等几个字段，记录了最近一次读/写事件的时间，IdleStateHandler 初始化的时候，会创建一个定时任务，定时检测当前时间与最后一次读/写时间的差值。如果超过我们设置的阈值（也就是上面 NettyServer 中设置的 idleTimeout），就会触发 IdleStateEvent 事件，并传递给后续的 ChannelHandler 进行处理。后续 ChannelHandler 的 userEventTriggered() 方法会根据接收到的 IdleStateEvent 事件，决定是关闭长时间空闲的连接，还是发送心跳探活。

最后来看**NettyServerHandler**，它继承了 ChannelDuplexHandler，这是 Netty 提供的一个同时处理 Inbound 数据和 Outbound 数据的 ChannelHandler，从下面的继承图就能看出来。

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/58/F3/Ciqc1F9wcFKAQQZ3AAB282frbWw282.png)

NettyServerHandler 继承关系图

在 NettyServerHandler 中有 channels 和 handler 两个核心字段。

-   channels（Map<String,Channel>集合）：记录了当前 Server 创建的所有 Channel，从下图中可以看到，连接创建（触发 channelActive() 方法）、连接断开（触发 channelInactive()方法）会操作 channels 集合进行相应的增删。
    

![Drawing 6.png](https://s0.lgstatic.com/i/image/M00/58/F3/Ciqc1F9wcFuABJWsAAaIoTwCIA0958.png)

-   handler（ChannelHandler 类型）：NettyServerHandler 内几乎所有方法都会触发该 Dubbo ChannelHandler 对象（如下图）。
    

![Drawing 7.png](https://s0.lgstatic.com/i/image/M00/58/FE/CgqCHl9wcGOAE_ykAAFvy5a4X58367.png)

这里以 write() 方法为例进行简单分析：

```
public void write(ChannelHandlerContext ctx, Object msg, ChannelPromise promise) throws Exception {
super.write(ctx, msg, promise); 
    NettyChannel channel = NettyChannel.getOrAddChannel(ctx.channel(), url, handler);
    handler.sent(channel, msg);
}
```

在 NettyServer 创建 NettyServerHandler 的时候，可以看到下面的这行代码：

```
final NettyServerHandler nettyServerHandler = new NettyServerHandler(getUrl(), this);
```

其中第二个参数传入的是 NettyServer 这个对象，你可以追溯一下 NettyServer 的继承结构，会发现它的最顶层父类 AbstractPeer 实现了 ChannelHandler，并且将所有的方法委托给其中封装的 ChannelHandler 对象，如下图所示：

![Drawing 8.png](https://s0.lgstatic.com/i/image/M00/58/F3/Ciqc1F9wcGuADQi3AAD6EEURlNU871.png)

也就是说，NettyServerHandler 会将数据委托给这个 ChannelHandler。

到此为止，Server 这条继承线就介绍完了。你可以回顾一下，从 AbstractPeer 开始往下，一路继承下来，NettyServer 拥有了 Endpoint、ChannelHandler 以及RemotingServer多个接口的能力，关联了一个 ChannelHandler 对象以及 Codec2 对象，并最终将数据委托给这两个对象进行处理。所以，上层调用方只需要实现 ChannelHandler 和 Codec2 这两个接口就可以了。

![6.png](https://s0.lgstatic.com/i/image/M00/59/E4/Ciqc1F9y4MyAR8XLAABTLdOZqrc228.png)

### 总结

本课时重点介绍了 Dubbo Transporter 层中 Server 相关的实现。

首先，我们介绍了 AbstractPeer 这个最顶层的抽象类，了解了 Server、Client 和 Channel 的公共属性。接下来，介绍了 AbstractEndpoint 抽象类，它提供了编解码等 Server 和 Client 所需的公共能力。最后，我们深入分析了 AbstractServer 抽象类以及基于 Netty 4 实现的 NettyServer，同时，还深入剖析了涉及的各种组件，例如，ExecutorRepository、NettyServerHandler 等。
