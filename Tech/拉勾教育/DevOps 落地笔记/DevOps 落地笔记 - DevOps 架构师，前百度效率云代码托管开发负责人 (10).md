---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743
author: 
---

# [DevOps 落地笔记 - DevOps 架构师，前百度效率云代码托管开发负责人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743)


上一讲我主要介绍在开发过程中如何处理应用程序在不同环境的配置问题，通过有效管理应用程序的配置，最终实现一包到底。不同的环境不仅会带来应用程序本身的配置管理问题，环境本身的创建、管理、一致性等问题也需要解决。环境管理的问题也是我目前工作的一个重点，由于业务需要，对环境的数量、环境部署的速度有很高的要求。如何快速的交付一套可用的环境就变得格外重要。这也是这个课时要介绍的内容——环境管理。

### 什么是环境管理？

在介绍环境管理之前，我们先来看看**环境**是指什么？环境是指**应用程序运行所需要的所有资源和配置信息**。比如下面的配置信息：

-   服务器的硬件信息，比如 CPU 的类型和数量、内存大小、硬盘和网卡信息，以及服务器之间互联使用的交换机信息。
    
-   操作系统的信息，操作系统的类型和版本，操作系统的优化配置，如文件描述符的数量。
    
-   应用程序运行所需要的中间件信息，如消息中间件 Kafka 的版本及配置信息。
    
-   应用程序本身的版本、数据及配置信息。
    

**环境管理就是准备部署环境的过程以及部署之后对环境的管控。既能保证准备环境的快速和一致性，又使得部署后的环境能够有效利用**。希望你学习完本课之后能够达到这个目标。

### 为什么环境管理？

下面来介绍一下做这件事情的意义。在之前的开发过程中，环境部署基本都是 IT 人员手动完成的。手动部署环境就会导致很多问题，比如：

-   **手动配置环境很容易出错**。由于环境的配置项较多，哪怕是改错一个配置项，都会让整个应用无法启动，或者影响服务的性能。
    
-   **手动配置环境非常低效**。通常以周为单位，拖慢了开发、测试的进度。
    
-   **手动配置环境不方便测试验证**。如果出现问题，很难复现，因此给测试验证带来了很大的困难。
    

因此，我们需要一种能够完全自动化的创建环境的过程。这不仅降低了环境管理的成本和风险，能够快速的交付环境，并且自动化的过程使得环境的部署是可重复的、时间是可预测的。

### 环境管理的实现

为了解决上面提到的问题，每个企业的实施方案并不完全一样。这就要根据落地 DevOps 的过程，采用中因地制宜的方法。有的企业环境的数量、用途是固定的，SIT 测试环境就是用来做集成测试的；性能测试的环境就是用来做性能测试的。一般情况下，环境的物理服务器、操作系统是不会经常发生变化的，变化的只是上层应用程序以及依赖的中间件服务。这时可以完全采用**基于 Git 的配置管理方式**来快速创建环境（参考上讲内容）。

这一讲要介绍的内容，包含**硬件物理服务器**、**操作系统**、**上层的中间件**、**应用程序**在内的所有元素的管理。我们可以认为，这是从裸金属服务器如何搭建成一套可用的环境。这个场景一般是在企业内的基础设施团队或者云基础设施的场景中用的比较多。下图是该方案的实施示意图。

![Drawing 0.png](https://s0.lgstatic.com/i/image2/M01/03/D3/CgpVE1_i8x2AEhusAACc3FwNqlA094.png)

这个方案主要包含三个内容，**Git 仓库**、**配置管理数据库（CMDB）**和**部署平台**。

-   **Git 仓库**：用于存储创建环境的部署脚本，并能够实现版本控制。部署脚本中尽可能将变化的信息抽取成变量，在部署的时候从 CMDB 中获取。比如，在哪些服务器上部署，部署哪些服务，用哪个操作系统镜像等。因此，这里的部署脚本可以理解为模板。
    
-   **CMDB**：用于存储每个环境相关的配置信息。包含**硬件信息**，如物理服务器的信息，CPU型号和数量，内存大小，以及硬盘、网卡等信息；**软件信息**，如部署哪些应用服务以及服务本身的启动参数和运行参数（数据库连接池大小等）。为了实现模块化部署，每个服务可以设置是否安装等参数。
    
-   **部署平台**：用于从 Git 仓库克隆部署脚本模板，从 CMDB 获取指定环境的硬件配置信息和软件配置信息，用 CMDB 中的数据填充部署脚本模板中的变量。然后执行具体的部署任务，创建出符合要求的环境。
    

这种基于 Git 版本控制系统存储脚本模板，利用CMDB存储个性化的配置项，最终通过部署平台进行组装的方式，可以快速地、可重复地创建一致性的环境。即通过这种方式，每次创建的环境 A 都是一样的。如果这个环境的配置信息发生改变，只需要在 CMDB 中进行变更即可，再次部署时就会应用到新的环境中。下面详细介绍一下这三个部分。

#### Git 仓库

采用 Git 仓库存储创建环境的部署脚本的实践，和基础设施即代码的实践是一样的。现在有个通用的术语来表示环境——基础设施。\*\*基础设施即代码，就是基于软件开发实践的基础设施自动化的方法。通过对代码进行更改，实现构建和变更基础设施的一致性和可重复性。并通过自动化测试等实践验证基础设施的可用性。\*\*基础设施即代码是目前快速、可靠的构建高质量基础设施的重要方法。

通过代码的方式来存储部署脚本的好处有以下三点。

-   **可重用性**：如果将一个环境定义为代码，可以用一份代码创建一个环境的多个实例。当发现问题时，也可以快速修复代码并重建环境。
    
-   **一致性**：通过代码创建的环境，每次都以相同的方式构建，保证了每次创建环境的一致性。因为都是相同的构建代码和构建方式，每次创建环境所需的时间是可预测的。
    
-   **透明性**：创建环境的代码每个人都可以查看，了解代码的构建逻辑，并提出改进建议。使得每个人都能学习、掌握创建环境中用到的技能，同时也提高了代码的健壮性。
    

#### 配置管理数据库

配置管理数据库（CMDB）是用于存储跟**硬件**和**软件**相关的配置信息。从上面也能看出，环境中涉及的元素很多，配置项也很多，并且也会经常新增模型和配置项。因此，如果要让 CMDB 解决环境部署的问题，就需要考虑下面这几个问题：

-   如何建模才能包含环境中涉及的元素，以及元素与元素之间的关联关系？
    
-   数据如何录入？能不能自动化？能不能自定义查询接口？
    
-   如何与部署脚本集成，获取哪些数据？
    

带着这几个问题，我们继续后面的内容。

**1\. 模型架构**。

CMDB 中模型的设计采取了平铺的方式。不管是硬件模型，如机房、机柜、交换机、服务器等等，还是软件模型，如业务相关的具体服务、进程等，都统一采用模型进行定义。每个模型可以包含多个实例，模型与模型之间以及实例与实例之间都是可以建立关联的。在模型和实例之上是业务和环境。一个业务包含多个环境，模型属于业务，实例属于环境。如下图所示：

![Drawing 1.png](https://s0.lgstatic.com/i/image2/M01/03/D1/Cip5yF_i8zOACxq7AAB2OUc-GMo871.png)

**2\. 数据准备。**

CMDB 中数据的准备工作量非常大，完全依靠人工是不现实的。在数据准备这部分，我们采取了**自动扫描**和**人工录入**两种方式。

-   **自动扫描**：基于 ironic-inspector 收集裸机的信息，能够自动扫描服务器的基本信息，如 CPU 型号和数量、内存大小，硬盘和网卡的信息，以及服务器的网卡上联的交换机以及网卡信息。基于扫描的数据就可以形成该环境的完整物理拓扑。
    
-   **人工录入**：最初的初始化数据需要由人工整理导入到 CMDB 中，比如服务器所在的机房、机柜、IPMI 的 IP、IPMI 的账号和密码等，这些数据是自动扫描的基础数据。另外，和当前环境相关的业务数据会由人工录入或者基于当前值进行人工确认。比如，哪些服务属于哪个服务器组，服务器组包含哪些服务器等等。
    

总之，数据准备的原则是**减少人工操作**、**尽量自动化**，**提高数据准备的效率**和**准确率**。该阶段完成后，创建环境需要的几个关键信息就比较清楚了：

-   该环境中的物理服务器信息；
    
-   该环境需要部署的服务信息；
    
-   服务部署在哪些服务器上的信息。
    

**3\. 查询接口。**

CMDB 通过查询接口对外提供数据查询能力。在设计时 CMDB 很关键的一点是灵活性。主要体现在以下几点上。

-   **模型**：支持添加任意模型。
    
-   **配置项**：支持添加任意类型的配置项。
    
-   **查询接口**：支持查询任意模型下、任意实例的配置项及关联关系。
    

CMDB 是配置管理数据库，对配置项的维护和查询需求是多种多样的。不用修改代码即可满足这种灵活性，这是一个难点，也是一个优势。目前我们基本上可以满足上面的灵活性，添加查询接口也可以在线添加。

#### 部署平台

经过上面的步骤，创建环境所使用的部署脚本模板和相对应的配置项都已经准备好了，下面就通过部署平台将其进行组装，并执行具体的创建环境的任务。分为几个步骤：

**STEP 1：克隆代部署脚本模块。**

```
$git clone https://xxxx/deploy.git
```

**STEP 2：调用CMDB接口并封装成部署所需要的文件。**  
对于搭建一套环境，部署脚本并不能直接使用的 CMDB 中数据，需要调用 CMDB 接口封装成部署所需要的文件，并将其放置于指定的位置。比如：

##### 1\. 生成 ansible 的 hosts 文件，内容示例为如下。

```
[service-a]
hostname-1 ansible_ssh_host=192.168.1.20
[service-a:vars]
ansible_ssh_user=root
ansible_ssh_pass="123456"
deploy_type="docker"
```

##### 2\. 生成 /etc/hosts 文件，内容示例如下。

```
192.168.1.101 hostname-1
192.168.1.102 hostname-2
192.168.1.103 hostname-3
192.168.1.104 hostname-4
192.168.1.105 hostname-5
```

##### STEP 3：执行部署脚本。

通过上面三个步骤就能够创建一套环境。如果要变更服务的部署方式，比如：将 deploy\_type 改为 K8s，只需要在 CMDB 中将该配置项改为 K8s，然后从 STEP 2 再次执行即可。这是因为，在部署脚本的模板里已经有了 K8s 部署的脚本，在 K8s 基础组件已经具备的前提下，不需要做其他任何改动，该服务就会以 K8s 的方式进行部署。  
最后，我建议一开始就考虑自动化，通过部署平台将整个环境部署的流程以自动化的方式实现，因为：

-   环境的变更比你想象得要频繁得多；
    
-   自动化执行环境配置可以保证从一开始就是一致的；
    
-   自动化帮助快速可靠的实施变更，速度造就了质量，质量保证了速度。
    

### 总结

本讲主要介绍了如何快速的、可重复的搭建测试环境，解决在软件开发过程中对环境依赖的问题。采用基础设施即代码和 CMDB 结合的方式，在代码库中提供部署模板，在 CMDB 中提供具体环境的配置信息，在部署环境时将该环境具体的配置项替换到模板中，即可完成部署。因为业务需要会经常将环境从操作系统到上层应用整体重推。

这个方案是目前我们处理环境问题的方案。如果你有更好的想法，欢迎留言交流。
