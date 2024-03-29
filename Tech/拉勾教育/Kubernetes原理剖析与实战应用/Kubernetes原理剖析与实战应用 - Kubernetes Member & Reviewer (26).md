---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=447#/detail/pc?id=4517
author: 
---

# [Kubernetes原理剖析与实战应用 - Kubernetes Member & Reviewer - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=447#/detail/pc?id=4517)


当一个 Pod 在 Kube-APIServer 中被创建出来以后，会被调度器调度，然后确定一个合适的节点，最终被这个节点上的 Kubelet 拉起，以容器状态运行。

那么 Kubelet 是如何跟容器打交道的呢，它是如何进行创建容器、获取容器状态等操作的呢？

今天我们就来了解一下。

### 容器运行时 （Container Runtime）

Kubelet 负责运行具体的 Pod，并维护其整个生命周期，为 Pod 提供存储、网络等必要的资源。但 Kubelet 本身并不负责真正的容器创建和逻辑管理，这些全部都是通过容器运行时（Container Runtime）完成的。大家平常熟知的 Docker 其实就是一种容器运行时，除此之外，还有[containerd](https://kubernetes.io/zh/docs/setup/production-environment/container-runtimes/#containerd)、[cri-o](https://kubernetes.io/zh/docs/setup/production-environment/container-runtimes/#cri-o)、[kata](https://katacontainers.io/)、[gVisor](https://gvisor.dev/) 等等。

下图就是 Kubelet 跟容器运行时进行的交互图：

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/6F/EA/CgqCHl-3Y6CAIjzjAAEbwUIQ2pI143.png)

图 1：[Kubelet 跟容器运行的交互图](https://www.threatstack.com/blog/diving-deeper-into-runtimes-kubernetes-cri-and-shims)

Kubelet 负责跟 kube-apiserver 进行数据同步，获取新的 Pod，并上报本机 Pod 的各个状态数据。Kubelet 通过调用容器运行时的接口完成容器的创建、容器状态的查询等工作。下图就是使用 Docker 作为容器的运行时。

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/6F/DE/Ciqc1F-3Y8OAGglUAAESe6PzHHQ855.png)  
图 2：[使用 Docker 作为容器的运行](https://www.threatstack.com/blog/diving-deeper-into-runtimes-kubernetes-cri-and-shims)

Docker 作为 Kubelet 内置支持的主要容器运行时，也是目前使用最为官方的容器运行时之一。

除了 Docker，在 Kubernetes v1.5 之前，Kubelet 还内置了对 [rkt](https://coreos.com/rkt/docs/latest/) 的支持。在这个阶段，如果我们想要自己去定义容器运行时，或者更改容器运行时的部分逻辑行为，是非常痛苦的，需要通过修改 Kubelet 的代码来实现。这些改动如果更新到上游社区，也会给社区造成很大的困扰，毕竟 Kubelet 自身的稳定性关乎着整个集群的稳定性。因此，这些改动在上游社区的合并通常都很谨慎，往往就需要开发者自己维护这些代码，维护成本非常高，也不方便升级。

介于这一点，很多开发者都希望 Kubernetes 可以支持更多的容器运行时。因此，从 v1.5 版本开始，社区引入了 [CRI（Container Runtime Interface）](https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes/)来解决这个问题。

CRI 接口的引入带来了两个好处：一是它很好地将 Kubelet 与容器运行时进行了解耦，这样我们每次对容器运行时进行更新升级等操作时，都再不需要对 Kubelet 做任何的更改；二是解放了 Kubelet，减少了 Kubelet 的负担，能够保证 Kubernetes 的代码质量和整个系统的稳定性。

下面我们就来了解一下CRI。

### CRI

CRI 接口可以分为两部分。

一个是容器运行时服务 RuntimeService，它主要负责管理 Pod 和容器的生命周期，比如创建容器、删除容器、查询容器状态等等。下面就是用[Protocol Buffers](https://developers.google.com/protocol-buffers)定义的 RuntimeService 的接口：

```
service RuntimeService {
rpc Version(VersionRequest) returns (VersionResponse) {}
rpc RunPodSandbox(RunPodSandboxRequest) returns (RunPodSandboxResponse) {}
rpc StartPodSandbox(StartPodSandboxRequest) returns (StartPodSandboxResponse) {}
rpc StopPodSandbox(StopPodSandboxRequest) returns (StopPodSandboxResponse) {}
rpc RemovePodSandbox(RemovePodSandboxRequest) returns (RemovePodSandboxResponse) {}
rpc PodSandboxStatus(PodSandboxStatusRequest) returns (PodSandboxStatusResponse) {}
rpc ListPodSandbox(ListPodSandboxRequest) returns (ListPodSandboxResponse) {}
rpc CreateContainer(CreateContainerRequest) returns (CreateContainerResponse) {}
rpc StartContainer(StartContainerRequest) returns (StartContainerResponse) {}
rpc StopContainer(StopContainerRequest) returns (StopContainerResponse) {}
rpc RemoveContainer(RemoveContainerRequest) returns (RemoveContainerResponse) {}
rpc PauseContainer(PauseContainerRequest) returns (PauseContainerResponse) {}
rpc UnpauseContainer(UnpauseContainerRequest) returns (UnpauseContainerResponse) {}
rpc ListContainers(ListContainersRequest) returns (ListContainersResponse) {}
rpc ContainerStatus(ContainerStatusRequest) returns (ContainerStatusResponse) {}
rpc UpdateContainerResources(UpdateContainerResourcesRequest) returns (UpdateContainerResourcesResponse) {}
rpc ReopenContainerLog(ReopenContainerLogRequest) returns (ReopenContainerLogResponse) {}
rpc ExecSync(ExecSyncRequest) returns (ExecSyncResponse) {}
rpc Exec(ExecRequest) returns (ExecResponse) {}
rpc Attach(AttachRequest) returns (AttachResponse) {}
rpc PortForward(PortForwardRequest) returns (PortForwardResponse) {}
rpc ContainerStats(ContainerStatsRequest) returns (ContainerStatsResponse) {}
rpc ListContainerStats(ListContainerStatsRequest) returns (ListContainerStatsResponse) {}
rpc UpdateRuntimeConfig(UpdateRuntimeConfigRequest) returns (UpdateRuntimeConfigResponse) {}
rpc Status(StatusRequest) returns (StatusResponse) {}
}
```

另一个部分是镜像服务 ImageService，主要负责容器镜像的生命周期管理，比如拉取镜像、删除镜像、查询镜像等等，如下所示：

```
service ImageService {
rpc ListImages(ListImagesRequest) returns (ListImagesResponse) {}
rpc ImageStatus(ImageStatusRequest) returns (ImageStatusResponse) {}
rpc PullImage(PullImageRequest) returns (PullImageResponse) {}
rpc RemoveImage(RemoveImageRequest) returns (RemoveImageResponse) {}
rpc ImageFsInfo(ImageFsInfoRequest) returns (ImageFsInfoResponse) {}
}
```

每一个容器运行时都需要自己实现一个 CRI shim，即完成对 CRI 这个抽象接口的具体实现。这样容器运行时就可以接收来自 Kubelet 的请求。

我们现在就来看看有了 CRI 接口以后，Kubelet 是如何和容器运行时进行交互的，见下图：

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/6F/DF/Ciqc1F-3ZBCAfnwJAACHtbND3KI539.png)

图 3：[Kubelet 与容器运行时的交互](https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes/))

从上图可以看出，新增的 CRI shim 是 Kubelet 和容器运行时之间的交互纽带，Kubelet 只需要跟 CRI shim 进行交互。Kubelet 调用 CRI shim 的接口，CRI shim 响应请求后会调用底层的运行容器时，完成对容器的相关操作。

这里我们需要将 Kubelet、CRI shim 以及容器运行时都部署在同一个节点上。一般来说，大多数的容器运行时都默认实现了 CRI 的接口，比如[containerd](https://containerd.io/docs/)。

目前 Kubelet 内部内置了对 Docker 的 CRI shim 的实现，见下图：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/6F/DF/Ciqc1F-3ZBmAdEVFAAAnf6SSCkk798.png)  
图 4：[Kubelet 内置对 CRI shim 的实现](https://dzone.com/articles/evolution-of-k8s-worker-nodes-cri-o)

而对于其他的容器运行时，比如[containerd](https://kubernetes.io/zh/docs/setup/production-environment/container-runtimes/#containerd)，我们就需要配置 kubelet 的 --container-runtime 参数为 remote，并设置 --container-runtime-endpoint 为对应的容器运行时的监听地址。

Kubernetes 自 v1.10 版本已经完成了和 containerd 1.1版本 的 GA 集成，你可以直接按照[这份文档](https://kubernetes.io/zh/docs/setup/production-environment/container-runtimes/#containerd)来部署 containerd 作为你的容器运行时。

![Drawing 5.png](https://s0.lgstatic.com/i/image/M00/6F/EA/CgqCHl-3ZCKAA7C9AABJ2r60MV4161.png)

图 5：部署 containerd

containerd 1.1 版本已经内置了对 CRI 的实现，比直接使用 Docker 的性能要高很多。

### 写在最后

Kubernetes 作为容器编排调度领域的事实标准，其优秀的架构设计还体现在其可扩展接口上。比如 CRI 提供了简单易用的扩展接口，方便各个容器运行时跟 Kubelet 进行交互接入，极大地方便了用户进行定制化。

通过 CRI 对容器运行时进行抽象，我们无须修改 Kubelet 就可以天然地支持多种容器运行时，这极大地方便了开发者的对接，也减少了升级和维护成本。CRI 的出现也促进了容器运行时的繁荣，也为强隔离、多租户等复杂的场景带来了更多的选择。

除了 CRI 以外，在 Kubernetes 中还可以为不同的 Pod 设置不同的容器运行时（Container Runtime），以提供性能与安全性之间的平衡。从 1.12 版本开始，Kuberentes 就提供了 [RuntimeClass](https://kubernetes.io/zh/docs/concepts/containers/runtime-class/) 来实现这个功能。你可以阅读[这份官方文档](https://kubernetes.io/zh/docs/concepts/containers/runtime-class/)，来学习如何使用 RuntimeClass。

如果你对本节课有什么想法或者疑问，欢迎你在留言区留言，我们一起讨论。
