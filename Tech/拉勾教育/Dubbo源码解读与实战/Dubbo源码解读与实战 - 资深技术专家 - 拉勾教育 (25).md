---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256
author: 
---

# [Dubbo源码解读与实战 - 资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256)


在上一课时，我们以 DubboProtocol 实现为基础，详细介绍了 Dubbo 服务发布的核心流程。在本课时，我们继续介绍 DubboProtocol 中**服务引用**相关的实现。

### refer 流程

下面我们开始介绍 DubboProtocol 中引用服务的相关实现，其核心实现在 protocolBindingRefer() 方法中：

```
public <T> Invoker<T> protocolBindingRefer(Class<T> serviceType, URL url) throws RpcException {
    optimizeSerialization(url); 
    DubboInvoker<T> invoker = new DubboInvoker<T>(serviceType, url, getClients(url), invokers);
    invokers.add(invoker); 
return invoker;
}
```

关于 DubboInvoker 的具体实现，我们先暂时不做深入分析。这里我们需要先关注的是**getClients() 方法**，它创建了底层发送请求和接收响应的 Client 集合，其核心分为了两个部分，一个是针对**共享连接**的处理，另一个是针对**独享连接**的处理，具体实现如下：

```
private ExchangeClient[] getClients(URL url) {
boolean useShareConnect = false;
int connections = url.getParameter(CONNECTIONS_KEY, 0);
    List<ReferenceCountExchangeClient> shareClients = null;
if (connections == 0) { 
        useShareConnect = true;
        String shareConnectionsStr = url.getParameter(SHARE_CONNECTIONS_KEY, (String) null);
        connections = Integer.parseInt(StringUtils.isBlank(shareConnectionsStr) ? ConfigUtils.getProperty(SHARE_CONNECTIONS_KEY,
                DEFAULT_SHARE_CONNECTIONS) : shareConnectionsStr);
        shareClients = getSharedClient(url, connections);
    }
    ExchangeClient[] clients = new ExchangeClient[connections];
for (int i = 0; i < clients.length; i++) {
if (useShareConnect) {
            clients[i] = shareClients.get(i);
        } else {
            clients[i] = initClient(url);
        }
    }
return clients;
}
```

当使用独享连接的时候，对每个 Service 建立固定数量的 Client，每个 Client 维护一个底层连接。如下图所示，就是针对每个 Service 都启动了两个独享连接：

![Lark20201020-171207.png](https://s0.lgstatic.com/i/image/M00/61/11/CgqCHl-OqnqAD_WFAAGYtk5Nou4688.png)

Service 独享连接示意图

当使用共享连接的时候，会区分不同的网络地址（host:port），一个地址只建立固定数量的共享连接。如下图所示，Provider 1 暴露了多个服务，Consumer 引用了 Provider 1 中的多个服务，共享连接是说 Consumer 调用 Provider 1 中的多个服务时，是通过固定数量的共享 TCP 长连接进行数据传输，这样就可以达到减少服务端连接数的目的。

![Lark20201020-171159.png](https://s0.lgstatic.com/i/image/M00/61/06/Ciqc1F-OqoOAHURKAAF2m0HX5qU972.png)

Service 共享连接示意图

那怎么去创建共享连接呢？**创建共享连接的实现细节是在 getSharedClient() 方法中**，它首先从 referenceClientMap 缓存（Map<String, List`<ReferenceCountExchangeClient>`\> 类型）中查询 Key（host 和 port 拼接成的字符串）对应的共享 Client 集合，如果查找到的 Client 集合全部可用，则直接使用这些缓存的 Client，否则要创建新的 Client 来补充替换缓存中不可用的 Client。示例代码如下：

```
private List<ReferenceCountExchangeClient> getSharedClient(URL url, int connectNum) {
    String key = url.getAddress(); 
    List<ReferenceCountExchangeClient> clients = referenceClientMap.get(key);
if (checkClientCanUse(clients)) { 
        batchClientRefIncr(clients); 
return clients;
    }
    locks.putIfAbsent(key, new Object());
synchronized (locks.get(key)) { 
        clients = referenceClientMap.get(key);
if (checkClientCanUse(clients)) { 
            batchClientRefIncr(clients); 
return clients;
        }
        connectNum = Math.max(connectNum, 1); 
if (CollectionUtils.isEmpty(clients)) {
            clients = buildReferenceCountExchangeClientList(url, connectNum);
            referenceClientMap.put(key, clients);
        } else { 
for (int i = 0; i < clients.size(); i++) {
                ReferenceCountExchangeClient referenceCountExchangeClient = clients.get(i);
if (referenceCountExchangeClient == null || referenceCountExchangeClient.isClosed()) {
                    clients.set(i, buildReferenceCountExchangeClient(url));
continue;
                }
                referenceCountExchangeClient.incrementAndGetCount();
            }
        }
        locks.remove(key); 
return clients;
    }
}
```

这里使用的 ExchangeClient 实现是 ReferenceCountExchangeClient，它是 ExchangeClient 的一个装饰器，在原始 ExchangeClient 对象基础上添加了引用计数的功能。

ReferenceCountExchangeClient 中除了持有被修饰的 ExchangeClient 对象外，还有一个 referenceCount 字段（AtomicInteger 类型），用于记录该 Client 被应用的次数。从下图中我们可以看到，在 ReferenceCountExchangeClient 的构造方法以及 incrementAndGetCount() 方法中会增加引用次数，在 close() 方法中则会减少引用次数。

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/61/06/Ciqc1F-OqqeAHAStAAF3BXy1LnA608.png)

referenceCount 修改调用栈

这样，对于同一个地址的共享连接，就可以满足两个基本需求：

1.  当引用次数减到 0 的时候，ExchangeClient 连接关闭；
    
2.  当引用次数未减到 0 的时候，底层的 ExchangeClient 不能关闭。
    

还有一个需要注意的细节是 ReferenceCountExchangeClient.close() 方法，在关闭底层 ExchangeClient 对象之后，会立即创建一个 LazyConnectExchangeClient ，也有人称其为“幽灵连接”。具体逻辑如下所示，这里的 LazyConnectExchangeClient 主要用于异常情况的兜底：

```
public void close(int timeout) {
if (referenceCount.decrementAndGet() <= 0) { 
if (timeout == 0) {
            client.close();
        } else {
            client.close(timeout);
        } 
        replaceWithLazyClient(); 
    }
}
private void replaceWithLazyClient() {
    URL lazyUrl = URLBuilder.from(url)
            .addParameter(LAZY_CONNECT_INITIAL_STATE_KEY, Boolean.TRUE)
            .addParameter(RECONNECT_KEY, Boolean.FALSE)
            .addParameter(SEND_RECONNECT_KEY, Boolean.TRUE.toString())
            .addParameter("warning", Boolean.TRUE.toString())
            .addParameter(LazyConnectExchangeClient.REQUEST_WITH_WARNING_KEY, true)
            .addParameter("_client_memo", "referencecounthandler.replacewithlazyclient")
            .build();
if (!(client instanceof LazyConnectExchangeClient) || client.isClosed()) {
        client = new LazyConnectExchangeClient(lazyUrl, client.getExchangeHandler());
    }
}
```

LazyConnectExchangeClient 也是 ExchangeClient 的装饰器，它会在原有 ExchangeClient 对象的基础上添加懒加载的功能。LazyConnectExchangeClient 在构造方法中不会创建底层持有连接的 Client，而是在需要发送请求的时候，才会调用 initClient() 方法进行 Client 的创建，如下图调用关系所示：

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/61/11/CgqCHl-OqrqAHcvUAAC9KpqKEBQ887.png)

initClient() 方法的调用位置

initClient() 方法的具体实现如下：

```
private void initClient() throws RemotingException {
if (client != null) { 
return;
    }
    connectLock.lock();
try {
if (client != null) { return; } 
this.client = Exchangers.connect(url, requestHandler);
    } finally {
        connectLock.unlock();
    }
}
```

在这些发送请求的方法中，除了通过 initClient() 方法初始化底层 ExchangeClient 外，还会调用warning() 方法，其会根据当前 URL 携带的参数决定是否打印 WARN 级别日志。为了防止瞬间打印大量日志的情况发生，这里有打印的频率限制，默认每发送 5000 次请求打印 1 条日志。你可以看到在前面展示的兜底场景中，我们就开启了打印日志的选项。

**分析完 getSharedClient() 方法创建共享 Client 的核心流程之后，我们回到 DubboProtocol 中，继续介绍创建独享 Client 的流程。**

创建独享 Client 的入口在**DubboProtocol.initClient() 方法**，它首先会在 URL 中设置一些默认的参数，然后根据 LAZY\_CONNECT\_KEY 参数决定是否使用 LazyConnectExchangeClient 进行封装，实现懒加载功能，如下代码所示：

```
private ExchangeClient initClient(URL url) {
    String str = url.getParameter(CLIENT_KEY, url.getParameter(SERVER_KEY, DEFAULT_REMOTING_CLIENT));
    url = url.addParameter(CODEC_KEY, DubboCodec.NAME);
    url = url.addParameterIfAbsent(HEARTBEAT_KEY, String.valueOf(DEFAULT_HEARTBEAT));
    ExchangeClient client;    
if (url.getParameter(LAZY_CONNECT_KEY, false)) {
        client = new LazyConnectExchangeClient(url, requestHandler);
    } else { 
        client = Exchangers.connect(url, requestHandler);
    }
return client;
}
```

这里涉及的 LazyConnectExchangeClient 装饰器以及 Exchangers 门面类在前面已经深入分析过了，就不再赘述了。

DubboProtocol 中还剩下几个方法没有介绍，这里你只需要简单了解一下它们的实现即可。

-   batchClientRefIncr() 方法：会遍历传入的集合，将其中的每个 ReferenceCountExchangeClient 对象的引用加一。
    
-   buildReferenceCountExchangeClient() 方法：会调用上面介绍的 initClient() 创建 Client 对象，然后再包装一层 ReferenceCountExchangeClient 进行修饰，最后返回。该方法主要用于创建共享 Client。
    

### destroy方法

在 DubboProtocol 销毁的时候，会调用 destroy() 方法释放底层资源，其中就涉及 export 流程中创建的 ProtocolServer 对象以及 refer 流程中创建的 Client。

DubboProtocol.destroy() 方法首先会逐个关闭 serverMap 集合中的 ProtocolServer 对象，相关代码片段如下：

```
for (String key : new ArrayList<>(serverMap.keySet())) {
    ProtocolServer protocolServer = serverMap.remove(key);
if (protocolServer == null) { continue;}
    RemotingServer server = protocolServer.getRemotingServer();
    server.close(ConfigurationUtils.getServerShutdownTimeout());
}
```

ConfigurationUtils.getServerShutdownTimeout() 方法返回的阻塞时长默认是 10 秒，我们可以通过 dubbo.service.shutdown.wait 或是 dubbo.service.shutdown.wait.seconds 进行配置。

之后，DubboProtocol.destroy() 方法会逐个关闭 referenceClientMap 集合中的 Client，逻辑与上述关闭ProtocolServer的逻辑相同，这里不再重复。只不过需要注意前面我们提到的 ReferenceCountExchangeClient 的存在，只有引用减到 0，底层的 Client 才会真正销毁。

最后，DubboProtocol.destroy() 方法会调用父类 AbstractProtocol 的 destroy() 方法，销毁全部 Invoker 对象，前面已经介绍过 AbstractProtocol.destroy() 方法的实现，这里也不再重复。

### 总结

本课时我们继续上一课时的话题，以 DubboProtocol 为例，介绍了 Dubbo 在 Protocol 层实现服务引用的核心流程。我们首先介绍了 DubboProtocol 初始化 Client 的核心逻辑，分析了共享连接和独立连接的模型，后续还讲解了ReferenceCountExchangeClient、LazyConnectExchangeClient 等装饰器的功能和实现，最后说明了 destroy() 方法释放底层资源的相关实现。

关于 DubboProtocol，你若还有什么疑问或想法，欢迎你留言跟我分享。下一课时，我们将开始深入介绍 Dubbo 的“心脏”—— Invoker 接口的相关实现，这是我们的一篇加餐文章，记得按时来听课。
