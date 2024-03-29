---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743
author: 
---

# [DevOps 落地笔记 - DevOps 架构师，前百度效率云代码托管开发负责人 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=561#/detail/pc?id=5743)


上一课时介绍了通过搭建一套部署流水线，高效、可靠的将软件部署到测试环境以及生产环境。到目前为止，我们学习了从用户需求到软件部署到生产环境交付给用户的全过程。随着软件工程不断发展，近几年，出现了一种新的实践，这就是今天要介绍的内容——混沌工程，它通过在生产环境中对系统进行破坏，来不断增强软件的健壮性。

### 什么是混沌工程？

《混沌工程原理》中这样定义：“混沌工程（Chaos Engineering）是在分布式系统上进行实验的学科, 目的是建立对系统抵御生产环境中失控条件的能力以及信心。”简而言之，混沌工程就是“故意破坏事物”的特殊方法，通过在生产环境中捣乱。比如随机重启生产环境中的服务器等，以发现生产环境中可能出现的隐藏问题；通过不断修复系统的缺陷，从而使系统更健壮、更具容错能力。

这里强调的是混沌工程并不仅仅是“搞破坏”，因为搞破坏非常容易，但在搞完破坏后，能不能有效控制破坏的爆炸半径，能不能有效控制对用户造成的影响，以及判断该问题是否需要修复并寻找修复方法......这些才是混沌工程中最关键的。

混沌工程和传统测试有很多重叠的部分。混沌工程应该成为传统测试的补充，是经过传统测试后系统已经足够稳定，可以在生产环境中被任意“破坏”，来进一步增强系统的稳定性的工程。由于需要生产环境中的真实场景，这类测试是不能通过单元测试和集成测试来模拟的。混沌工程的核心思想是以可控的方式主动注入故障，以验证系统的行为是否符合我们的预期，并在不正常的情况下进行修复，以此提高系统的稳定性。

### 为什么要实施混沌工程？

创建可靠的软件是当今企业获取用户，赢得市场竞争的基础。特别是当我们的系统迁移到分布式架构，一些不可预知的问题时常发生。传统的测试只能保证软件的应用层的质量，无法保证应用程序以及各种服务或整个系统在**任何情况下**都能正常使用，不管是“正常情况”还是极端负载或异常情况。应用程序的任何异常都会影响用户体验。

混沌工程可以主动测试生产环境中各种压力下的行为。通过比较假设行为和实际行为，我们可以在系统出现故障之前发现问题并修复问题。混沌工程可以做以下几件事情：

1.  对软件和基础设施进行比传统形式更广泛的测试和验证；
    
2.  发现传统测试无法发现的问题；
    
3.  帮助团队了解系统在真实生产环境中的行为，服务如何被中断以及都有哪些Bug？
    

因此，混沌工程可以帮助我们增强系统的稳定性和可靠性，带来更好的用户体验。

### 如何实施混沌工程？

混沌工程也是近几年出现的一个新的工程实践，目前只是在少数大公司里实施，如 Google、Facebook、阿里巴巴等。那么，如何在企业里实施混沌工程？我们可以通过下面几个步骤来实施混沌工程。

#### 建立基线指标

在进行混沌工程实验之前，要先收集一组基线指标数据。这些指标包含基础设施的监控指标、告警指标、严重级别指标、应用程序指标等。下面介绍下这些指标的内容。

-   **基础设施的监控指标**：包含服务器的CPU 峰值、IO峰值、磁盘使用率、内存使用率，网络的延迟、数据丢包率、DNS 等指标。
    
-   **告警指标**：可以按服务统计每周的告警数量，处理告警的时间，以及每种服务每周最频繁的告警类型。
    
-   **严重级别指标**：可以按服务统计每周不同严重级别的事件数量，以及按服务统计每种严重级别的 MTTD（平均检测时间）、MTTR（平均故障恢复时间）和MTBF（平均故障间隔时间）。
    
-   **应用指标**：应用程序的可观察性指标，事件数量，请求的响应时间，数据库连接数，QPS（每秒查询数量），TPS（每秒事务数量）。
    

#### 模拟真实事件

在生产系统中模拟真实事件来进行实验，有两种方式：攻击和场景。

-   **攻击**：将故障注入系统中，如消耗计算资源、关闭系统、丢弃网络包等方法，攻击是单个的故障注入方式。
    
-   **场景**：是将一组攻击保存的集合。场景中的攻击按顺序执行，可以更好地控制攻击的执行方式，并可以模拟较为复杂的故障。保存下来的场景可以被重复执行，并能够观察系统随着时间的行为变化。
    

不管使用哪种方式，在执行完成后，需要记录上述指标的观察结果并与基线进行比较。

#### 分析结果

基于从实验中获得的结果数据与假设进行比较，并得出结论。这里有几个问题需要给出答案：

-   系统行为是否符合预期？
    
-   如果系统有监控告警等系统，是否按预期运行？
    
-   本次实验发现了哪些新问题？
    
-   告警系统多长时间检测到问题并发出通知？该时间是否可以接收？
    
-   实验结束后，系统是否自动恢复到正常状态？还是需要人工干预？
    

#### 重复实验

修复问题后，重复执行该实验以确保问题得到彻底解决。如果系统成功抵御了攻击，说明该问题已经被修复。此时，应该考虑增加攻击的程度，爆炸半径或者一次性攻击目标的系统数量。这对于测试集群系统、自动扩展系统或负载均衡系统比较有用。

#### 自动化实验

一旦系统能够抵御该攻击，就可以按照常规测试惯例定期执行攻击。可以将该实验的执行嵌入到 CI/CD 流水线中，这样有利于新的变更不会引起新的可靠性问题。下图显示了可以在软件生命周期中执行不同类型的混沌实验的各个阶段。只要有设计良好的混沌实验，就可以在每次执行流水线时都会执行这些混沌实验。这一步的目的是通过在生产之前或者在生产中引起问题之前发现实际问题。

![图片1.png](https://s0.lgstatic.com/i/image/M00/8D/38/Ciqc1F_8HESAE0MsAACQTIYnXtw429.png)

### 混沌工程案例

下面介绍一下如何将 Chaos Monkey 集成到 Spring Boot 应用程序中。

#### SpringBoot 集成 ChaosMonkey

Netflix 不仅制定了《混沌工程原理》，还提供了一个将理论付出实际的强大工具：**ChaosMonkey**。ChaosMonkey 是一种工具，该工具会随机终止生产环境中运行的虚拟机实例和容器，使工程师能够构建更加弹性的服务。Spring Boot 是目前构建 Java 后台应用程序最受欢迎的框架。Spring Boot Chaos Monkey 是一个依赖库，可以将混沌工程的实践集成到 Spring Boot 的应用中。只需要下面两步就可以将 Chaos Monkey 添加的应用程序中。

**STEP 1：在应用程序中添加 ChaosMonkey 的依赖包。**

```
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>chaos-monkey-spring-boot</artifactId>
    <version>2.2.0</version>
</dependency>
```

**STEP 2：在启动应用程序的时候，需要激活 chaos-monkey的profile 来初始化 ChaosMonkey。**

```
java -jar chaosmonkeyforspringboot.jar --spring.profiles.active=chaos-monkey
```

启动后，就会在控制台中打印出 Chaos Moneky 的字样。

![Drawing 1.png](https://s0.lgstatic.com/i/image/M00/8D/31/Ciqc1F_75aWAd6yFAACrg8d9bZs440.png)

#### ChaosMonkey 配置

Chaos Monkey 在引入后并未开启，需要通过 chaos.monkey.enabled 配置项来开启。Chaos Monkey 提供了四种不同的攻击方式：

-   延迟攻击；
    
-   异常攻击；
    
-   杀掉应用程序攻击；
    
-   内存攻击。
    

这种攻击的开启和关闭可以通过下面四个配置项决定，并且每种攻击方式也有相应的配置参数。比如，延迟攻击是在每个请求处理时添加随机的延迟时间，该值由 chaos.monkey.assaults.latency-range-start 和chaos.monkey.assaults.latency-range-end 两个参数的区间值来设置。

```
chaos.monkey.assaults.latency-active=true
chaos.monkey.assaults.exceptions-active=true
chaos.monkey.assaults.memory-active=true
chaos.monkey.assaults.kill-application-active=true
```

ChaosMonkey 的配置项清单可以通过 Spring Boot Actuator 的访问端口查看，首先需要通过下面两个配置项开启并将 chaosmonkey 添加到暴露的端口列表中。

```
management.endpoint.chaosmonkey.enabled=true
management.endpoints.web.exposure.include=health,info,chaosmonkey
```

在地址栏里输入 [http://localhost:8080/actuator/chaosmonkey](http://localhost:8080/actuator/chaosmonkey) 可以看到如下配置项清单：

```
{
"chaosMonkeyProperties": {
"enabled": true
},
"assaultProperties": {
"level": 5,
"latencyRangeStart": 1000,
"latencyRangeEnd": 2000,
"latencyActive": true,
"exceptionsActive": true,
"exception": {
"type": null,
"arguments": null
},
"killApplicationActive": true,
"memoryActive": true,
"memoryMillisecondsHoldFilledMemory": 90000,
"memoryMillisecondsWaitNextIncrease": 1000,
"memoryFillIncrementFraction": 0.15,
"memoryFillTargetFraction": 0.25,
"runtimeAssaultCronExpression": "OFF",
"watchedCustomServices": null
},
"watcherProperties": {
"controller": false,
"restController": false,
"service": true,
"repository": false,
"component": false
}
}
```

#### 测试示例项目

在 Chaos Monkey 的设置里开启 chaos.monkey.assaults.exceptions-active=true，添加一个测试的 Controller 类，如下：

```
@RestController
@RequestMapping("/v1/test/chaosmonkey")
public class OrderController {
@Autowired
private OrderMapper orderMapper;
@GetMapping("/orders")
public List<Order> getOrders() {
try {
return orderMapper.selectAll();
        } catch (Exception e) {
            e.printStackTrace();
return null;
        }
    }
}
```

当调用该接口时，会随机产生异常。下图是使用 postman 批量调用该接口产生的结果，可以看出该接口执行了 10 次，其中成功 8 次，失败 2 次。这 2 次失败就是因为 Chaos Monkey 导致的。

![Drawing 2.png](https://s0.lgstatic.com/i/image2/M01/05/17/Cip5yF_75daAfGMJAAE_HePNBUY303.png)

在服务的后台日志中也打印出来异常信息，如下图所示。从日志可以看出，该 RuntimeException 是由 Chaos Monkey 抛出的。

![Drawing 3.png](https://s0.lgstatic.com/i/image/M00/8D/3C/CgqCHl_75dyAUzvyAADtBHR7to8252.png)

混沌工程的落地离不开工具或平台，Spring Boot Chaos Monkey 是一个不错的开源项目，可以应用在企业内部的故障演练中，暴露服务本身以及服务与服务之间的调用问题，提升系统的健壮性。

### 总结

本课时主要介绍了如何使用混沌工程的实践来进一步提高服务的稳定性和健壮性。混沌实验可以在软件开发生命周期的多个阶段进行开展，尽可能在部署到生产环境之前做尽可能多的测试，减少部署到生产环境中出现问题的风险。当有些测试场景无法在测试环境中模拟时，需要在生产环境中进行实验，此时对应用程序来说也是最大的挑战。在生产环境中进行混沌实验时，务必要进行充分的设计和回滚方案的制定，以及对故障产生的影响范围的把控，以为真的对业务系统造成破坏。

到此，软件开发的全生命周期中涉及的最佳实践就讲完了，不知道你对 DevOps 是否有一个全局视角？DevOps 涉及的内容很多，这几讲帮助你梳理一下 DevOps 在软件开发全生命周期中的脉络，具体的细节还是要在具体的实践中去把握。在后面的课时中，我们的主要内容将放在介绍 DevOps 中度量相关的内容方面，敬请期待。
