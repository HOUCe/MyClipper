---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256
author: 
---

# [Dubbo源码解读与实战 - 资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256)


在上一课时我们了解了 LoadBalance 接口定义以及 AbstractLoadBalance 抽象类的内容，还详细介绍了 ConsistentHashLoadBalance 以及 RandomLoadBalance 这两个实现类的核心原理和大致实现。本课时我们将继续介绍 LoadBalance 的剩余三个实现。

### LeastActiveLoadBalance

LeastActiveLoadBalance 使用的是**最小活跃数负载均衡算法**。它认为当前活跃请求数越小的 Provider 节点，剩余的处理能力越多，处理请求的效率也就越高，那么该 Provider 在单位时间内就可以处理更多的请求，所以我们应该优先将请求分配给该 Provider 节点。

LeastActiveLoadBalance 需要配合 ActiveLimitFilter 使用，ActiveLimitFilter 会记录每个接口方法的活跃请求数，在 LeastActiveLoadBalance 进行负载均衡时，只会从活跃请求数最少的 Invoker 集合里挑选 Invoker。

在 LeastActiveLoadBalance 的实现中，首先会选出所有活跃请求数最小的 Invoker 对象，之后的逻辑与 RandomLoadBalance 完全一样，即按照这些 Invoker 对象的权重挑选最终的 Invoker 对象。下面是 LeastActiveLoadBalance.doSelect() 方法的具体实现：

```
protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
int length = invokers.size();
int leastActive = -1;
int leastCount = 0;
int[] leastIndexes = new int[length];
int[] weights = new int[length];
int totalWeight = 0;
int firstWeight = 0;
boolean sameWeight = true;
for (int i = 0; i < length; i++) { 
        Invoker<T> invoker = invokers.get(i);
int active = RpcStatus.getStatus(invoker.getUrl(), invocation.getMethodName()).getActive();
int afterWarmup = getWeight(invoker, invocation);
        weights[i] = afterWarmup;
if (leastActive == -1 || active < leastActive) {
            leastActive = active; 
            leastCount = 1; 
            leastIndexes[0] = i; 
            totalWeight = afterWarmup; 
            firstWeight = afterWarmup; 
            sameWeight = true; 
        } else if (active == leastActive) { 
            leastIndexes[leastCount++] = i; 
            totalWeight += afterWarmup; 
if (sameWeight && afterWarmup != firstWeight) {
                sameWeight = false; 
            }
        }
    }
if (leastCount == 1) {
return invokers.get(leastIndexes[0]);
    }
if (!sameWeight && totalWeight > 0) {
int offsetWeight = ThreadLocalRandom.current().nextInt(totalWeight);
for (int i = 0; i < leastCount; i++) {
int leastIndex = leastIndexes[i];
            offsetWeight -= weights[leastIndex];
if (offsetWeight < 0) {
return invokers.get(leastIndex);
            }
        }
    }
return invokers.get(leastIndexes[ThreadLocalRandom.current().nextInt(leastCount)]);
}
```

ActiveLimitFilter 以及底层的 RpcStatus 记录活跃请求数的具体原理，在前面的[第 30 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=5194)中我们已经详细分析过了，这里不再重复，如果有不清楚的地方，你可以回顾之前课时相关的内容。

### RoundRobinLoadBalance

RoundRobinLoadBalance 实现的是**加权轮询负载均衡算法**。

轮询指的是将请求轮流分配给每个 Provider。例如，有 A、B、C 三个 Provider 节点，按照普通轮询的方式，我们会将第一个请求分配给 Provider A，将第二个请求分配给 Provider B，第三个请求分配给 Provider C，第四个请求再次分配给 Provider A……如此循环往复。

**轮询是一种无状态负载均衡算法，实现简单，适用于集群中所有 Provider 节点性能相近的场景。** 但现实情况中就很难保证这一点了，因为很容易出现集群中性能最好和最差的 Provider 节点处理同样流量的情况，这就可能导致性能差的 Provider 节点各方面资源非常紧张，甚至无法及时响应了，但是性能好的 Provider 节点的各方面资源使用还较为空闲。这时我们可以通过加权轮询的方式，降低分配到性能较差的 Provider 节点的流量。

加权之后，分配给每个 Provider 节点的流量比会接近或等于它们的权重比。例如，Provider 节点 A、B、C 权重比为 5:1:1，那么在 7 次请求中，节点 A 将收到 5 次请求，节点 B 会收到 1 次请求，节点 C 则会收到 1 次请求。

**在 Dubbo 2.6.4 版本及之前，RoundRobinLoadBalance 的实现存在一些问题，例如，选择 Invoker 的性能问题、负载均衡时不够平滑等。在 Dubbo 2.6.5 版本之后，这些问题都得到了修复**，所以这里我们就来介绍最新的 RoundRobinLoadBalance 实现。

每个 Provider 节点有两个权重：一个权重是配置的 weight，该值在负载均衡的过程中不会变化；另一个权重是 currentWeight，该值会在负载均衡的过程中动态调整，初始值为 0。

当有新的请求进来时，RoundRobinLoadBalance 会遍历 Invoker 列表，并用对应的 currentWeight 加上其配置的权重。遍历完成后，再找到最大的 currentWeight，将其减去权重总和，然后返回相应的 Invoker 对象。

下面我们通过一个示例说明 RoundRobinLoadBalance 的执行流程，这里我们依旧假设 A、B、C 三个节点的权重比例为 5:1:1。

![Lark20201127-153527.png](https://s0.lgstatic.com/i/image/M00/72/19/CgqCHl_ArGSAfxA6AAHyWL4Af1o908.png)

1.  处理第一个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[0, 0, 0\] 变为 \[5, 1, 1\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 A。最后，将节点 A 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[-2, 1, 1\]。
    
2.  处理第二个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[-2, 1, 1\] 变为 \[3, 2, 2\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 A。最后，将节点 A 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[-4, 2, 2\]。
    
3.  处理第三个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[-4, 2, 2\] 变为 \[1, 3, 3\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 B。最后，将节点 B 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[1, -4, 3\]。
    
4.  处理第四个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[1, -4, 3\] 变为 \[6, -3, 4\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 A。最后，将节点 A 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[-1, -3, 4\]。
    
5.  处理第五个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[-1, -3, 4\] 变为 \[4, -2, 5\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 C。最后，将节点 C 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[4, -2, -2\]。
    
6.  处理第六个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[4, -2, -2\] 变为 \[9, -1, -1\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 A。最后，将节点 A 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[2, -1, -1\]。
    
7.  处理第七个请求，currentWeight 数组中的权重与配置的 weight 相加，即从 \[2, -1, -1\] 变为 \[7, 0, 0\]。接下来，从中选择权重最大的 Invoker 作为结果，即节点 A。最后，将节点 A 的 currentWeight 值减去 totalWeight 值，最终得到 currentWeight 数组为 \[0, 0, 0\]。
    

到此为止，一个轮询的周期就结束了。

而在 Dubbo 2.6.4 版本中，上面示例的一次轮询结果是 \[A, A, A, A, A, B, C\]，也就是说前 5 个请求会全部都落到 A 这个节点上。这将会使节点 A 在短时间内接收大量的请求，压力陡增，而节点 B 和节点 C 此时没有收到任何请求，处于完全空闲的状态，这种“瞬间分配不平衡”的情况也就是前面提到的“不平滑问题”。

在 RoundRobinLoadBalance 中，我们**为每个 Invoker 对象创建了一个对应的 WeightedRoundRobin 对象**，用来记录配置的权重（weight 字段）以及随每次负载均衡算法执行变化的 current 权重（current 字段）。

了解了 WeightedRoundRobin 这个内部类后，我们再来看 RoundRobinLoadBalance.doSelect() 方法的具体实现：

```
protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
    String key = invokers.get(0).getUrl().getServiceKey() + "." + invocation.getMethodName();
    ConcurrentMap<String, WeightedRoundRobin> map = methodWeightMap.computeIfAbsent(key, k -> new ConcurrentHashMap<>());
int totalWeight = 0;
long maxCurrent = Long.MIN_VALUE;
long now = System.currentTimeMillis(); 
    Invoker<T> selectedInvoker = null;
    WeightedRoundRobin selectedWRR = null;
for (Invoker<T> invoker : invokers) {
        String identifyString = invoker.getUrl().toIdentityString();
int weight = getWeight(invoker, invocation);
        WeightedRoundRobin weightedRoundRobin = map.computeIfAbsent(identifyString, k -> {
            WeightedRoundRobin wrr = new WeightedRoundRobin();
            wrr.setWeight(weight);
return wrr;
        });
if (weight != weightedRoundRobin.getWeight()) {
            weightedRoundRobin.setWeight(weight);
        }
long cur = weightedRoundRobin.increaseCurrent();
        weightedRoundRobin.setLastUpdate(now);
if (cur > maxCurrent) {
            maxCurrent = cur;
            selectedInvoker = invoker;
            selectedWRR = weightedRoundRobin;
        }
        totalWeight += weight; 
    }
if (invokers.size() != map.size()) {
        map.entrySet().removeIf(item -> now - item.getValue().getLastUpdate() > RECYCLE_PERIOD);
    }
if (selectedInvoker != null) {
        selectedWRR.sel(totalWeight);
return selectedInvoker;
    }
return invokers.get(0);
}
```

### ShortestResponseLoadBalance

ShortestResponseLoadBalance 是**Dubbo 2.7 版本之后新增加的一个 LoadBalance 实现类**。它实现了**最短响应时间的负载均衡算法**，也就是从多个 Provider 节点中选出调用成功的且响应时间最短的 Provider 节点，不过满足该条件的 Provider 节点可能有多个，所以还要再使用随机算法进行一次选择，得到最终要调用的 Provider 节点。

了解了 ShortestResponseLoadBalance 的核心原理之后，我们一起来看 ShortestResponseLoadBalance.doSelect() 方法的核心实现，如下所示：

```
protected <T> Invoker<T> doSelect(List<Invoker<T>> invokers, URL url, Invocation invocation) {
int length = invokers.size();
long shortestResponse = Long.MAX_VALUE;
int shortestCount = 0;
int[] shortestIndexes = new int[length];
int[] weights = new int[length];
int totalWeight = 0;
int firstWeight = 0;
boolean sameWeight = true;
for (int i = 0; i < length; i++) {
        Invoker<T> invoker = invokers.get(i);
        RpcStatus rpcStatus = RpcStatus.getStatus(invoker.getUrl(), invocation.getMethodName());
long succeededAverageElapsed = rpcStatus.getSucceededAverageElapsed();
int active = rpcStatus.getActive();
long estimateResponse = succeededAverageElapsed * active;
int afterWarmup = getWeight(invoker, invocation);
        weights[i] = afterWarmup;
if (estimateResponse < shortestResponse) { 
            shortestResponse = estimateResponse;
            shortestCount = 1;
            shortestIndexes[0] = i;
            totalWeight = afterWarmup;
            firstWeight = afterWarmup;
            sameWeight = true;
        } else if (estimateResponse == shortestResponse) {
            shortestIndexes[shortestCount++] = i;
            totalWeight += afterWarmup;
if (sameWeight && i > 0
                    && afterWarmup != firstWeight) {
                sameWeight = false;
            }
        }
    }
if (shortestCount == 1) {
return invokers.get(shortestIndexes[0]);
    }
if (!sameWeight && totalWeight > 0) {
int offsetWeight = ThreadLocalRandom.current().nextInt(totalWeight);
for (int i = 0; i < shortestCount; i++) {
int shortestIndex = shortestIndexes[i];
            offsetWeight -= weights[shortestIndex];
if (offsetWeight < 0) {
return invokers.get(shortestIndex);
            }
        }
    }
return invokers.get(shortestIndexes[ThreadLocalRandom.current().nextInt(shortestCount)]);
}
```

### 总结

今天我们紧接上一课时介绍了 LoadBalance 接口的剩余三个实现。

我们首先介绍了 LeastActiveLoadBalance 实现，它使用最小活跃数负载均衡算法，选择当前请求最少的 Provider 节点处理最新的请求；接下来介绍了 RoundRobinLoadBalance 实现，它使用加权轮询负载均衡算法，弥补了单纯的轮询负载均衡算法导致的问题，同时随着 Dubbo 版本的升级，也将其自身不够平滑的问题优化掉了；最后介绍了 ShortestResponseLoadBalance 实现，它会从响应时间最短的 Provider 节点中选择一个 Provider 节点来处理新请求。

下一课时，我们将开始介绍 Cluster 接口以及容错机制的相关内容，记得按时来听课。
