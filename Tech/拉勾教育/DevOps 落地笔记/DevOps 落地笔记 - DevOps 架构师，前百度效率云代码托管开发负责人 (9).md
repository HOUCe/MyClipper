---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743
author: 
---

# [DevOps 落地笔记 - DevOps 架构师，前百度效率云代码托管开发负责人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743)


上一讲主要介绍技术债务的产生和管理，通过避免产生、提前发现、不断偿还等步骤将技术债务对软件质量的影响降到最低。

在软件开发过程中，当我们开发完成后，就会将软件进行编译打包发布到不同的环境。一般情况下，企业里的环境会分为**开发环境**、**测试环境**和**生产环境**。测试环境又分为 SIT 测试环境、UAT 测试环境和性能测试环境等。每一套环境都有相对应的配置信息。最简单的办法是把配置信息打包到每个环境的部署包里，但这样每次都需要构建多个环境的部署包，导致编译打包时间较长且部署包体积较大，并且不能保证每个环境测试的软件的唯一性。

那么有没有更好的办法呢？这就是今天要跟你介绍的内容——配置管理，在**每个环境使用相同的部署包，通过配置项来管理每个环境的差异**，既不会降低编译和传输速度，也能确保最终发布的软件就是经过测试过的软件。

### 什么是配置管理

配置管理最初是指版本控制，发展到现在已经超出了版本控制的范畴。在《持续交付—发布可靠软件的系统方法》一书是这样对配置管理进行定义的：

> 配置管理是指一个过程，通过该过程，所有与项目相关的产物，以及它们之间的关系都被唯一定义、修改、存储和检索。

我们都知道，一个应用程序是由软件代码、运行数据以及配置信息共同组成。在 《12 因素应用》（The Twelve-Factor App）中提到，应用程序的配置在不同部署环境（开发环境、测试环境、生产环境等）之间会有很大的差异。比如：数据库连接地址、缓存连接地址、第三方证书等。因此，需要将代码和配置分离，通过配置文件屏蔽各个部署环境的差异。

12 因素应用中也推荐将应用的配置存储于环境变量中。环境变量可以非常方便地在不同的部署间做修改，却不用动一行代码。因此，这里的配置主要是指应用程序的配置，**配置管理主要是指如何存储不同环境的应用程序配置**，以保证各个环境使用的都是同一份代码。

### 配置信息的描述和存储

下面来介绍一下配置信息是如何描述和存储的。

#### 配置信息的描述

通常情况下，配置项以键值对的形式来表示，比如 spring.application.name=service-a，这代表一个配置项。应用程序使用配置文件来存储多个配置项，通过层级来组织配置项，特别是当以 yaml 格式展现时，层级会更加清晰，这也是目前采用最多的展现形式。除此之外还有 properties、xml 等形式。

#### 配置信息的存储

配置信息比较常见的存储形式有数据库、版本控制库、文件目录、环境变量等。下面介绍一下每种形式的优缺点。

-   **数据库**：优点是可以充分利用数据库的检索功能，能够按不同的条件查询配置项，可以维护多个环境的配置。缺点是需要对配置项进行建模，开发单独的配置管理系统用于管理数据库中的配置项。另外，配置项的版本、版本的回退都需要单独维护。
    
-   **版本控制库**：优点是可以充分利用版本控制库本身的特性，对配置项的变更进行版本控制和变更追溯，非常容易地获取任意时刻的版本以及版本的回退，并且不需要开发额外的系统。此外，它也能管理多个环境的配置。缺点是不支持单个配置项的查询功能，每次更新都需要全量更新。
    
-   **文件目录**：优点是直接从本地获取，不依赖于其他系统。缺点是不能有效地进行版本控制，每个环境都是单独的文件目录，不能有效管理和控制多环境下的配置信息。
    
-   **环境变量**：优点是方便设置与读取，能够更好地与脚本集成，与每个环境绑定。缺点是不能有效地进行版本控制。
    

以上几种存储形式中，如果在脚本中需要一些**全局配置**可以使用**环境变量**，如果需要对配置项进行复杂的检索和版本控制，需要使用**数据库**和**版本控制库存储**。

#### 配置管理的时机

在应用程序生命周期中，不少阶段可以对应用程序进行配置，比如构建、部署、启动、运行和发布阶段。下面介绍下这几个阶段的是如何进行配置的。

-   **构建阶段**：在构建时，可以将配置文件直接添加到生成的二进制文件中。这种方式由于二进制文件与配置文件捆绑在一起，每个环境都需要生成一个单独的二进制文件，违反了12 因素应用中的原则，所以不推荐使用该方法。
    
-   **部署阶段**：在安装应用程序时，部署脚本或安装程序获取必需的配置信息。这种方式可以保证在二进制文件是同一个，遵循了 12 因素应用中将代码和配置分离的原则。
    
-   **启动阶段**：在应用程序启动时，将配置文件加载进来。该方式需要保证影响应用程序启动的配置信息能够获取，否则程序无法启动。
    
-   **运行时阶段**：在应用程序运行时，动态的变更配置文件。该方式主要用于在不需要停止服务的情况下变更配置信息。
    
-   **发布阶段**：是指在应用程序真正发布上线的时候，将配置文件改为生产环境的版本。该方式只是用于生产环境发布，由于此时发布的软件和之前测试的是同一个，因此测试通过是具备发布上线条件的。
    

从上面可以看出，如果想实现一包到底的目标，是可以在部署阶段和启动阶段来获得环境专属的配置信息。

### 配置管理的实现方式

配置管理目前常见的实现方式：

-   有 Spring Boot 的 Profile 形式；
    
-   基于 Git 的配置管理；
    
-   配置管理系统如携程的 Apollo；
    
-   配置管理数据库 CMDB。
    

这里主要介绍一下 Spring Boot 的 Profile 和基于 Git 的版本控制形式。这两种实现方式都是基于现有的框架和工具，实现和维护都比较简单。根据命名规范，后续代码中统一采用：**dev、test、prod代表开发环境**、**测试环境和生产环境**。

#### Spring Boot 的配置管理

前面提到，当我们将应用程序部署到不同的环境时，通常每个环境的配置是不一样的。Spring 框架自 3.1 版本就引入了对 profile 的支持。**Profiles 是一种基于条件的配置，哪个 profile 被激活就使用哪个配置**。

在 Spring 中有两种方式表示配置信息。

-   **配置类：**
    

使用 @Configuration 注解，该类会在运行时生成一个或多个能够被 Spring 容器处理的 @Bean 对象。可以在配置类中添加 @Profile 注解标识属于哪个环境的配置，如下面代码所示。

开发环境如下，

```
@Profile("dev")
@Configuration
@EnableWebSecurity
public class SecurityConfigDev extends WebSecurityConfigurerAdapter {
...
}
```

测试环境：

```
@Profile("test")
@Configuration
@EnableWebSecurity
public class SecurityConfigDev extends WebSecurityConfigurerAdapter {
...
}
```

生产环境如下。

```
@Profile("prod")
@Configuration
@EnableWebSecurity
public class SecurityConfigPro extends WebSecurityConfigurerAdapter {
...
}
```

-   **配置文件：**
    

配置文件是以 properties、yaml 为结尾的文件，一般会包含多个 application-${profile}.properties 的格式文件。比如以下代码。

```
#开发环境
application-dev.properties
#测试环境
application-test.properties
#生产环境
application-prod.properties
```

当某个 profile 被激活时，相对应的配置就会生效，否则会被忽略。激活 profile 的方式有以下几种：

**1\. 配置文件激活**，是指直接在 application.yaml 配置文件中激活特定的 profile。

```
spring.profiles.active=prod
```

**2\. 构建时激活**，是指在构建软件时指定要激活的 profile。以Maven构建为例，首先在 pom.xml 中定义 profile 配置：

```
<profiles>
<profile>
<id>dev</id>
<properties>
<activatedProperties>dev</activatedProperties>
</properties>
<activation>
<activeByDefault>true</activeByDefault>
</activation>
</profile>
<profile>
<id>test</id>
<properties>
<activatedProperties>test</activatedProperties>
</properties>
</profile>
<profile>
<id>prod</id>
<properties>
<activatedProperties>prod</activatedProperties>
</properties>
</profile>
</profiles>
```

请注意 `<activeByDefault>`true`</activeByDefault>` 标签，该标签意味着在构建时如果未指定要激活哪个 profile，将使用 dev 作为默认配置文件。  
将 Maven 与 Spring Boot 的配置文件结合使用，可以在构建时指定某个 profile 的配置文件。在 application.yaml 里添加一个参数 spring.profiles.active=@activatedProperties@，该参数可以告诉 Spring 到底使用哪个 profile。

在使用 Maven 构建时通过 -P 参数指定使用哪个 profile。如下构建命令会用 prod 值替换上面指定的参数，从而实现激活 prod 配置文件的目的。

```
$ mvn -Pprod clean package
```

**3\. 运行时激活**，是指运行应用程序时通过传参的方式指定需要激活的 profile。参考如下命令，其中 -D 指定的参数放置于 jar 之前，-- 指定的参数放置于 jar 之后。

```
$ java –jar -Dspring.profiles.active=prod app.jar
或者
$ java –jar  app.jar --spring.profiles.active=prod
```

上面三种激活 profile 的方式，后面的可以覆盖前面的。相比较而言，第三种方式更加灵活，能实现同一份代码在不同环境中使用的目的。目前在实际开发过程中，也多采用的第三种方式。

#### 基于 Git 的配置管理

的基于 Git 的配置管理是指使用 Git 作为后端服务来存储配置信息的方式。对于每个部署环境，如开发环境（dev）、测试环境（test）、生产环境（prod）等，都有一个对应的 Git 分支。每个服务都有自己独立的配置仓库，环境与分支之间的对应关系是一对一。这样在获取该服务在特定环境中的配置时，只需要克隆该代码库，并切换到对应的分支即可。如果想对获得的配置信息进行更复杂的逻辑处理，比如以 json 格式输出或者输出单个配置项的值，可以在 Git 库与环境之间增加单独的服务——配置管理服务，来满足个性化要求。注意：该服务不是必须得。

通常情况下，Git 库中的分支不会合并。这样就可以更改分支中的配置信息，并且应用到特定的环境中。同时，基于 Git 本身的版本控制特性，可以非常简单的实现变更追溯：谁？在什么时间？进行了哪些变更？基于 Git 的配置服务体系结构示意图如下：

![图片1.png](https://s0.lgstatic.com/i/image2/M01/03/AD/CgpVE1_giFqALv23AAJEdwpR6Po811.png)  
具体的操作过程如下面步骤。

**Step1：新建配置库并存储配置文件。**

假设我们有一个名称为“**服务A**”的应用程序，需要运行在开发、测试和生产三个环境中。因此，需要在 Git 仓库中新建一个名为**config-data-service-a**的配置库，并且新建**dev**、**test**和**prod**三个分支，分别存储三个环境的配置\*\*。\*\*

下面以 application.properties 配置文件为例，在 dev 分支中，该配置文件的内容是：

```
spring.application.name=service-a
spring.datasource.name=dev
spring.datasource.url=jdbc:mysql:
spring.datasource.username=devops
spring.datasource.password=123456
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
```

在 test 分支中，该配置文件的内容如下所示。

```
spring.application.name=service-a
spring.datasource.name=test
spring.datasource.url=jdbc:mysql:
spring.datasource.username=devops
spring.datasource.password=123456
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
```

在 prod 分支中，该配置文件的内容是：

```
spring.application.name=service-a
spring.datasource.name=prod
spring.datasource.url=jdbc:mysql:
spring.datasource.username=devops
spring.datasource.password=123456
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
```

这三个分支中不同的内容为 spring.datasource.name 和 spring.datasource.url 的值。

**Step2**：获取配置文件并启动服务。

在配置管理服务中，可以按服务名称、环境获取配置文件。比如下面的接口。用于获取特定服务、特定环境下的配置文件，默认是 application.properties。

还是以“服务A”为例，serviceName为service-a。获取 dev 环境的配置接口是：

获取 test 环境的配置接口如下所示。

提供这样的接口之后，在部署脚本里只需要通过不同的参数来识别不同的环境即可，在与CICD集成时也会更加方便。增加配置管理服务的好处是将屏蔽了处理配置文件的复杂性，比如克隆代码库，解析文件，转换内容等。部署脚本与配置管理服务之间只需通过接口交互。下面是一段shell脚本的示例

```
#!/bin/bash
#通过参数获取服务名和环境信息
SERVICE_NAME=$1
ENV=$2
#调用接口获取配置文件并写到特定路径下
echo $(http://config-service/api/v1/services/${SERVICE_NAME}/envs/${ENV})>/usr/local/${SERVICE_NAME}/application.properties
#切换到服务目录下
cd /usr/local/${SERVICE_NAME}
#启动服务
sh start.sh
```

当配置文件需要变更时，就像提交代码一样，将变更后的配置文件提交到代码库中。再通过接口获取文件时，配置管理服务会检测配置文件是否发生变更？这时，当检测到发生变更后，会重新克隆最新的 Git 仓库，返回给调用方。基于 Git 存储配置信息的实践也有很多，它也是目前 GitOps 理念的基本做法。  
其他的配置管理的工具还有**携程的 Apollo**。它是由携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端。并且，它具备规范的权限、流程治理等特性，适用于微服务配置管理的场景。目前很多企业都在使用。这个工具目前已经开源，地址是[https://github.com/ctripcorp/apollo](https://github.com/ctripcorp/apollo)，上面有比较详细的介绍文档，你可以自行查阅。

### 总结

本课时主要介绍了在开发过程中如何处理多环境配置的问题。介绍了配置信息的描述、存储以及配置管理的时机，最后介绍了两种常见的实现方式。一种是基于 Spring Boot Profile 的方式，能够在**应用程序启动时**指定特定环境的 profile，这种方式虽然并未实现代码与配置的隔离，但也能保证不同环境使用的是同一个软件。另外一种是基于 Git 的方式，能够在**应用程序部署时**提供特定环境的配置文件，实现了代码与配置的隔离，保证了代码的唯一性。配置管理问题是一个通用问题，每个企业都会涉及，如果你有更好的实践欢迎在评论区留言。
