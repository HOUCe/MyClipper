---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=81#/detail/pc?id=2035
author: 
---

# [42讲轻松通关 Flink - 资深大数据工程师 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=81#/detail/pc?id=2035)


我们在上一课时中讲了 CEP 的基本原理并且用官网的案例介绍了 CEP 的简单应用。在 Flink CEP 中存在多个比较晦涩的概念，如果你对于这些概念理解有困难，我们可以把：**创建系列 Pattern，然后利用 NFACompiler 将 Pattern 进行拆分并且创建出 NFA，NFA 包含了 Pattern 中的各个状态和各个状态间转换的表达式**。这整个过程我们可以把 Flink CEP 的使用类比为正则表达式的使用。CEP 中定义的 Pattern 就是正则表达式，而 DataStream 是需要进行匹配的原数据，Flink CEP 通过 DataStream 和 Pattern 进行匹配，然后生成一个经过正则过滤后的 DataStream。

### Flink CEP 源码解析

Flink CEP 被设计成 Flink 中的算子，而不是单独的引擎。那么当一条数据到来时，Flink CEP 是如何工作的呢？

#### Flink DataStream 和 PatternStream

我们还是用官网中的案例来进行讲解：

```
DataStream<Event> input = ... 
Pattern<Event, ?> pattern = Pattern.<Event>begin("start").where( 
new SimpleCondition<Event>() { 
@Override 
public boolean filter(Event event) { 
return event.getId() == 42; 
            } 
        } 
    ).next("middle").subtype(SubEvent.class).where( 
new SimpleCondition<SubEvent>() { 
@Override 
public boolean filter(SubEvent subEvent) { 
return subEvent.getVolume() >= 10.0; 
            } 
        } 
    ).followedBy("end").where( 
new SimpleCondition<Event>() { 
@Override 
public boolean filter(Event event) { 
return event.getName().equals("end"); 
            } 
         } 
    ); 
PatternStream<Event> patternStream = CEP.pattern(input, pattern); 
DataStream<Alert> result = patternStream.process( 
new PatternProcessFunction<Event, Alert>() { 
@Override 
public void processMatch( 
                Map<String, List<Event>> pattern, 
                Context ctx, 
                Collector<Alert> out) throws Exception { 
            out.collect(createAlertFrom(pattern)); 
        } 
```

我们知道，Flink 中的 DataStream 由相同类型的事件或者元素构成，可以经过复杂的算子比如 Map、Filter 等进行转换。

那么 CEP 是如何和 DataStream 无缝衔接的呢？我们注意到 CEP 的 pattern 方法：

```
public class CEP { 
public CEP() { 
    } 
public static <T> PatternStream<T> pattern(DataStream<T> input, Pattern<T, ?> pattern) { 
return new PatternStream(input, pattern); 
    } 
public static <T> PatternStream<T> pattern(DataStream<T> input, Pattern<T, ?> pattern, EventComparator<T> comparator) { 
        PatternStream<T> stream = new PatternStream(input, pattern); 
return stream.withComparator(comparator); 
    } 
} 
```

PatternStream 是 Flink CEP 对模式匹配后流的抽象和定义，它把 DataStream 和 Pattern 组合到一起，并且基于 PatternStream 提供了一系列的方法，比如 select、process 等。

我们在 PatternStream 上调用 select 或者 process 方法时，会继续调用到下面的方法：

```
@Internal 
final class PatternStreamBuilder<IN> { 
   ... 
    <OUT, K> SingleOutputStreamOperator<OUT> build(TypeInformation<OUT> outTypeInfo, PatternProcessFunction<IN, OUT> processFunction) { 
        Preconditions.checkNotNull(outTypeInfo); 
        Preconditions.checkNotNull(processFunction); 
        TypeSerializer<IN> inputSerializer = this.inputStream.getType().createSerializer(this.inputStream.getExecutionConfig()); 
boolean isProcessingTime = this.inputStream.getExecutionEnvironment().getStreamTimeCharacteristic() == TimeCharacteristic.ProcessingTime; 
boolean timeoutHandling = processFunction instanceof TimedOutPartialMatchHandler; 
        NFAFactory<IN> nfaFactory = NFACompiler.compileFactory(this.pattern, timeoutHandling); 
        CepOperator<IN, K, OUT> operator = new CepOperator(inputSerializer, isProcessingTime, nfaFactory, this.comparator, this.pattern.getAfterMatchSkipStrategy(), processFunction, this.lateDataOutputTag); 
        SingleOutputStreamOperator patternStream; 
if (this.inputStream instanceof KeyedStream) { 
            KeyedStream<IN, K> keyedStream = (KeyedStream)this.inputStream; 
            patternStream = keyedStream.transform("CepOperator", outTypeInfo, operator); 
        } else { 
            KeySelector<IN, Byte> keySelector = new NullByteKeySelector(); 
            patternStream = this.inputStream.keyBy(keySelector).transform("GlobalCepOperator", outTypeInfo, operator).forceNonParallel(); 
        } 
return patternStream; 
    } 
    ... 
} 
```

这其中 NFACompiler.compileFactory，会帮我们完成 Pattern 到 State 的转换。

```
public static <T> NFACompiler.NFAFactory<T> compileFactory(Pattern<T, ?> pattern, boolean timeoutHandling) { 
if (pattern == null) { 
return new NFACompiler.NFAFactoryImpl(0L, Collections.emptyList(), timeoutHandling); 
    } else { 
        NFACompiler.NFAFactoryCompiler<T> nfaFactoryCompiler = new NFACompiler.NFAFactoryCompiler(pattern); 
        nfaFactoryCompiler.compileFactory(); 
return new NFACompiler.NFAFactoryImpl(nfaFactoryCompiler.getWindowTime(), nfaFactoryCompiler.getStates(), timeoutHandling); 
    } 
} 
```

其中，compileFactory 方法会生成 State，也就是说把 Pattern 转化为 NFA 中的状态信息，状态会不断地向后追加，所以需要分别先后创建 EndState、MiddleState 和 StartState。这里 Pattern 中专门定义了一个 getPrevious 方法用来获取前一个状态。

```
void compileFactory() { 
if (this.currentPattern.getQuantifier().getConsumingStrategy() == ConsumingStrategy.NOT_FOLLOW) { 
throw new MalformedPatternException("NotFollowedBy is not supported as a last part of a Pattern!"); 
    } else { 
this.checkPatternNameUniqueness(); 
this.checkPatternSkipStrategy(); 
        State<T> sinkState = this.createEndingState(); 
        sinkState = this.createMiddleStates(sinkState); 
this.createStartState(sinkState); 
    } 
} 
```

#### Event 事件处理

那么，当一条消息进入 Flink CEP 中，是如何被处理的呢？

当 Flink 的事件属性为 EventTime 时，关键代码如下：

```
public void processElement(StreamRecord<IN> element) throws Exception { 
long currentTime; 
if (this.isProcessingTime) { 
if (this.comparator == null) { 
            NFAState nfaState = this.getNFAState(); 
long timestamp = this.getProcessingTimeService().getCurrentProcessingTime(); 
this.advanceTime(nfaState, timestamp); 
this.processEvent(nfaState, element.getValue(), timestamp); 
this.updateNFA(nfaState); 
        } else { 
            currentTime = this.timerService.currentProcessingTime(); 
this.bufferEvent(element.getValue(), currentTime); 
this.timerService.registerProcessingTimeTimer(VoidNamespace.INSTANCE, currentTime + 1L); 
        } 
    } else { 
        currentTime = element.getTimestamp(); 
        IN value = element.getValue(); 
if (currentTime > this.lastWatermark) { 
this.saveRegisterWatermarkTimer(); 
this.bufferEvent(value, currentTime); 
        } else if (this.lateDataOutputTag != null) { 
this.output.collect(this.lateDataOutputTag, element); 
        } 
    } 
} 
```

当 EventTime 大于上一次的 Watermark 时，会把当前的数据加入 elementQueueState 队列中，不符合条件的数据会直接丢弃，关键代码如下：

```
currentTime = element.getTimestamp(); 
IN value = element.getValue(); 
if (currentTime > this.lastWatermark) { 
this.saveRegisterWatermarkTimer(); 
this.bufferEvent(value, currentTime); 
} else if (this.lateDataOutputTag != null) { 
this.output.collect(this.lateDataOutputTag, element); 
} 
```

满足条件的数据加入队列后，会在 onEventTime 方法中判断是否触发计算：

```
public void onEventTime(InternalTimer<KEY, VoidNamespace> timer) throws Exception { 
    PriorityQueue<Long> sortedTimestamps = this.getSortedTimestamps(); 
    NFAState nfaState; 
long timestamp; 
for(nfaState = this.getNFAState(); !sortedTimestamps.isEmpty() && (Long)sortedTimestamps.peek() <= this.timerService.currentWatermark(); this.elementQueueState.remove(timestamp)) { 
        timestamp = (Long)sortedTimestamps.poll(); 
this.advanceTime(nfaState, timestamp); 
        Stream<IN> elements = this.sort((Collection)this.elementQueueState.get(timestamp)); 
        Throwable var7 = null; 
try { 
            elements.forEachOrdered((event) -> { 
try { 
this.processEvent(nfaState, event, timestamp); 
                } catch (Exception var6) { 
throw new RuntimeException(var6); 
                } 
            }); 
        } catch (Throwable var16) { 
            var7 = var16; 
throw var16; 
        } finally { 
if (elements != null) { 
if (var7 != null) { 
try { 
                        elements.close(); 
                    } catch (Throwable var15) { 
                        var7.addSuppressed(var15); 
                    } 
                } else { 
                    elements.close(); 
                } 
            } 
        } 
    } 
this.advanceTime(nfaState, this.timerService.currentWatermark()); 
this.updateNFA(nfaState); 
if (!sortedTimestamps.isEmpty() || !this.partialMatches.isEmpty()) { 
this.saveRegisterWatermarkTimer(); 
    } 
this.updateLastSeenWatermark(this.timerService.currentWatermark()); 
} 
```

到此为止，我们可以看到一条数据进入 Flink CEP 中处理的逻辑大概可以分为以下几个步骤：

-   DataSource 中的数据转换为 DataStream；
    
-   定义 Pattern，并将 DataStream 和 Pattern 组合转换为 PatternStream；
    
-   PatternStream 经过 select、process 等算子转换为 DataStraem；
    
-   再次转换的 DataStream 经过处理后，sink 到目标库。
    

### 自定义消息事件

我们在后面的案例中会分别举几个不同的场景，那么我们需要定义几个不同的消息源。

-   第一个场景，连续登录场景
    

在这个场景中，我们模拟生产环境中可能出现的“爆破登录”现象，模拟用户的登录请求信息：

```
public class LogInEvent { 
private Long userId; 
private String isSuccess; 
private Long timeStamp; 
public Long getUserId() { 
return userId; 
    } 
public void setUserId(Long userId) { 
this.userId = userId; 
    } 
public String getIsSuccess() { 
return isSuccess; 
    } 
public void setIsSuccess(String isSuccess) { 
this.isSuccess = isSuccess; 
    } 
public Long getTimeStamp() { 
return timeStamp; 
    } 
public void setTimeStamp(Long timeStamp) { 
this.timeStamp = timeStamp; 
    } 
} 
```

其中 userId 为用户的 ID，isSuccess 表示用户本次登录是否成功，timeStamp 表示用户登录时间戳。

-   第二个场景，超时未支付
    

在这个场景中，我们要检测出那些用户下单后 5 分钟还没有支付的信息：

```
public class PayEvent { 
private Long userId; 
private String action; 
private Long timeStamp; 
public Long getUserId() { 
return userId; 
    } 
public void setUserId(Long userId) { 
this.userId = userId; 
    } 
public String getAction() { 
return action; 
    } 
public void setAction(String action) { 
this.action = action; 
    } 
public Long getTimeStamp() { 
return timeStamp; 
    } 
public void setTimeStamp(Long timeStamp) { 
this.timeStamp = timeStamp; 
    } 
} 
```

同样的，其中 userId 为用户的 ID，action 表示用户的操作事件枚举比如下单、支付等，timeStamp 表示用户操作的时间戳。

-   高频交易，找出活跃账户
    

在这个场景中，我们模拟账户交易信息中，那些高频的转账支付信息，希望能发现其中的风险或者活跃的用户：

```
public class TransactionEvent { 
private String accout; 
private Double amount; 
private Long timeStamp; 
public String getAccout() { 
return accout; 
    } 
public void setAccout(String accout) { 
this.accout = accout; 
    } 
public Double getAmount() { 
return amount; 
    } 
public void setAmount(Double amount) { 
this.amount = amount; 
    } 
public Long getTimeStamp() { 
return timeStamp; 
    } 
public void setTimeStamp(Long timeStamp) { 
this.timeStamp = timeStamp; 
    } 
} 
```

其中 account 表是账户信息，amount 为转账金额，timeStamp 是交易时的时间戳信息。

### 总结

本节课我们详细讲解了 Flink CEP 的源码实现，逐步讲解了一条数据进入 Flink CEP 中处理的逻辑步骤，你可以根据需要进一步查看实现原理。最后我们模拟了 3 种实际生产环境中的场景，定义了消息事件，为我们后面的课程做准备。
