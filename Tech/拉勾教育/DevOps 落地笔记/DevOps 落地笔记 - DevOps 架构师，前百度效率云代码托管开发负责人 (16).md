---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743
author: 
---

# [DevOps 落地笔记 - DevOps 架构师，前百度效率云代码托管开发负责人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743)


上一课时介绍了通过在应用程序中主动注入故障，以便提前发现问题这一混沌工程实践，来构建强大的弹性系统的方法。混沌工程可以在软件开发全生命周期的多个阶段进行，不一定就是生产环境，只是有些执行的条件只有生产环境才具备。 经过前面的几个课时，软件已经发布到生产环境，交付给用户使用了。今天的内容是一个案例分析，介绍一下百度效率云这一 DevOps 平台是如何做代码审查的。

### 效率云简介

效率云是百度自主研发的整套 DevOps 解决方案，涵盖了创新管理、产品管理、项目管理、代码托管、持续交付、微服务治理、线上反馈的软件开发全生命周期。核心功能有以下几点。

-   **项目管理 iCafe**：主要包含用户故事地图、迭代管理、需求定制化查询、文档协同、定制化报表等功能。基于这些功能团队可以进行产品的整体规划、需求的拆解和录入，可视化看板的定制等。
    
-   **代码托管 iCode**：除了包含代码托管功能之外、还提供了一系列可配置规则。iCode是基于开源代码评审工具 Gerrit 的代码托管平台，也是采用的 Change Request 机制，当开发人员提交代码后，就会触发 iCode 内置的代码检查和流水线检查，最大程度的保证代码入库的质量。
    
-   **持续交付 iPipe**：主要包含持续集成、持续交付等功能，内置了 maven、gradle、npm、pip 等多种语言的依赖管理和制品管理功能。支持基于主干、Branch 和 Change 的构建流水线，支持串行、并行两种任务执行策略以及手动、自动、定时多种触发策略。
    

除了上面的核心组件，还有代码扫描 iScan、制品管理 iRepo、接口测试组件 ITP、压力测试组件 dumeter 等。效率云分为百度内部版本、百度云上 SaaS 版本，以及独立部署的版本。

### 代码托管 iCode

代码审查这个实践主要跟效率云中的代码托管平台 iCode 相关，下面的内容主要介绍在 iCode 中是如何进行代码审查。与当前比较流行的 GitHub 和 GitLab 一样，iCode 也是基于分布式版本控制系统 Git 搭建的代码托管平台。二者区别在于 iCode 的权限管理、代码评审模型是基于 Google 的 Gerrit 实现的，而 GitHub 及 GitLab 则是原生设计的。

#### 开发协作模型

在之前的课时中也讲到，DevOps 中关于代码质量检查的原则是尽可能前置，代码问题发现的越早，修复的成本就越低。在代码托管平台中，不同的开发协作模型决定了代码检查的时机。这四种代码托管平台的开发协作模型如下：

-   Pull Request 模型——GitHub；
    
-   Merge Request 模型——GitLab；
    
-   Change Request 模型——Gerrit，iCode。
    

**第一，Pull Request 模型。**

Pull Request 模型是 GitHub 设计的开发协作模型，是为了满足全球各地的开发者共同参与开源项目开发的需求。GitHub Flow如下图所示。

![Drawing 0.png](https://s0.lgstatic.com/i/image/M00/8D/6F/CgqCHl_-uHqAdp9ZAABDWTuBuOA185.png)

主要包含几个步骤：

-   开发者如果想修复一个开源项目的 Bug，首先将开源项目的远程代码库fork到个人代码库中；
    
-   在个人代码库中修复该开源项目的 Bug；
    
-   将修改的代码提交到个人代码库中；
    
-   在 GitHub 中远程仓库发起一个 PR；
    
-   远程仓库的 Owner 同意后，代码即可合入远程仓库。
    

Pull Request 模型的**代码评审阶**段一般发生在**开发者向开源项目提交 PR**的时候。

**第二，Merge Request 模型。**

Merge Request 模型是 GitLab设计的开发协作模型，主要是满足企业内部团队之间的协作开发，是一种分支开发、主干发布的设计思想。GitLab Flow 如下图所示。

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/8D/64/Ciqc1F_-uJOAShU8AABFiWxsObU213.png)

主要包含几个步骤：

-   当团队成员开发一个新功能时，先从主干分支拉取新的特性分支；
    
-   在本地开发新功能；
    
-   将代码提交到该特性分支；
    
-   在 GitLab 中发起 MR，请求将该特性分支的代码合并到主干分支；
    
-   负责人对 MR 进行 Code Review，同意后点击 Merge，即可合入主干分支。
    

Merge Request 模型的**代码评审阶段**一般发生在**开发者将特性分支合并到主干分支**的时刻。

**第三，Change Request 模型。**

上面的**PR**和**MR**模型提供了基本管理方式，但在使用的过程中也存在一定的问题，比如：

-   特性分支需要在合并的时候才能进行 CodeReview，此时需要评审的代码量很多，无法保证 Code Review 的效果。
    
-   因为只有在分支合并的时候才能进行 CodeReview，每次需要建立新的分支和分支的合并，对于小型团队来说，增加了流程的复杂性。
    

Change Request 模型是 Gerrit 设计的开发协作模型，也是 iCode 目前采用的模型，能够将代码检查前置到代码提交阶段。Change Request 的开发协作流程如下图所示。

![Drawing 2.png](https://s0.lgstatic.com/i/image/M00/8D/6F/CgqCHl_-uJ2AO1RNAAA3dBtZ49Q669.png)

主要包含几个步骤：

-   执行 git pull， 将远程分支的代码更新到本地工作空间；
    
-   在本地工作空间开发新功能；
    
-   执行 git commit 将代码提交到本地仓库；
    
-   执行 git push HEAD:refs/for/branch 发起一个 Change Request；
    
-   经过流水线检查和 Code Review后，才能将代码合入代码库。
    

Change Request 模型的代码评审阶段是在代码提交阶段，而不是分支合并的时候。与 Pull Request 模型和 Merge Request 模型相比，Change Request 模型代码评审的时机更靠前。

#### 提交规则设置

上面介绍了 iCode 中采用的是 Change Request 的开发协作模型。在新建代码库后，默认并未开启该协作模型，可以在“提交规则”里进行相关设置，这样可以满足不同的团队对代码评审的要求。如下图所示。提交规则包含四个方面：**提交**、**机器评审**、**人工评审**和**合入策略**。

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/8D/6F/CgqCHl_-uLOAa4m-AACj6zH9dAU169.png)

下面介绍一下跟代码评审相关的几个规则：

**1\. 提交**。

**提交代码必须经过评审（使用 git push origin HEAD:refs/for/\[分支名\]）**，选中该配置项即可开启 Change Request 的代码评审。当团队对提交代码的质量有严格要求时，需通过开启代码评审，进行代码检查，来控制提交代码的质量。

**2\. 机器评审**。

**开启 iPipe 流水线检查**，该配置项是用于当提交代码评审后，会自动触发 Change 流水线，对代码执行编译、单元测试等流水线检查。流水线检查通过后代码评审会+1。

**3\. 人工评审**。

**必须有评审人+2**，该配置项是用于保证人工评审的效果。这是 Gerrit 的规则，在代码评审中，任何人都可以查看任何人提交的代码，并给予不具有约束力的+1。只有一小部分人才能批准提交的代码并将其合并到代码库中，也就是必须要有一个评审人给予+2。

通过上面三个配置项，可以根据团队的需求灵活的设置代码评审的开启和关闭，以及代码合入的准则。

#### 评审人设置

评审人设置功能是为了在每次发起评审时，系统会自动将这里设置的评审人加入该评审单的评审人，不需要开发者手工添加。除了这里的评审人，其他人员还是可以评审的。如下图所示：

![Drawing 4.png](https://s0.lgstatic.com/i/image/M00/8D/64/Ciqc1F_-uNCABfccAABjbkLzGHw004.png)

该设置能够细化到按分支和目录设置评审人，如果一个代码库包含多个不同的功能模块，每个功能模块在不同的目录下时会非常有用。默认评审人的设置分为以下两种情况。

-   **所有分支**：如果“分支”列是所有分支，也就是选择\*时，“文件”列需要留空，表示所有分支的所有文件。
    
-   **特定分支**：如果“分支”列选择某个特定分支，如 master，“文件”列可以输入特定路径，需要以^开头，以/.\*结尾。
    

### 代码评审示例

下面，我通过一个示例演示代码评审的整个过程。该演示过程是在百度效率云的 SaaS 版本中进行的。分为**代码提交**、**代码评审**和**代码合入**几个步骤。

#### 代码提交

假设本次提交的内容是“修改版本号”，在 build.gradle 文件中将 version = '0.0.1-SNAPSHOT' 修改为 version = '1.0.1-SNAPSHOT'。提交代码命令如下。

```
D:\code\devops>git add .
warning: LF will be replaced by CRLF in build.gradle.
The file will have its original line endings in your working directory

D:\code\devops>git commit -m "update version"
[master 4bef3f8] update version
1 file changed, 1 insertion(+), 1 deletion(-)

D:\code\devops>git push origin HEAD:refs/for/master
Password for 'https://xinglong_devopser@xly.bce.baidu.com':
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2)
remote:
remote: Processing changes: new: 1, refs: 1, done
remote:
remote: New Changes:
remote:   http:
remote:
To https:
 * [new branch]      HEAD -> refs/for/master

D:\code\github\devops>

```

注意，在提交到远程仓库时使用的是 git push origin HEAD:refs/for/master 命令。从提交日志中可以看到两个重要信息：  
生成了一个新的Change，路径[http://xly.bce.baidu.com/icode/myreview/changes/71999](http://xly.bce.baidu.com/icode/myreview/changes/71999)，该URL为此次评审详情页面的地址；

此次提交是 push 到了 refs/for/master 这个临时分支，并不是直接 push 到 master 分支。

#### 代码评审

打开上面的地址，可以看到代码评审的详情页面，评审人就是在该评审页面中进行 Code Review。如下图所示：

![Drawing 5.png](https://s0.lgstatic.com/i/image2/M01/05/4B/Cip5yF_-uOKAPlizAAD9LtI2USw910.png)

从这个评审页面中，可以看到几个关键的信息，用于辅助评审人进行 Code Review。

-   基本信息：代码提交的时间、提交人，message，CommitId 以及要合入的分支。
    
-   代码变更：本次变更的文件以及每个文件变更的内容，可以在变更对比视图中清晰地展示出来，本次变更只修改了版本号。评审人可以在变更内容的位置添加行间评论，发表对此次变更的意见。
    
-   代码评审：本地提交需要人工评审 +2 后才能合入。目前已经有评审人 +2，并且状态显示为自动化任务检查通过，人工评审通过，可以合入。
    

#### 代码合入

此次变更已经通过负责人的 Code Review，便可以点击“合入”按钮合入目标分支中。合入后，评审的状态显示为“已合入”。在 master 分支的提交历史中也会显示此次变更。

![Drawing 6.png](https://s0.lgstatic.com/i/image2/M01/05/4C/CgpVE1_-uOiAc8qjAABTf3GzVDw511.png)

这是百度效率云基于 Change Request 模型的代码评审过程。该模型可以有效管控提交代码的质量，因为每次提交都会生成一个 Code Review，如果代码变更存在问题，那么此次变更就不会合入代码库中，根本不会影响到代码库中的代码。

### 总结

本课时介绍了百度效率云如何进行代码审查。百度效率云的 iCode 采用 Change Request 开发协作模型，能够在 git push 的时候进行代码评审，从而控制了提交入库的代码的质量。基于 Change Request 模型，iCode 平台提高了相应的提交规则设置，团队可以根据自己的需求开启和关闭代码评审。最后，通过一个实际操作演示了从代码提交、代码评审到代码合入的整个过程。百度效率云的 SaaS 服务已经部署在百度云上，感兴趣的同学可以试用一下。

到此，第二部分 DevOps 软件开发的最佳实践就讲完了。希望这部分能够给你带来收获，也希望学习会这些课时后，在谈论 DevOps 的时候，你能够从软件开发全生命周期的角度去思考，对 DevOps 的理解有一个全局的视角。
