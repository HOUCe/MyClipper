---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256
author: 
---

# [Dubbo源码解读与实战 - 资深技术专家 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4256)


在前面的课时中，我们已经将整个 Dubbo 的核心实现进行了分析。接下来的两个课时，我们将串联 Dubbo 中的这些核心实现，分析 Dubbo**服务发布**和**服务引用**的全流程，帮助你将之前课时介绍的独立知识点联系起来，形成一个完整整体。

本课时我们就先来重点关注 Provider 节点发布服务的过程，在这个过程中会使用到之前介绍的很多 Dubbo 核心组件。我们从 DubboBootstrap 这个入口类开始介绍，分析 Provider URL 的组装以及服务发布流程，其中会详细介绍本地发布和远程发布的核心流程。

### DubboBootstrap 入口

在[第 01 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=393#/detail/pc?id=4257)dubbo-demo-api-provider 示例的 Provider 实现中我们可以看到，整个 Provider 节点的启动入口是 DubboBootstrap.start() 方法，在该方法中会执行一些初始化操作，以及一些状态控制字段的更新，具体实现如下：

```
public DubboBootstrap start() {
if (started.compareAndSet(false, true)) { 
        ready.set(false); 
        initialize(); 
        exportServices();
if (!isOnlyRegisterProvider() || hasExportedServices()) {
            exportMetadataService();
            registerServiceInstance();
        }
        referServices();
if (asyncExportingFutures.size() > 0) {
new Thread(() -> {
this.awaitFinish();
                ready.set(true);
            }).start();
        } else { 
            ready.set(true);
        }
    }
return this;
}
```

**不仅是直接通过 API 启动 Provider 的方式会使用到 DubboBootstrap，在 Spring 与 Dubbo 集成的时候也是使用 DubboBootstrap 作为服务发布入口的**，具体逻辑在 DubboBootstrapApplicationListener 这个 Spring Context 监听器中，如下所示：

```
public class DubboBootstrapApplicationListener extends OneTimeExecutionApplicationContextEventListener
implements Ordered {
private final DubboBootstrap dubboBootstrap;
public DubboBootstrapApplicationListener() {
this.dubboBootstrap = DubboBootstrap.getInstance();
    }
@Override
public void onApplicationContextEvent(ApplicationContextEvent event) {
if (event instanceof ContextRefreshedEvent) {
            onContextRefreshedEvent((ContextRefreshedEvent) event);
        } else if (event instanceof ContextClosedEvent) {
            onContextClosedEvent((ContextClosedEvent) event);
        }
    }
private void onContextRefreshedEvent(ContextRefreshedEvent event) {
        dubboBootstrap.start(); 
    }
private void onContextClosedEvent(ContextClosedEvent event) {
        dubboBootstrap.stop();
    }
@Override
public int getOrder() {
return LOWEST_PRECEDENCE;
    }
}
```

这里我们重点关注的是**exportServices() 方法，它是服务发布核心逻辑的入口**，其中每一个服务接口都会转换为对应的 ServiceConfig 实例，然后通过代理的方式转换成 Invoker，最终转换成 Exporter 进行发布。服务发布流程中涉及的核心对象转换，如下图所示：

![Lark20201215-163844.png](https://s0.lgstatic.com/i/image/M00/89/79/Ciqc1F_YdkGABhTFAACpT-2oDtw867.png)

服务发布核心流程图

exportServices() 方法的具体实现如下：

```
private void exportServices() {
    configManager.getServices().forEach(sc -> {
        ServiceConfig serviceConfig = (ServiceConfig) sc;
        serviceConfig.setBootstrap(this);
if (exportAsync) { 
            ExecutorService executor = executorRepository.getServiceExporterExecutor();
            Future<?> future = executor.submit(() -> {
                sc.export();
                exportedServices.add(sc);
            });
            asyncExportingFutures.add(future);
        } else {
            sc.export(); 
            exportedServices.add(sc);
        }
    });
}
```

### ServiceConfig

在 ServiceConfig.export() 方法中，服务发布的第一步是检查参数，第二步会根据当前配置决定是延迟发布还是立即调用 doExport() 方法进行发布，第三步会通过 exported() 方法回调相关监听器，具体实现如下：

```
public synchronized void export() {
if (!shouldExport()) {
return;
    }
if (bootstrap == null) {
        bootstrap = DubboBootstrap.getInstance();
        bootstrap.init();
    }
    checkAndUpdateSubConfigs();
    ... 
if (shouldDelay()) { 
        DELAY_EXPORT_EXECUTOR.schedule(this::doExport, getDelay(), TimeUnit.MILLISECONDS);
    } else { 
        doExport();
    }
    exported(); 
}
```

在 checkAndUpdateSubConfigs() 方法中，会去检查各项配置是否合理，并补齐一些缺省的配置信息，这个方法非常冗长，这里就不再展示，你若感兴趣的话可以参考[源码](https://github.com/xxxlxy2008/dubbo)进行学习。

完成配置的检查之后，再来看 doExport() 方法，其中首先调用 loadRegistries() 方法加载注册中心信息，即将 RegistryConfig 配置解析成 registryUrl。无论是使用 XML、Annotation，还是 API 配置方式，都可以配置多个注册中心地址，一个服务接口可以同时注册在多个不同的注册中心。

RegistryConfig 是 Dubbo 的多个配置对象之一，可以通过解析 XML、Annotation 中注册中心相关的配置得到，对应的配置如下（当然，也可以直接通过 API 创建得到）：

```
<dubbo:registry address="zookeeper://127.0.0.1:2181" protocol="zookeeper" port="2181" />
```

RegistryUrl 的格式大致如下（为了方便查看，这里将每个 URL 参数单独放在一行中展示）：

```
registry:
application=dubbo-demo-api-provider
&dubbo=2.0.2
&pid=9405
&registry=zookeeper 
&timestamp=1600307343086
```

加载注册中心信息得到 RegistryUrl 之后，会遍历所有的 ProtocolConfig，依次调用 doExportUrlsFor1Protocol(protocolConfig, registryURLs) 在每个注册中心发布服务。一个服务接口可以以多种协议进行发布，每种协议都对应一个 ProtocolConfig，例如我们在 Demo 示例中，只使用了 dubbo 协议，对应的配置是：`<dubbo:protocol name="dubbo" />`。

### 组装服务 URL

doExportUrlsFor1Protocol() 方法的代码非常长，这里我们分成两个部分进行介绍：一部分是组装服务的 URL，另一部分就是后面紧接着介绍的服务发布。

**组装服务的 URL**核心步骤有如下 7 步。

1.  获取此次发布使用的协议，默认使用 dubbo 协议。
    
2.  设置服务 URL 中的参数，这里会从 MetricsConfig、ApplicationConfig、ModuleConfig、ProviderConfig、ProtocolConfig 中获取配置信息，并作为参数添加到 URL 中。这里调用的 appendParameters() 方法会将 AbstractConfig 中的配置信息存储到 Map 集合中，后续在构造 URL 的时候，会将该集合中的 KV 作为 URL 的参数。
    
3.  解析指定方法的 MethodConfig 配置以及方法参数的 ArgumentConfig 配置，得到的配置信息也是记录到 Map 集合中，后续作为 URL 参数。
    
4.  根据此次调用是泛化调用还是普通调用，向 Map 集合中添加不同的键值对。
    
5.  获取 token 配置，并添加到 Map 集合中，默认随机生成 UUID。
    
6.  获取 host、port 值，并开始组装服务的 URL。
    
7.  根据 Configurator 覆盖或新增 URL 参数。
    

下面是 doExportUrlsFor1Protocol() 方法组装 URL 的核心实现：

```
private void doExportUrlsFor1Protocol(ProtocolConfig protocolConfig, List<URL> registryURLs) {
    String name = protocolConfig.getName(); 
if (StringUtils.isEmpty(name)) { 
        name = DUBBO;
    }
    Map<String, String> map = new HashMap<String, String>(); 
    map.put(SIDE_KEY, PROVIDER_SIDE); 
    ServiceConfig.appendRuntimeParameters(map);
    AbstractConfig.appendParameters(map, getMetrics());
    AbstractConfig.appendParameters(map, getApplication());
    AbstractConfig.appendParameters(map, getModule());
    AbstractConfig.appendParameters(map, provider);
    AbstractConfig.appendParameters(map, protocolConfig);
    AbstractConfig.appendParameters(map, this);
    MetadataReportConfig metadataReportConfig = getMetadataReportConfig();
if (metadataReportConfig != null && metadataReportConfig.isValid()) {
        map.putIfAbsent(METADATA_KEY, REMOTE_METADATA_STORAGE_TYPE);
    }
if (CollectionUtils.isNotEmpty(getMethods())) { 
for (MethodConfig method : getMethods()) {
            AbstractConfig.appendParameters(map, method, method.getName());
            String retryKey = method.getName() + ".retry";
if (map.containsKey(retryKey)) {
                String retryValue = map.remove(retryKey);
if ("false".equals(retryValue)) {
                    map.put(method.getName() + ".retries", "0");
                }
            }
            List<ArgumentConfig> arguments = method.getArguments();
if (CollectionUtils.isNotEmpty(arguments)) {
for (ArgumentConfig argument : arguments) { 
                    ... ...
                }
            }
        } 
    }
if (ProtocolUtils.isGeneric(generic)) { 
        map.put(GENERIC_KEY, generic);
        map.put(METHODS_KEY, ANY_VALUE);
    } else {
        String revision = Version.getVersion(interfaceClass, version);
if (revision != null && revision.length() > 0) {
            map.put(REVISION_KEY, revision);
        }
        String[] methods = Wrapper.getWrapper(interfaceClass).getMethodNames();
if (methods.length == 0) {
            map.put(METHODS_KEY, ANY_VALUE);
        } else {
            map.put(METHODS_KEY, StringUtils.join(new HashSet<String>(Arrays.asList(methods)), ","));
        }
    }
if(ConfigUtils.isEmpty(token) && provider != null) {
        token = provider.getToken();
    }
if (!ConfigUtils.isEmpty(token)) {
if (ConfigUtils.isDefault(token)) {
            map.put(TOKEN_KEY, UUID.randomUUID().toString());
        } else {
            map.put(TOKEN_KEY, token);
        }
    }
    serviceMetadata.getAttachments().putAll(map);
    String host = findConfigedHosts(protocolConfig, registryURLs, map);
    Integer port = findConfigedPorts(protocolConfig, name, map);
    URL url = new URL(name, host, port, getContextPath(protocolConfig).map(p -> p + "/" + path).orElse(path), map);
if (ExtensionLoader.getExtensionLoader(ConfiguratorFactory.class)
            .hasExtension(url.getProtocol())) {
        url = ExtensionLoader.getExtensionLoader(ConfiguratorFactory.class)
                .getExtension(url.getProtocol()).getConfigurator(url).configure(url);
    }
    ... ...
}
```

经过上述准备操作之后，得到的服务 URL 如下所示（为了方便查看，这里将每个 URL 参数单独放在一行中展示）：

```
dubbo:
anyhost=true
&application=dubbo-demo-api-provider
&bind.ip=172.17.108.185
&bind.port=20880
&default=true
&deprecated=false
&dubbo=2.0.2
&dynamic=true
&generic=false
&interface=org.apache.dubbo.demo.DemoService
&methods=sayHello,sayHelloAsync
&pid=3918
&release=
&side=provider
&timestamp=1600437404483
```

### 服务发布入口

完成了服务 URL 的组装之后，doExportUrlsFor1Protocol() 方法开始执行服务发布。服务发布可以分为**远程发布**和**本地发布**，具体发布方式与服务 URL 中的 scope 参数有关。

scope 参数有三个可选值，分别是 none、remote 和 local，分别代表不发布、发布到本地和发布到远端注册中心，从下面介绍的 doExportUrlsFor1Protocol() 方法代码中可以看到：

-   发布到本地的条件是 scope != remote；
    
-   发布到注册中心的条件是 scope != local。
    

**scope 参数的默认值为 null**，也就是说，默认会同时在本地和注册中心发布该服务。下面来看 doExportUrlsFor1Protocol() 方法中发布服务的具体实现：

```
private void doExportUrlsFor1Protocol(ProtocolConfig protocolConfig, List<URL> registryURLs) {
    ... ...
    String scope = url.getParameter(SCOPE_KEY);
if (!SCOPE_NONE.equalsIgnoreCase(scope)) { 
if (!SCOPE_REMOTE.equalsIgnoreCase(scope)) {
            exportLocal(url);
        }
if (!SCOPE_LOCAL.equalsIgnoreCase(scope)) { 
if (CollectionUtils.isNotEmpty(registryURLs)) { 
for (URL registryURL : registryURLs) { 
if (LOCAL_PROTOCOL.equalsIgnoreCase(url.getProtocol())){
continue;
                    }
                    url = url.addParameterIfAbsent(DYNAMIC_KEY, registryURL.getParameter(DYNAMIC_KEY));
                    URL monitorUrl = ConfigValidationUtils.loadMonitor(this, registryURL);
if (monitorUrl != null) {
                        url = url.addParameterAndEncoded(MONITOR_KEY, monitorUrl.toFullString());
                    }
                    String proxy = url.getParameter(PROXY_KEY);
if (StringUtils.isNotEmpty(proxy)) {
                        registryURL = registryURL.addParameter(PROXY_KEY, proxy);
                    }
                    Invoker<?> invoker = PROXY_FACTORY.getInvoker(ref, (Class) interfaceClass, registryURL.addParameterAndEncoded(EXPORT_KEY, url.toFullString()));
                    DelegateProviderMetaDataInvoker wrapperInvoker = new DelegateProviderMetaDataInvoker(invoker, this);
                    Exporter<?> exporter = PROTOCOL.export(wrapperInvoker);
                    exporters.add(exporter);
                }
            } else {
                Invoker<?> invoker = PROXY_FACTORY.getInvoker(ref, (Class) interfaceClass, url);
                DelegateProviderMetaDataInvoker wrapperInvoker = new DelegateProviderMetaDataInvoker(invoker, this);
                Exporter<?> exporter = PROTOCOL.export(wrapperInvoker);
                exporters.add(exporter);
            }
            WritableMetadataService metadataService = WritableMetadataService.getExtension(url.getParameter(METADATA_KEY, DEFAULT_METADATA_STORAGE_TYPE));
if (metadataService != null) {
                metadataService.publishServiceDefinition(url);
            }
        }
    }
this.urls.add(url);
}
```

### 本地发布

了解了本地发布、远程发布的入口逻辑之后，下面我们开始深入本地发布的逻辑。

在 exportLocal() 方法中，会将 Protocol 替换成 injvm 协议，将 host 设置成 127.0.0.1，将 port 设置为 0，得到新的 LocalURL，大致如下：

```
injvm:
&application=dubbo-demo-api-provider
&bind.ip=172.17.108.185
&bind.port=20880
&default=true
&deprecated=false
&dubbo=2.0.2
&dynamic=true
&generic=false
&interface=org.apache.dubbo.demo.DemoService
&methods=sayHello,sayHelloAsync
&pid=4249
&release=
&side=provider
&timestamp=1600440074214
```

之后，会通过 ProxyFactory 接口适配器找到对应的 ProxyFactory 实现（默认使用 JavassistProxyFactory），并调用 getInvoker() 方法创建 Invoker 对象；最后，通过 Protocol 接口的适配器查找到 InjvmProtocol 实现，并调用 export() 方法进行发布。 exportLocal() 方法的具体实现如下：

```
private void exportLocal(URL url) {
    URL local = URLBuilder.from(url) 
            .setProtocol(LOCAL_PROTOCOL)
            .setHost(LOCALHOST_VALUE)
            .setPort(0)
            .build();
    Exporter<?> exporter = PROTOCOL.export(
            PROXY_FACTORY.getInvoker(ref, (Class) interfaceClass, local));
    exporters.add(exporter);
}
```

InjvmProtocol 的相关实现比较简单，这里就不再展示，你若感兴趣的话可以参考[源码](https://github.com/xxxlxy2008/dubbo)进行学习。

### 远程发布

介绍完本地发布之后，我们再来看远程发布的核心逻辑，远程服务发布的流程相较本地发布流程，要复杂得多。

在 doExportUrlsFor1Protocol() 方法中，远程发布服务时，会遍历全部 RegistryURL，并根据 RegistryURL 选择对应的 Protocol 扩展实现进行发布。我们知道 RegistryURL 是 "registry://" 协议，所以这里使用的是 RegistryProtocol 实现。

下面来看 RegistryProtocol.export() 方法的核心流程：

```
public <T> Exporter<T> export(final Invoker<T> originInvoker) throws RpcException {
    URL registryUrl = getRegistryUrl(originInvoker);
    URL providerUrl = getProviderUrl(originInvoker);
final URL overrideSubscribeUrl = getSubscribedOverrideUrl(providerUrl);
final OverrideListener overrideSubscribeListener = new OverrideListener(overrideSubscribeUrl, originInvoker);
    overrideListeners.put(overrideSubscribeUrl, overrideSubscribeListener);
    providerUrl = overrideUrlWithConfig(providerUrl, overrideSubscribeListener);
final ExporterChangeableWrapper<T> exporter = doLocalExport(originInvoker, providerUrl);
final Registry registry = getRegistry(originInvoker);
final URL registeredProviderUrl = getUrlToRegistry(providerUrl, registryUrl);
boolean register = providerUrl.getParameter(REGISTER_KEY, true);
if (register) { 
        register(registryUrl, registeredProviderUrl);
    }
    registerStatedUrl(registryUrl, registeredProviderUrl, register);
    registry.subscribe(overrideSubscribeUrl, overrideSubscribeListener);
    exporter.setRegisterUrl(registeredProviderUrl);
    exporter.setSubscribeUrl(overrideSubscribeUrl);
    notifyExport(exporter);
return new DestroyableExporter<>(exporter);
}
```

我们可以看到，远程发布流程大致可分为下面 5 个步骤。

1.  准备 URL，比如 ProviderURL、RegistryURL 和 OverrideSubscribeUrl。
    
2.  发布 Dubbo 服务。在 doLocalExport() 方法中调用 DubboProtocol.export() 方法启动 Provider 端底层 Server。
    
3.  注册 Dubbo 服务。在 register() 方法中，调用 ZookeeperRegistry.register() 方法向 Zookeeper 注册服务。
    
4.  订阅 Provider 端的 Override 配置。调用 ZookeeperRegistry.subscribe() 方法订阅注册中心 configurators 节点下的配置变更。
    
5.  触发 RegistryProtocolListener 监听器。
    

远程发布的详细流程如下图所示：

![Drawing 1.png](https://s0.lgstatic.com/i/image2/M01/01/2C/CgpVE1_YNDaATl3fAAFcJTJOw3M699.png)

服务发布详细流程图

### 总结

本课时我们重点介绍了 Dubbo 服务发布的核心流程。

首先我们介绍了 DubboBootstrap 这个入口门面类中与服务发布相关的方法，重点是 start() 和 exportServices() 两个方法；然后详细介绍了 ServiceConfig 类的三个核心步骤：检查参数、立即（或延迟）执行 doExport() 方法进行发布、回调服务发布的相关监听器。

接下来，我们分析了**doExportUrlsFor1Protocol() 方法，它是发布一个服务的入口，也是规定服务发布流程的地方**，其中涉及 Provider URL 的组装、本地服务发布流程以及远程服务发布流程，对于这些步骤，我们都进行了详细的分析。

下一课时，我们将继续分析 Dubbo 服务引用的全流程，记得按时来听课。
