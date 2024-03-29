---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256
author: 
---

# [Dubbo源码解读与实战 - 资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256)


在前面[第 43 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4280)中介绍 Dubbo 的服务自省方案时，我们可以看到除了需要元数据方案的支持之外，还需要服务发布订阅功能的支持，这样才能构成完整的服务自省架构。

本课时我们就来讲解一下 Dubbo 中服务实例的发布与订阅功能的具体实现：首先说明 ServiceDiscovery 接口的核心定义，然后再重点介绍以 ZooKeeper 为注册中心的 ZookeeperServiceDiscovery 实现，这其中还会涉及相关事件监听的实现。

### ServiceDiscovery 接口

**ServiceDiscovery 主要封装了针对 ServiceInstance 的发布和订阅操作**，你可以暂时将其理解成一个 ServiceInstance 的注册中心。ServiceDiscovery 接口的定义如下所示：

```
@SPI("zookeeper")
public interface ServiceDiscovery extends Prioritized {
void initialize(URL registryURL) throws Exception;
void destroy() throws Exception;
void register(ServiceInstance serviceInstance) throws RuntimeException;
void update(ServiceInstance serviceInstance) throws RuntimeException;
void unregister(ServiceInstance serviceInstance) throws RuntimeException;
Set<String> getServices();
default int getDefaultPageSize() {
return 100;
    }
default List<ServiceInstance> getInstances(String serviceName) throws NullPointerException {
        List<ServiceInstance> allInstances = new LinkedList<>();
int offset = 0;
int pageSize = getDefaultPageSize();
        Page<ServiceInstance> page = getInstances(serviceName, offset, pageSize);
        allInstances.addAll(page.getData());
while (page.hasNext()) {
            offset += page.getDataSize();
            page = getInstances(serviceName, offset, pageSize);
            allInstances.addAll(page.getData());
        }
return unmodifiableList(allInstances);
    }
default Page<ServiceInstance> getInstances(String serviceName, int offset, int pageSize) throws NullPointerException,
            IllegalArgumentException {
return getInstances(serviceName, offset, pageSize, false);
    }
default Page<ServiceInstance> getInstances(String serviceName, int offset, int pageSize, boolean healthyOnly) throws
            NullPointerException, IllegalArgumentException, UnsupportedOperationException {
throw new UnsupportedOperationException("Current implementation does not support pagination query method.");
    }
default Map<String, Page<ServiceInstance>> getInstances(Iterable<String> serviceNames, int offset, int requestSize) throws
            NullPointerException, IllegalArgumentException {
        Map<String, Page<ServiceInstance>> instances = new LinkedHashMap<>();
for (String serviceName : serviceNames) {
            instances.put(serviceName, getInstances(serviceName, offset, requestSize));
        }
return unmodifiableMap(instances);
    }
default void addServiceInstancesChangedListener(ServiceInstancesChangedListener listener)
throws NullPointerException, IllegalArgumentException {
    }
default void dispatchServiceInstancesChangedEvent(String serviceName) {
        dispatchServiceInstancesChangedEvent(serviceName, getInstances(serviceName));
    }
default void dispatchServiceInstancesChangedEvent(String serviceName, String... otherServiceNames) {
        dispatchServiceInstancesChangedEvent(serviceName, getInstances(serviceName));
if (otherServiceNames != null) {
            Stream.of(otherServiceNames)
                    .filter(StringUtils::isNotEmpty)
                    .forEach(this::dispatchServiceInstancesChangedEvent);
        }
    }
default void dispatchServiceInstancesChangedEvent(String serviceName, Collection<ServiceInstance> serviceInstances) {
        dispatchServiceInstancesChangedEvent(new ServiceInstancesChangedEvent(serviceName, serviceInstances));
    }
default void dispatchServiceInstancesChangedEvent(ServiceInstancesChangedEvent event) {
        getDefaultExtension().dispatch(event);
    }
}
```

ServiceDiscovery 接口被 @SPI 注解修饰，是一个扩展点，针对不同的注册中心，有不同的 ServiceDiscovery 实现，如下图所示：

![Lark20201229-160604.png](https://s0.lgstatic.com/i/image/M00/8C/4D/Ciqc1F_q45aAGn14AAEh58Guyew441.png)

ServiceDiscovery 继承关系图

在 Dubbo 创建 ServiceDiscovery 对象的时候，会通过 ServiceDiscoveryFactory 工厂类进行创建。ServiceDiscoveryFactory 接口也是一个扩展接口，Dubbo 只提供了一个默认实现—— DefaultServiceDiscoveryFactory，其继承关系如下图所示：

![Lark20201229-160606.png](https://s0.lgstatic.com/i/image2/M01/04/32/CgpVE1_q4_iAZ8ARAAEu4mMS65Y213.png)

ServiceDiscoveryFactory 继承关系图

在 AbstractServiceDiscoveryFactory 中维护了一个 ConcurrentMap<String, ServiceDiscovery> 类型的集合（discoveries 字段）来缓存 ServiceDiscovery 对象，并提供了一个 createDiscovery() 抽象方法来创建 ServiceDiscovery 实例。

```
public ServiceDiscovery getServiceDiscovery(URL registryURL) {
    String key = registryURL.toServiceStringWithoutResolving();
return discoveries.computeIfAbsent(key, k -> createDiscovery(registryURL));
}
```

在 DefaultServiceDiscoveryFactory 中会实现 createDiscovery() 方法，使用 Dubbo SPI 机制获取对应的 ServiceDiscovery 对象，具体实现如下：

```
protected ServiceDiscovery createDiscovery(URL registryURL) {
    String protocol = registryURL.getProtocol();
    ExtensionLoader<ServiceDiscovery> loader = getExtensionLoader(ServiceDiscovery.class);
return loader.getExtension(protocol);
}
```

### ZookeeperServiceDiscovery 实现分析

Dubbo 提供了多个 ServiceDiscovery 用来接入多种注册中心，下面我们以 ZookeeperServiceDiscovery 为例介绍 Dubbo 是如何接入 ZooKeeper 作为注册中心，实现服务实例发布和订阅的。

**在 ZookeeperServiceDiscovery 中封装了一个 Apache Curator 中的 ServiceDiscovery 对象来实现与 ZooKeeper 的交互**。在 initialize() 方法中会初始化 CuratorFramework 以及 Curator ServiceDiscovery 对象，如下所示：

```
public void initialize(URL registryURL) throws Exception {
    ... 
this.curatorFramework = buildCuratorFramework(registryURL);
this.rootPath = ROOT_PATH.getParameterValue(registryURL);
this.serviceDiscovery = buildServiceDiscovery(curatorFramework, rootPath);
this.serviceDiscovery.start();
}
```

在 ZookeeperServiceDiscovery 中的方法基本都是调用 Curator ServiceDiscovery 对象的相应方法实现，例如，register()、update() 、unregister() 方法都会调用 Curator ServiceDiscovery 对象的相应方法完成 ServiceInstance 的添加、更新和删除。这里我们以 register() 方法为例：

```
public void register(ServiceInstance serviceInstance) throws RuntimeException {
    doInServiceRegistry(serviceDiscovery -> {
        serviceDiscovery.registerService(build(serviceInstance));
    });
}
public static org.apache.curator.x.discovery.ServiceInstance<ZookeeperInstance> build(ServiceInstance serviceInstance) {
    ServiceInstanceBuilder builder = null;
    String serviceName = serviceInstance.getServiceName();
    String host = serviceInstance.getHost();
int port = serviceInstance.getPort();
    Map<String, String> metadata = serviceInstance.getMetadata();
    String id = generateId(host, port);
    ZookeeperInstance zookeeperInstance = new ZookeeperInstance(null, serviceName, metadata);
    builder = builder().id(id).name(serviceName).address(host).port(port)
            .payload(zookeeperInstance);
return builder.build();
}
```

除了上述服务实例发布的功能之外，在服务实例订阅的时候，还会用到 ZookeeperServiceDiscovery 查询服务实例的信息，这些方法都是直接依赖 Apache Curator 实现的，例如，getServices() 方法会调用 Curator ServiceDiscovery 的 queryForNames() 方法查询 Service Name，getInstances() 方法会通过 Curator ServiceDiscovery 的 queryForInstances() 方法查询 Service Instance。

### EventListener 接口

ZookeeperServiceDiscovery 除了实现了 ServiceDiscovery 接口之外，还实现了 EventListener 接口，如下图所示：

![Drawing 2.png](https://s0.lgstatic.com/i/image2/M01/04/18/Cip5yF_petCAV9sXAAB9u4EYOqk073.png)

ZookeeperServiceDiscovery 继承关系图

也就是说，**ZookeeperServiceDiscovery 本身也是 EventListener 实现，可以作为 EventListener 监听某些事件**。下面我们先来看 Dubbo 中 EventListener 接口的定义，其中关注三个方法：onEvent() 方法、getPriority() 方法和 findEventType() 工具方法。

```
@SPI
@FunctionalInterface
public interface EventListener<E extends Event> extends java.util.EventListener, Prioritized {
void onEvent(E event); 
default int getPriority() { 
return MIN_PRIORITY;
    }
static Class<? extends Event> findEventType(EventListener<?> listener) {
return findEventType(listener.getClass());
    }
static Class<? extends Event> findEventType(Class<?> listenerClass) {
        Class<? extends Event> eventType = null;
if (listenerClass != null && EventListener.class.isAssignableFrom(listenerClass)) {
            eventType = findParameterizedTypes(listenerClass)
                    .stream()
                    .map(EventListener::findEventType) // 获取listenerClass中定义的Event泛型
                    .filter(Objects::nonNull)
                    .findAny()
                    // 获取listenerClass父类中定义的Event泛型
                    .orElse((Class) findEventType(listenerClass.getSuperclass()));
        }
return eventType;
    }
    ... 
}
```

Dubbo 中有很多 EventListener 接口的实现，如下图所示：

![Drawing 3.png](https://s0.lgstatic.com/i/image2/M01/04/19/Cip5yF_petmAI2w9AAC5QQrgGjY394.png)

EventListener 继承关系图

我们先来重点关注 ZookeeperServiceDiscovery 这个实现，在其 onEvent() 方法（以及 addServiceInstancesChangedListener() 方法）中会调用 registerServiceWatcher() 方法重新注册：

```
public void onEvent(ServiceInstancesChangedEvent event) {
    String serviceName = event.getServiceName();
    registerServiceWatcher(serviceName);
}
protected void registerServiceWatcher(String serviceName) {
    String path = buildServicePath(serviceName);
    CuratorWatcher watcher = watcherCaches.computeIfAbsent(path, key ->
new ZookeeperServiceDiscoveryChangeWatcher(this, serviceName));
    curatorFramework.getChildren().usingWatcher(watcher).forPath(path);
}
```

**ZookeeperServiceDiscoveryChangeWatcher 是 ZookeeperServiceDiscovery 配套的 CuratorWatcher 实现**，其中 process() 方法实现会关注 NodeChildrenChanged 事件和 NodeDataChanged 事件，并调用关联的 ZookeeperServiceDiscovery 对象的 dispatchServiceInstancesChangedEvent() 方法，具体实现如下：

```
public void process(WatchedEvent event) throws Exception {
    Watcher.Event.EventType eventType = event.getType();
if (NodeChildrenChanged.equals(eventType) || NodeDataChanged.equals(eventType)) {
        zookeeperServiceDiscovery.dispatchServiceInstancesChangedEvent(serviceName);
    }
}
```

通过上面的分析我们可以知道，ZookeeperServiceDiscoveryChangeWatcher 的核心就是将 ZooKeeper 中的事件转换成了 Dubbo 内部的 ServiceInstancesChangedEvent 事件。

### EventDispatcher 接口

通过上面对 ZookeeperServiceDiscovery 实现的分析我们知道，它并没有对 dispatchServiceInstancesChangedEvent() 方法进行覆盖，那么在 ZookeeperServiceDiscoveryChangeWatcher 中调用的 dispatchServiceInstancesChangedEvent() 方法就是 ServiceDiscovery 接口中的默认实现。在该默认实现中，会通过 Dubbo SPI 获取 EventDispatcher 的默认实现，并分发 ServiceInstancesChangedEvent 事件，具体实现如下：

```
default void dispatchServiceInstancesChangedEvent(ServiceInstancesChangedEvent event) {
        EventDispatcher.getDefaultExtension().dispatch(event);
}
```

下面我们来看 EventDispatcher 接口的具体定义：

```
@SPI("direct")
public interface EventDispatcher extends Listenable<EventListener<?>> {
    Executor DIRECT_EXECUTOR = Runnable::run;
void dispatch(Event event);
default Executor getExecutor() {
return DIRECT_EXECUTOR;
    }
static EventDispatcher getDefaultExtension() {
return ExtensionLoader.getExtensionLoader(EventDispatcher.class).getDefaultExtension();
    }
}
```

EventDispatcher 接口被 @SPI 注解修饰，是一个扩展点，Dubbo 提供了两个具体实现——ParallelEventDispatcher 和 DirectEventDispatcher，如下图所示：

![Drawing 4.png](https://s0.lgstatic.com/i/image2/M01/04/19/Cip5yF_pew-AdtkyAAB-Epfg96E814.png)

EventDispatcher 继承关系图

在 AbstractEventDispatcher 中维护了两个核心字段。

-   listenersCache（ConcurrentMap<Class<? extends Event>, List> 类型）：用于记录监听各类型事件的 EventListener 集合。在 AbstractEventDispatcher 初始化时，会加载全部 EventListener 实现并调用 addEventListener() 方法添加到 listenersCache 集合中。
    
-   executor（Executor 类型）：该线程池在 AbstractEventDispatcher 的构造函数中初始化。在 AbstractEventDispatcher 收到相应事件时，由该线程池来触发对应的 EventListener 集合。
    

AbstractEventDispatcher 中的 addEventListener()、removeEventListener()、getAllEventListeners() 方法都是通过操作 listenersCache 集合实现的，具体实现比较简单，这里就不再展示，你若感兴趣的话可以参考[源码](https://github.com/xxxlxy2008/dubbo)进行学习。

AbstractEventDispatcher 中另一个要关注的方法是 dispatch() 方法，该方法**会从 listenersCache 集合中过滤出符合条件的 EventListener 对象，并按照串行或是并行模式进行通知**，具体实现如下：

```
public void dispatch(Event event) {
    Executor executor = getExecutor();
    executor.execute(() -> {
        sortedListeners(entry -> entry.getKey().isAssignableFrom(event.getClass()))
                .forEach(listener -> {
if (listener instanceof ConditionalEventListener) { 
                        ConditionalEventListener predicateEventListener = (ConditionalEventListener) listener;
if (!predicateEventListener.accept(event)) {
return;
                        }
                    }
                    listener.onEvent(event);
                });
    });
}
protected Stream<EventListener> sortedListeners(Predicate<Map.Entry<Class<? extends Event>, List<EventListener>>> predicate) {
return listenersCache
            .entrySet()
            .stream()
            .filter(predicate)
            .map(Map.Entry::getValue)
            .flatMap(Collection::stream)
            .sorted();
}
```

AbstractEventDispatcher 已经实现了 EventDispatcher 分发 Event 事件、通知 EventListener 的核心逻辑，然后在 ParallelEventDispatcher 和 DirectEventDispatcher 确定是并行通知模式还是串行通知模式即可。

在 ParallelEventDispatcher 中通知 EventListener 的线程池是 ForkJoinPool，也就是并行模式；在 DirectEventDispatcher 中使用的是 EventDispatcher.DIRECT\_EXECUTOR 线程池，也就是串行模式。这两个 EventDispatcher 的具体实现比较简单，这里就不再展示。

我们回到 ZookeeperServiceDiscovery，在其构造方法中会获取默认的 EventDispatcher 实现对象，并调用 addEventListener() 方法将 ZookeeperServiceDiscovery 对象添加到 listenersCache 集合中监听 ServiceInstancesChangedEvent 事件。ZookeeperServiceDiscovery 直接继承了 ServiceDiscovery 接口中 dispatchServiceInstancesChangedEvent() 方法的默认实现，并没有进行覆盖，在该方法中，会获取默认的 EventDispatcher 实现并调用 dispatch() 方法分发 ServiceInstancesChangedEvent 事件。

### 总结

在本课时，我们重点介绍了 Dubbo 服务自省方案中服务实例发布和订阅的基础。

首先，我们说明了 ServiceDiscovery 接口的核心定义，其中定义了服务实例发布和订阅的核心方法。接下来我们分析了以 ZooKeeper 作为注册中心的 ZookeeperServiceDiscovery 实现，其中还讲解了在 ZookeeperServiceDiscovery 上添加监听器的相关实现以及 ZookeeperServiceDiscovery 处理 ServiceInstancesChangedEvent 事件的机制。

下一课时，我们将继续介绍 Dubbo 服务自省方案中的服务实例发布以及订阅实现，记得按时来听课。
