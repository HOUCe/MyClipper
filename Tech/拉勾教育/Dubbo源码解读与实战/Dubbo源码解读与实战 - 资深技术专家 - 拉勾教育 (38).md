---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256
author: 
---

# [Dubbo源码解读与实战 - 资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256)


你好，我是杨四正，今天我和你分享的主题是集群容错：一个好汉三个帮（下篇）。

在上一课时，我们介绍了 Dubbo Cluster 层中集群容错机制的基础知识，还说明了 Cluster 接口的定义以及其各个实现类的核心功能。同时，我们还分析了 AbstractClusterInvoker 抽象类以及 AbstractCluster 抽象实现类的核心实现。

那接下来在本课时，我们将介绍 Cluster 接口的全部实现类，以及相关的 Cluster Invoker 实现类。

### FailoverClusterInvoker

通过前面对 Cluster 接口的介绍我们知道，Cluster 默认的扩展实现是 FailoverCluster，其 doJoin() 方法中会创建一个 FailoverClusterInvoker 对象并返回，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new FailoverClusterInvoker<>(directory);
}
```

**FailoverClusterInvoker 会在调用失败的时候，自动切换 Invoker 进行重试**。下面来看 FailoverClusterInvoker 的核心实现：

```
public Result doInvoke(Invocation invocation, final List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
    List<Invoker<T>> copyInvokers = invokers;
    checkInvokers(copyInvokers, invocation);
    String methodName = RpcUtils.getMethodName(invocation);
int len = getUrl().getMethodParameter(methodName, RETRIES_KEY, DEFAULT_RETRIES) + 1;
if (len <= 0) {
        len = 1;
    }
    RpcException le = null;
    List<Invoker<T>> invoked = new ArrayList<Invoker<T>>(copyInvokers.size());
    Set<String> providers = new HashSet<String>(len);
for (int i = 0; i < len; i++) {
if (i > 0) {
            checkWhetherDestroyed();
            copyInvokers = list(invocation);
            checkInvokers(copyInvokers, invocation);
        }
        Invoker<T> invoker = select(loadbalance, invocation, copyInvokers, invoked);
        invoked.add(invoker);
        RpcContext.getContext().setInvokers((List) invoked);
try {
            Result result = invoker.invoke(invocation);
if (le != null && logger.isWarnEnabled()) {
                logger.warn("...");
            }
return result;
        } catch (RpcException e) {
if (e.isBiz()) { 
throw e;
            }
            le = e;
        } catch (Throwable e) { 
            le = new RpcException(e.getMessage(), e);
        } finally {
            providers.add(invoker.getUrl().getAddress());
        }
    }
throw new RpcException(le.getCode(), "...");
}
```

### FailbackClusterInvoker

FailbackCluster 是 Cluster 接口的另一个扩展实现，扩展名是 failback，其 doJoin() 方法中创建的 Invoker 对象是 FailbackClusterInvoker 类型，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new FailbackClusterInvoker<>(directory);
}
```

**FailbackClusterInvoker 在请求失败之后，返回一个空结果给 Consumer，同时还会添加一个定时任务对失败的请求进行重试**。下面来看 FailbackClusterInvoker 的具体实现：

```
protected Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
    Invoker<T> invoker = null;
try {
        checkInvokers(invokers, invocation);
        invoker = select(loadbalance, invocation, invokers, null);
return invoker.invoke(invocation);
    } catch (Throwable e) {
        addFailed(loadbalance, invocation, invokers, invoker);
return AsyncRpcResult.newDefaultAsyncResult(null, null, invocation); 
    }
}
```

在 doInvoke() 方法中，请求失败时会调用 addFailed() 方法添加定时任务进行重试，默认每隔 5 秒执行一次，总共重试 3 次，具体实现如下：

```
private void addFailed(LoadBalance loadbalance, Invocation invocation, List<Invoker<T>> invokers, Invoker<T> lastInvoker) {
if (failTimer == null) {
synchronized (this) {
if (failTimer == null) { 
                failTimer = new HashedWheelTimer(
new NamedThreadFactory("failback-cluster-timer", true),
1,
                        TimeUnit.SECONDS, 32, failbackTasks);
            }
        }
    }
    RetryTimerTask retryTimerTask = new RetryTimerTask(loadbalance, invocation, invokers, lastInvoker, retries, RETRY_FAILED_PERIOD);
try {
        failTimer.newTimeout(retryTimerTask, RETRY_FAILED_PERIOD, TimeUnit.SECONDS);
    } catch (Throwable e) {
        logger.error("...");
    }
}
```

在 RetryTimerTask 定时任务中，会重新调用 select() 方法筛选合适的 Invoker 对象，并尝试进行请求。如果请求再次失败且重试次数未达到上限，则调用 rePut() 方法再次添加定时任务，等待进行重试；如果请求成功，也不会返回任何结果。RetryTimerTask 的核心实现如下：

```
public void run(Timeout timeout) {
try {
        Invoker<T> retryInvoker = select(loadbalance, invocation, invokers, Collections.singletonList(lastInvoker));
        lastInvoker = retryInvoker;
        retryInvoker.invoke(invocation); 
    } catch (Throwable e) {
if ((++retryTimes) >= retries) { 
            logger.error("...");
        } else {
            rePut(timeout); 
        }
    }
}
private void rePut(Timeout timeout) {
if (timeout == null) { 
return;
    }
    Timer timer = timeout.timer();
if (timer.isStop() || timeout.isCancelled()) { 
return;
    }
    timer.newTimeout(timeout.task(), tick, TimeUnit.SECONDS);
}
```

### FailfastClusterInvoker

FailfastCluster 的扩展名是 failfast，在其 doJoin() 方法中会创建 FailfastClusterInvoker 对象，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new FailfastClusterInvoker<>(directory);
}
```

**FailfastClusterInvoker 只会进行一次请求，请求失败之后会立即抛出异常，这种策略适合非幂等的操作**，具体实现如下：

```
public Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
    checkInvokers(invokers, invocation);
    Invoker<T> invoker = select(loadbalance, invocation, invokers, null);
try {
return invoker.invoke(invocation); 
    } catch (Throwable e) { 
if (e instanceof RpcException && ((RpcException) e).isBiz()) {
throw (RpcException) e;
        }
throw new RpcException("...");
    }
}
```

### FailsafeClusterInvoker

FailsafeCluster 的扩展名是 failsafe，在其 doJoin() 方法中会创建 FailsafeClusterInvoker 对象，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new FailsafeClusterInvoker<>(directory);
}
```

**FailsafeClusterInvoker 只会进行一次请求，请求失败之后会返回一个空结果**，具体实现如下：

```
public Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
try {
        checkInvokers(invokers, invocation);
        Invoker<T> invoker = select(loadbalance, invocation, invokers, null);
return invoker.invoke(invocation);
    } catch (Throwable e) {
        logger.error("...");
return AsyncRpcResult.newDefaultAsyncResult(null, null, invocation); 
    }
}
```

### ForkingClusterInvoker

ForkingCluster 的扩展名称为 forking，在其 doJoin() 方法中，会创建一个 ForkingClusterInvoker 对象，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new ForkingClusterInvoker<>(directory);
}
```

ForkingClusterInvoker 中会**维护一个线程池**（executor 字段，通过 Executors.newCachedThreadPool() 方法创建的线程池），**并发调用多个 Provider 节点，只要有一个 Provider 节点成功返回了结果，ForkingClusterInvoker 的 doInvoke() 方法就会立即结束运行**。

ForkingClusterInvoker 主要是为了应对一些实时性要求较高的读操作，因为没有并发控制的多线程写入，可能会导致数据不一致。

ForkingClusterInvoker.doInvoke() 方法首先从 Invoker 集合中选出指定个数（forks 参数决定）的 Invoker 对象，然后通过 executor 线程池并发调用这些 Invoker，并将请求结果存储在 ref 阻塞队列中，则当前线程会阻塞在 ref 队列上，等待第一个请求结果返回。下面是 ForkingClusterInvoker 的具体实现：

```
public Result doInvoke(final Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
try {
        checkInvokers(invokers, invocation);
final List<Invoker<T>> selected;
final int forks = getUrl().getParameter(FORKS_KEY, DEFAULT_FORKS);
final int timeout = getUrl().getParameter(TIMEOUT_KEY, DEFAULT_TIMEOUT);
if (forks <= 0 || forks >= invokers.size()) {
            selected = invokers;
        } else {
            selected = new ArrayList<>(forks);
while (selected.size() < forks) {
                Invoker<T> invoker = select(loadbalance, invocation, invokers, selected);
if (!selected.contains(invoker)) {
                    selected.add(invoker); 
                }
            }
        }
        RpcContext.getContext().setInvokers((List) selected);
final AtomicInteger count = new AtomicInteger();
final BlockingQueue<Object> ref = new LinkedBlockingQueue<>();
for (final Invoker<T> invoker : selected) {  
            executor.execute(() -> { 
try {
                    Result result = invoker.invoke(invocation);
                    ref.offer(result);
                } catch (Throwable e) {
int value = count.incrementAndGet();
if (value >= selected.size()) {
                        ref.offer(e); 
                    }
                }
            });
        }
try {
            Object ret = ref.poll(timeout, TimeUnit.MILLISECONDS);
if (ret instanceof Throwable) { 
                Throwable e = (Throwable) ret;
throw new RpcException("...");
            }
return (Result) ret; 
        } catch (InterruptedException e) {
throw new RpcException("...");
        }
    } finally { 
        RpcContext.getContext().clearAttachments();
    }
}
```

### BroadcastClusterInvoker

BroadcastCluster 这个 Cluster 实现类的扩展名为 broadcast，在其 doJoin() 方法中创建的是 BroadcastClusterInvoker 类型的 Invoker 对象，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new BroadcastClusterInvoker<>(directory);
}
```

**在 BroadcastClusterInvoker 中，会逐个调用每个 Provider 节点，其中任意一个 Provider 节点报错，都会在全部调用结束之后抛出异常**。BroadcastClusterInvoker**通常用于通知类的操作**，例如通知所有 Provider 节点更新本地缓存。

下面来看 BroadcastClusterInvoker 的具体实现：

```
public Result doInvoke(final Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
    checkInvokers(invokers, invocation);
    RpcContext.getContext().setInvokers((List) invokers);
    RpcException exception = null; 
    Result result = null;
for (Invoker<T> invoker : invokers) {
try {
            result = invoker.invoke(invocation);
        } catch (RpcException e) {
            exception = e;
            logger.warn(e.getMessage(), e);
        } catch (Throwable e) {
            exception = new RpcException(e.getMessage(), e);
            logger.warn(e.getMessage(), e);
        }
    }
if (exception != null) { 
throw exception;
    }
return result;
}
```

### AvailableClusterInvoker

AvailableCluster 这个 Cluster 实现类的扩展名为 available，在其 join() 方法中创建的是 AvailableClusterInvoker 类型的 Invoker 对象，具体实现如下：

```
public <T> Invoker<T> join(Directory<T> directory) throws RpcException {
return new AvailableClusterInvoker<>(directory);
}
```

在 AvailableClusterInvoker 的 doInvoke() 方法中，会遍历整个 Invoker 集合，**逐个调用对应的 Provider 节点，当遇到第一个可用的 Provider 节点时，就尝试访问该 Provider 节点，成功则返回结果；如果访问失败，则抛出异常终止遍历**。

下面是 AvailableClusterInvoker 的具体实现：

```
public Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
for (Invoker<T> invoker : invokers) { 
if (invoker.isAvailable()) { 
return invoker.invoke(invocation);
        }
    }
throw new RpcException("No provider available in " + invokers);
}
```

### MergeableClusterInvoker

MergeableCluster 这个 Cluster 实现类的扩展名为 mergeable，在其 doJoin() 方法中创建的是 MergeableClusterInvoker 类型的 Invoker 对象，具体实现如下：

```
public <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new MergeableClusterInvoker<T>(directory);
}
```

**MergeableClusterInvoker 会对多个 Provider 节点返回结果合并**。如果请求的方法没有配置 Merger 合并器（即没有指定 merger 参数），则不会进行结果合并，而是直接将第一个可用的 Invoker 结果返回。下面来看 MergeableClusterInvoker 的具体实现：

```
protected Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
    checkInvokers(invokers, invocation);
    String merger = getUrl().getMethodParameter(invocation.getMethodName(), MERGER_KEY);
if (ConfigUtils.isEmpty(merger)) {
for (final Invoker<T> invoker : invokers) {
if (invoker.isAvailable()) {
try {
return invoker.invoke(invocation);
                } catch (RpcException e) {
if (e.isNoInvokerAvailableAfterFilter()) {
                        log.debug("No available provider for service" + getUrl().getServiceKey() + " on group " + invoker.getUrl().getParameter(GROUP_KEY) + ", will continue to try another group.");
                    } else {
throw e;
                    }
                }
            }
        }
return invokers.iterator().next().invoke(invocation);
    }
    Class<?> returnType;
try {
        returnType = getInterface().getMethod(
                invocation.getMethodName(), invocation.getParameterTypes()).getReturnType();
    } catch (NoSuchMethodException e) {
        returnType = null;
    }
    Map<String, Result> results = new HashMap<>();
for (final Invoker<T> invoker : invokers) {
        RpcInvocation subInvocation = new RpcInvocation(invocation, invoker);
        subInvocation.setAttachment(ASYNC_KEY, "true");
        results.put(invoker.getUrl().getServiceKey(), invoker.invoke(subInvocation));
    }
    Object result = null;
    List<Result> resultList = new ArrayList<Result>(results.size());
for (Map.Entry<String, Result> entry : results.entrySet()) {
        Result asyncResult = entry.getValue();
try {
            Result r = asyncResult.get();
if (r.hasException()) {
                log.error("Invoke " + getGroupDescFromServiceKey(entry.getKey()) +
" failed: " + r.getException().getMessage(),
                        r.getException());
            } else {
                resultList.add(r);
            }
        } catch (Exception e) {
throw new RpcException("Failed to invoke service " + entry.getKey() + ": " + e.getMessage(), e);
        }
    }
if (resultList.isEmpty()) {
return AsyncRpcResult.newDefaultAsyncResult(invocation);
    } else if (resultList.size() == 1) {
return resultList.iterator().next();
    }
if (returnType == void.class) {
return AsyncRpcResult.newDefaultAsyncResult(invocation);
    }
if (merger.startsWith(".")) {
        merger = merger.substring(1);
        Method method;
try {
            method = returnType.getMethod(merger, returnType);
        } catch (NoSuchMethodException e) {
throw new RpcException("Can not merge result because missing method [ " + merger + " ] in class [ " +
                    returnType.getName() + " ]");
        }
if (!Modifier.isPublic(method.getModifiers())) {
            method.setAccessible(true);
        }
        result = resultList.remove(0).getValue();
try {
if (method.getReturnType() != void.class
                    && method.getReturnType().isAssignableFrom(result.getClass())) {
for (Result r : resultList) { // 反射调用
                    result = method.invoke(result, r.getValue());
                }
            } else {
for (Result r : resultList) { 
                    method.invoke(result, r.getValue());
                }
            }
        } catch (Exception e) {
throw new RpcException("Can not merge result: " + e.getMessage(), e);
        }
    } else {
        Merger resultMerger;
if (ConfigUtils.isDefault(merger)) {
            resultMerger = MergerFactory.getMerger(returnType);
        } else {
            resultMerger = ExtensionLoader.getExtensionLoader(Merger.class).getExtension(merger);
        }
if (resultMerger != null) {
            List<Object> rets = new ArrayList<Object>(resultList.size());
for (Result r : resultList) {
                rets.add(r.getValue());
            }
            result = resultMerger.merge(
                    rets.toArray((Object[]) Array.newInstance(returnType, 0)));
        } else {
throw new RpcException("There is no merger to merge result.");
        }
    }
return AsyncRpcResult.newDefaultAsyncResult(result, invocation);
}
```

### ZoneAwareClusterInvoker

ZoneAwareCluster 这个 Cluster 实现类的扩展名为 zone-aware，在其 doJoin() 方法中创建的是 ZoneAwareClusterInvoker 类型的 Invoker 对象，具体实现如下：

```
protected <T> AbstractClusterInvoker<T> doJoin(Directory<T> directory) throws RpcException {
return new ZoneAwareClusterInvoker<T>(directory);
}
```

在 Dubbo 中使用多个注册中心的架构如下图所示：

![Lark20201203-183149.png](https://s0.lgstatic.com/i/image/M00/76/E6/Ciqc1F_IvtuAXngKAADJgn-frEE576.png)

双注册中心结构图

Consumer 可以使用 ZoneAwareClusterInvoker 先在多个注册中心之间进行选择，选定注册中心之后，再选择 Provider 节点，如下图所示：

![Lark20201203-183145.png](https://s0.lgstatic.com/i/image/M00/76/E6/Ciqc1F_IvuOAFBfoAAD_GvyhrZY880.png)

ZoneAwareClusterInvoker 在多注册中心之间进行选择的策略有以下四种。

1.  找到**preferred 属性为 true 的注册中心，它是优先级最高的注册中心**，只有该中心无可用 Provider 节点时，才会回落到其他注册中心。
    
2.  根据**请求中的 zone key 做匹配**，优先派发到相同 zone 的注册中心。
    
3.  根据**权重**（也就是注册中心配置的 weight 属性）进行轮询。
    
4.  如果上面的策略都未命中，则选择**第一个可用的 Provider 节点**。
    

下面来看 ZoneAwareClusterInvoker 的具体实现：

```
public Result doInvoke(Invocation invocation, final List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
for (Invoker<T> invoker : invokers) {
        MockClusterInvoker<T> mockClusterInvoker = (MockClusterInvoker<T>) invoker;
if (mockClusterInvoker.isAvailable() && mockClusterInvoker.getRegistryUrl()
                .getParameter(REGISTRY_KEY + "." + PREFERRED_KEY, false)) {
return mockClusterInvoker.invoke(invocation);
        }
    }
    String zone = (String) invocation.getAttachment(REGISTRY_ZONE);
if (StringUtils.isNotEmpty(zone)) {
for (Invoker<T> invoker : invokers) {
            MockClusterInvoker<T> mockClusterInvoker = (MockClusterInvoker<T>) invoker;
if (mockClusterInvoker.isAvailable() && zone.equals(mockClusterInvoker.getRegistryUrl().getParameter(REGISTRY_KEY + "." + ZONE_KEY))) {
return mockClusterInvoker.invoke(invocation);
            }
        }
        String force = (String) invocation.getAttachment(REGISTRY_ZONE_FORCE);
if (StringUtils.isNotEmpty(force) && "true".equalsIgnoreCase(force)) {
throw new IllegalStateException("...");
        }
    }
    Invoker<T> balancedInvoker = select(loadbalance, invocation, invokers, null);
if (balancedInvoker.isAvailable()) {
return balancedInvoker.invoke(invocation);
    }
for (Invoker<T> invoker : invokers) {
        MockClusterInvoker<T> mockClusterInvoker = (MockClusterInvoker<T>) invoker;
if (mockClusterInvoker.isAvailable()) {
return mockClusterInvoker.invoke(invocation);
        }
    }
throw new RpcException("No provider available in " + invokers);
}
```

### 总结

本课时我们重点介绍了 Dubbo 中 Cluster 接口的各个实现类的原理以及相关 Invoker 的实现原理。这里重点分析的 Cluster 实现有：Failover Cluster、Failback Cluster、Failfast Cluster、Failsafe Cluster、Forking Cluster、Broadcast Cluster、Available Cluster 和 Mergeable Cluster。除此之外，我们还分析了多注册中心的 ZoneAware Cluster 实现。

在后面的课时中，我们将针对 Mergeable Cluster 实现中使用到的 Merger 策略，以及 ZoneAware Cluster 实现涉及的 Mock 机制进行分析。记得按时来听课。
