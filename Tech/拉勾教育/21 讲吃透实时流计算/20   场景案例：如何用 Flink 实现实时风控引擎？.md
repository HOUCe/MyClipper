---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=614#/detail/pc?id=6417
author: 
---

# [21 讲吃透实时流计算 - 系统架构师 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=614#/detail/pc?id=6417)


在前面的课时中，我们讨论了实时流计算系统的核心概念和四种流计算框架。从今天起，我们将把其中部分知识点整合起来，以展示两个完整的实时流计算应用案例。从而帮助你更好地理解这些概念以及如何将流计算框架运用到具体业务中。

今天我们先来看第一个案例，也就是使用 Flink 实现一个包括**特征提取**和**风险评分**功能的风控引擎。

### 业务场景

考虑这么一种场景。有一天 Bob 窃取了 Alice 的手机银行账号和密码等信息，并准备将 Alice 手机银行上的钱，全部转移到他提前准备好的“骡子账户”（mule account，帮助转移资产的中转账户）上去。但是手机银行出于安全的原因，每次只允许最多转账 1000 元。于是 Bob 就准备每次转账 1000 元，分多次将 Alice 的钱转完。

那作为风控系统，该怎样及时检测并阻止这种异常交易呢？我们需要针对这种异常交易行为构建一个用于风险评分的规则或模型。不管是规则还是模型，它们的输入都是一些**特征**，所以我们必须先设定要提取的特征。

经过初步思考，我们想到了**如下 3 个有利于异常交易行为检测的特征**。

-   一是，过去一小时支付账户交易次数。
    
-   二是，过去一小时接收账户接收的总金额。
    
-   三是，过去一小时交易的不同接收账户数。
    

现在，我们已经确定了需要提取的特征，那接下来就是设定用于风险评分的规则或模型，用于判定交易行为是否异常。假设我们决定使用规则系统来判定交易是否异常，当输入的特征满足以下条件时，即判定交易是异常的。

-   一是，在过去一小时支付账户交易次数超过 5 次。
    
-   二是，在过去一小时接收账户接收的总金额超过 5000。
    
-   三是，在过去一小时交易的不同接收账户数不超过 2。
    

至此，风控系统要提取的特征，以及用于判定交易行为异常的规则都已经确定了。接下来，就是具体实现这个风控系统了。

### 实现原理

我们使用 Flink 来实现风控引擎。为了集中讨论流计算处理部分，我们这里假定从客户端上报的交易事件，已经被数据采集服务器接收后写入 Kafka 了。我们接下来只需要用 Flink 从 Kafka 读取交易事件作为输入流即可。

整体而言，我们要实现的风控引擎分成了两部分，即**特征提取部分**和**风险评分部分**。

在**特征提取部分**，我们在前面确定要提取的特征有 3 个，不过这只是为了简化讲解，在实际业务场景下可以是更多的特征，比如数十个特征。如果每计算一个特征要 50ms，那么串行执行的话，计算 3 个特征就是 150ms，计算 30 个特征就是 1500ms，这样就不太好了。因为计算时延明显增加，业务变得不那么实时，用户体验就变差了。

所以，为了减少特征计算的整体耗时，我们必须**并行计算各个特征**。

那具体怎样并行计算呢？这个时候就需要用到 Flink 中的 KeyedStream 了。Flink 中的 KeyedStream 非常贴心地提供了**将流进行逻辑分区**的功能。使用 KeyedStream，我们能够**将事件流分成多个独立的流从而实现并行计算，这正好满足了我们对特征进行并行计算的需求**。

但问题随之而来，我们应该怎样选择对流进行分区的主键呢？或许你会觉得随机分配就好了，但这是不行的！**因为特征计算的过程中，会涉及流信息状态的读写，如果特征被不受控制地随机分配到 Flink 的各个节点上去，那么就不能保证读取到与该特征相关的完整流信息状态，也就是特征计算所需使用字段的历史业务数据**。所以，必须是**按照特征使用到的字段**对流进行分区。

我们用下面图 1 展示的 Flink 实现风控系统的原理图进行详细讲解。

![Drawing 1.png](https://s0.lgstatic.com/i/image6/M01/27/AD/Cgp9HWBdg3KAe4vDAAMMdbPVA5A512.png)

在上面的图 1 中，当接收到交易事件时，我们给该交易事件分配一个随机生成的事件 ID，也就是图中的 event\_id。这个事件 ID 在之后会帮我们将分散的并行计算特征结果合并起来。

当分配好事件 ID 后，我们立刻通过 flatMap 操作（[第 09 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=614&sid=20-h5Url-0&buyFrom=2&pageId=1pz4#/detail/pc?id=6426&fileGuid=xxQTRXtVcqtHK6j8)已经讲解过 flatMap），将事件分解（split）成多个“事件分身”，每个“事件分身”都包含了事件的**全部或部分字段**。并且，每个“事件分身”还需要包含\*\*分区键（key）\*\*信息相关字段，用于指示该“事件分身”应该划分到哪个分区流 KeyedStream 中。

比如，图 1 中我们用“count(pay\_account.history,1h)”这种**自定义的 DSL**（Domain Specific Language，领域专用语言）来表示“过去一小时支付账户交易次数”这个特征，那么这个特征计算时需要用到的“事件分身”就是由 event\_id、pay\_account、timestamp(事件发生时间) 和**分区键名**、**分区键值**这五个字段组成的 JSON 对象，其中**分区键名**是 "pay\_account.history"，**分区键值**是 “pay\_account字段名#pay\_account字段值.history”，代表了支付账户的历史事件。

再比如，图 1 中我们用“sum(amount#rcv\_account.history,1h)”来表示“过去一小时接收账户接收的总金额”这个特征，那么这个特征计算时需要用到的“事件分身”就是由 event\_id、rcv\_account、amount、timestamp(事件发生时间) 和**分区键名**、**分区键值**这六个字段组成的 JSON 对象，其中**分区键名**是“amount#rcv\_account.history”，**分区键值**是“amount字段名#rcv\_account字段值.history”，代表了接收账户的历史交易金额。

这样，具有相同**分区键值**的“事件分身”会被划分到相同的分区流，继而路由到相同的**状态存储**节点上。在状态存储节点上，我们根据新到的“事件分身”，对 Keyed State 中保存的**历史业务数据**进行读取和更新。

比如，对于“count(pay\_account.history,1h)”这个特征，我们在 Keyed State 里保存了 pay\_account.history 流信息状态，也就是 pay\_account 的历史交易事件数据（为了简化问题，只记录最近 100 条历史记录）。再比如，对于“sum(amount#rcv\_account.history,1h)”这个特征，由于需要计算的是交易金额，所以我们在 Keyed State 里保存了 amount#rcv\_account.history 流信息状态，也就是 rcv\_account 的历史交易金额数据。

这里补充说明下我们的特征描述 DSL 是怎样定义的，下面的图 2 进行了说明。

![Drawing 3.png](https://s0.lgstatic.com/i/image6/M01/27/AD/Cgp9HWBdg5OAEkfmAAW3vi5u3Gs974.png)

在上面的图 2 中，我们使用了“#”符号来分割“目标字段”和“条件字段”。其中，“目标字段”是计算所针对的字段，“条件字段”则表明目标字段是对于哪个字段而言。如果没有“条件字段”，也就是没有“#”符号的话，那就将“目标字段”同时也作为“条件字段”使用。

比如，在“sum(amount#rcv\_account.history,1h)”中，目标字段是 amount，意味着 sum 计算是针对 amount 字段进行的，也就是说，我们是针对 amount 字段的值进行 sum 求和计算。条件字段是 rcv\_account， 则意味着前面的 amount 是对于接收账户 rcv\_account 而言的。而后缀“.history”则表明在 Keyed State 中保存的是**历史数据类型**的**流信息状态**，也就是接收账户 rcv\_account 的交易金额 amount 的“所有历史数据”。

我们也可以自行增加其他有意义的后缀，比如用“.window”后缀表明保存的是“窗口聚合数据”等，这样就可以支持保存其他类型的**流信息状态**。最后的计算窗口“1h”，则表明了计算的时间窗口，也就是我们需要 sum 求和的是过去 1 小时的数据。

定义好各个字段后，我们再将“amount字段名#rcv\_account字段值.history”作为划分 KeyedStream 的**分区键值**，这样相同接收账户 rcv\_account 的交易金额 amount 数据才会路由到相同的 KeyedStream 上，进而保存到相同的 Keyed State 中。

之后，我们将从 Keyed State 中读取出的历史业务数据，附加到“事件分身”上。接下来就可以**根据这些历史业务数据计算特征**了，比如 count、sum 和 count\_distinct 等。当特征计算完成后，此时**计算结果是分散在各个节点**上的，我们还需要将这些包含特征计算结果的“事件分身”合并起来，所以这一次是根据原始事件的事件 ID（也就是 event\_id）对“事件分身”进行路由。由于使用的是事件 ID，所以先前被分解为多个部分的“事件分身”会被路由到相同的节点上。这样，就能**将分开的特征计算结果重新合并起来，从而得到完整的特征集合了**。

最后，将特征集合输入基于规则的**风险评分部分**，就可以判定本次转账事件是否异常了。在图 1 中，最右边的矩形就是风险评分部分。这个相对简单，只需要使用 map 操作（第 09 课时已经讲解过 map）就可以完成风险评分计算了。

至此，我们就整理清楚风控引擎的整体实现思路了。接下来，我们根据这个思路来实现具体的风控引擎。

### 具体实现

我们按照图 1 所示的原理，来实现基于 Flink 的风控引擎。

#### 整体流程实现

下面的代码就是 Flink 风控引擎的整体流程。注意由于代码有点长，所以我对代码做了详细注释，你在阅读代码时可以重点关注下用"//"注释的部分（[完整代码参考这里](https://github.com/alain898/realtime_stream_computing_course/blob/main/course20/src/main/java/com/alain898/course/realtimestreaming/course20/riskengine/FlinkRiskEngine.java?fileGuid=xxQTRXtVcqtHK6j8)）。

```
public class FlinkRiskEngine {
private static final List<String[]> features = Arrays.asList(
        parseDSL("count(pay_account.history,1h)"),
        parseDSL("sum(amount#rcv_account.history,1h)"),
        parseDSL("count_distinct(rcv_account#pay_account.history,1h)")
    );
private static String[] parseDSL(String dsl) {
return Arrays.stream(dsl.split("[(,)]")).map(String::trim)
                .collect(Collectors.toList()).toArray(new String[0]);
    }
private static final Set<String> keys = features.stream().map(x -> x[1]).collect(Collectors.toSet());
public static void main(String[] args) throws Exception {
final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);
        env.enableCheckpointing(5000);
        FlinkKafkaConsumer010<String> myConsumer = createKafkaConsumer();
        DataStream<String> stream = env.addSource(myConsumer);
        DataStream counts = stream
                .map(new MapFunction<String, JSONObject>() {
@Override
public JSONObject map(String s) throws Exception {
if (StringUtils.isEmpty(s)) {
return new JSONObject();
                        }
return JSONObject.parseObject(s);
                    }
                })
                .flatMap(new EventSplitFunction())
                .keyBy(new KeySelector<JSONObject, String>() {
@Override
public String getKey(JSONObject value) throws Exception {
return value.getString("KEY_VALUE");
                    }
                })
                .map(new KeyEnrichFunction())
                .map(new FeatureEnrichFunction())
                .keyBy(new KeySelector<JSONObject, String>() {
@Override
public String getKey(JSONObject value) throws Exception {
return value.getString("EVENT_ID");
                    }
                })
                .flatMap(new FeatureReduceFunction())
                .map(new RuleBasedModeling());
        counts.print().setParallelism(1);
        env.execute("FlinkRiskEngine");
    }
```

在上面的代码中，我们先从 Kafka 读取出代表交易事件的 JSON 字符串，这个步骤是通过 createKafkaConsumer 方法完成。  
然后，我们将输入的字符串，解析为 JSON 对象。这一步是通过 map 函数和 JSONObject.parseObject 函数完成。

再然后，由于需要并行计算所有特征，所以我们将事件根据计算特征所需要的字段，分解为多个“事件分身”。这一步是通过 flatMap 函数和 EventSplitFunction 类完成。注意，这里就是我们在[第 09 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=614&sid=20-h5Url-0&buyFrom=2&pageId=1pz4#/detail/pc?id=6426&fileGuid=xxQTRXtVcqtHK6j8)讲到过的，用 flatMap 函数实现流式处理 Map/Reduce 计算模式中的 Map 部分。

接着，我们让具有相同“分区键值”的“事件分身”，划分到相同的分区流 KeyedStream 里，进而路由到相同的状态存储节点上，存储到相同的 Keyed State 里。这一步是通过 keyBy 函数和 KeySelector 类完成的。

再接着，我们在状态节点上，完成对 Keyed State 里**流信息状态**的读取和更新，并将读取出的流信息状态，也就是**历史业务数据**附加到“事件分身”上。这一步是通过 map 函数和 KeyEnrichFunction 类实现的。

之后，我们基于**历史业务数据**计算出特征，并将计算结果添加到“事件分身”上。这一步是通过 map 函数和 FeatureEnrichFunction 类完成的。

再之后，由于前面计算出特征的“事件分身”还散布在不同节点上，所以需要根据事件 ID，也就 event\_id 字段，将原本属于同一事件的“事件分身”重新路由到相同的 KeyedStream 里。这一步是通过 keyBy 函数和 KeySelector 类完成的。

接着，由于属于同一事件的“事件分身”只是路由到了相同的 KeyedStream 里，要想得到完整的特征集合，还需要将这些“事件分身”合并起来。这一步是通过 flatMap 函数和 FeatureReduceFunction 类完成的。可以看到，这里就是在[第 09 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=614&sid=20-h5Url-0&buyFrom=2&pageId=1pz4#/detail/pc?id=6426&fileGuid=xxQTRXtVcqtHK6j8)讲到过的，用 flatMap 函数实现流式处理 Map/Reduce 计算模式中的 Reduce 部分。

最后，当我们得到包含完整特征集合的事件后，就可以根据基于规则的模型，对这次交易事件进行风险判定了。这一步是通过 map 函数和 RuleBasedModeling 类实现的。

至此，我们就实现了一个完整的风控引擎执行流程。这个风控引擎的代码是严格按照前面所描述的实现原理所开发的，在代码的注释部分也对各个步骤做了详细说明。

不过，上面的代码描述的是一个整体流程，其中的几个关键类实现还需要进一步详细说明。

#### 关键类实现

**首先是 EventSplitFunction 类**。下面是它的具体实现代码。

```
public static class EventSplitFunction implements FlatMapFunction<JSONObject, JSONObject> {
private static final Set<String> keys = FlinkRiskEngine.keys;
@Override
public void flatMap(JSONObject value, Collector<JSONObject> out) throws Exception {
        String eventId = UUID.randomUUID().toString();
long timestamp = value.getLongValue("timestamp");
        JSONObject event = new JSONObject();
        event.put("KEY_NAME", "event"); 
        event.put("KEY_VALUE", eventId);
        event.put("EVENT_ID", eventId);
        event.putAll(value);
        out.collect(event);
        keys.forEach(key -> {
            JSONObject json = new JSONObject();
            json.put("timestamp", timestamp);
            json.put("KEY_NAME", key);  
            json.put("KEY_VALUE", genKeyValue(value, key));
            json.put("EVENT_ID", eventId);
            genKeyFields(key).forEach(f -> json.put(f, value.get(f)));
            out.collect(json);
        });
    }
private String genKeyValue(JSONObject event, String key) {
if (!key.endsWith(".history")) {
throw new UnsupportedOperationException("unsupported key type");
        }
        String[] splits = key.replace(".history", "").split("#");
        String keyValue;
if (splits.length == 1) {
            String target = splits[0];
            keyValue = String.format("%s#%s.history", target, String.valueOf(event.get(target)));
        } else if (splits.length == 2) {
            String target = splits[0];
            String on = splits[1];
            keyValue = String.format("%s#%s.history", target, String.valueOf(event.get(on)));
        } else {
throw new UnsupportedOperationException("unsupported key type");
        }
return keyValue;
    }
private Set<String> genKeyFields(String key) {
if (!key.endsWith(".history")) {
throw new UnsupportedOperationException("unsupported key type");
        }
        String[] splits = key.replace(".history", "").split("#");
return new HashSet<>(Arrays.asList(splits));
    }
}
```

在上面的代码中，我们在 flatMap 函数里将代表原始事件的 value 对象，按照特征计算时所需要使用的字段进行分解，从而原本的一个事件被分解为多个“事件分身”。其中，除了一个“事件分身”代表原始事件本身以外，其他的“事件分身”则各自包含了原始事件的部分字段，之后在特征计算时需要使用到这些字段。

可以看到，这里我们用 flatMap 函数将一条数据转化成了多条数据，这正是我们在[第 09 课时](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=614&sid=20-h5Url-0&buyFrom=2&pageId=1pz4#/detail/pc?id=6426&fileGuid=xxQTRXtVcqtHK6j8)讲到过的，用 flatMap 函数实现流式处理 Map/Reduce 计算模式中的 Map 部分。

**然后是 KeyEnrichFunction 类**。下面是它的具体实现代码。

```
public static class KeyEnrichFunction extends RichMapFunction<JSONObject, JSONObject> {
private ValueState<Serializable> keyState;
@Override
public void open(Configuration config) {
            keyState = getRuntimeContext().getState(new ValueStateDescriptor<>("saved keyState", Serializable.class));
        }
        // 从 ValueState 中读取出流信息状态，也就是历史业务数据，
        // 并转化为指定类型后再返回。
private <T> T getState(Class<T> tClass) throws IOException {
return tClass.cast(keyState.value());
        }
private void setState(Serializable v) throws IOException {
            keyState.update(v);
        }
@Override
public JSONObject map(JSONObject event) throws Exception {
            String keyName = event.getString("KEY_NAME");
if (keyName.equals("event")) {
return event;
            }
if (keyName.endsWith(".history")) {
                JSONArray history = getState(JSONArray.class);
                // 如果 history 为 null，
                // 说明这是该 KeyedStream 分区流上到达的第一个事件，
                // 所以需要创建一个空的JSON数组，
                // 后续就是用这个 JSON 数组来存储"历史业务数据"，
                // 并且保存到 ValueState 里的也就是这个 JSON 数组。
if (history == null) {
                    history = new JSONArray();
                }
                history.add(event);
if (history.size() > 100) {
                    history.remove(0);
                }
                setState(history);
                JSONObject newEvent = new JSONObject();
                newEvent.putAll(event);
                newEvent.put("HISTORY", history);
return newEvent;
            } else {
throw new UnsupportedOperationException("unsupported key type");
            }
        }
    }
```

在上面的代码中，使用 ValueState 这一 Keyed State 实现了**流信息状态**的**分布式存储**。我们将每个 KeyedStream 上最近 100 个历史记录保存在了 ValueState 中，并将历史记录附加到“事件分身”上，之后的特征计算就是基于这些历史记录。

**再然后是 FeatureEnrichFunction 类。** 下面是它的具体实现代码。

```
public static class FeatureEnrichFunction extends RichMapFunction<JSONObject, JSONObject> {
private static final List<String[]> features = FlinkRiskEngine.features;
@Override
public JSONObject map(JSONObject value) throws Exception {
            String keyName = value.getString("KEY_NAME");
if (keyName.equals("event")) {
return value;
            }
for (String[] feature : features) {
                String key = feature[1];
if (!StringUtils.equals(key, keyName)) {
continue;
                }
                String function = feature[0];
long window = parseTimestamp(feature[2]);
                JSONArray history = value.getJSONArray("HISTORY");
                String target = key.replace(".history", "").split("#")[0];
                Object featureResult;
if ("sum".equalsIgnoreCase(function)) {
                    featureResult = doSum(history, target, window);
                } else if ("count".equalsIgnoreCase(function)) {
                    featureResult = doCount(history, target, window);
                } else if ("count_distinct".equalsIgnoreCase(function)) {
                    featureResult = doCountDistinct(history, target, window);
                } else {
throw new UnsupportedOperationException(String.format("unsupported function[%s]", function));
                }
                value.putIfAbsent("features", new JSONObject());
                String featureName = String.format("%s(%s,%s)", feature[0], feature[1], feature[2]);
                value.getJSONObject("features").put(featureName, featureResult);
            }
return value;
        }
    }
```

在上面的代码中，在获得每个特征计算所需字段的流信息状态（在本例中也就是"历史业务数据"）后，我们根据最初特征 DSL 的定义，选择对应的计算方法和窗口，然后完成特征的计算。比如，对于“sum(amount#rcv\_account.history,1h)”，计算方法 sum 对应的实现是 doSum 函数，history 参数就是接收账户 rcv\_account 交易金额 amount 的历史交易数据，而 1h 就是计算窗口。

**接下来是 FeatureReduceFunction 类**。下面是它的具体实现代码。

```
public static class FeatureReduceFunction extends RichFlatMapFunction<JSONObject, JSONObject> {
private ValueState<JSONObject> merged;
private static final List<String[]> features = FlinkRiskEngine.features;
@Override
public void open(Configuration config) {
            merged = getRuntimeContext().getState(new ValueStateDescriptor<>("saved reduceJson", JSONObject.class));
        }
@Override
public void flatMap(JSONObject value, Collector<JSONObject> out) throws Exception {
            JSONObject mergedValue = merged.value();
if (mergedValue == null) {
                mergedValue = new JSONObject();
            }
            String keyName = value.getString("KEY_NAME");
if (keyName.equals("event")) {
                mergedValue.put("event", value);
            } else {
                mergedValue.putIfAbsent("features", new JSONObject());
if (value.containsKey("features")) {
mergedValue.getJSONObject("features").putAll(value.getJSONObject("features"));
                }
            }
if (mergedValue.containsKey("event") && mergedValue.containsKey("features")
                    && mergedValue.getJSONObject("features").size() == features.size()) {
                out.collect(mergedValue);
                merged.clear();
            } else {
                merged.update(mergedValue);
            }
        }
    }
```

在上面的代码中，我们将分布在各个计算节点上的特征计算结果，按照事件 ID 路由到相同的 KeyedStream 上，然后用 ValueState 保存合并结果。当最终所有特征计算结果都收集齐全后，就可以将这个最终合并结果输出了。

同时为了防止资源泄漏，还清除掉不再需要的 ValueState 状态。你可以特别注意下，这里我们就是用 flatMap 函数配合 KeyedStream 和 ValueState，实现了流计算 Map/Reduce 模式中的 Reduce 部分。它与前面 EventSplitFunction 类的功能是相对应的，在 EventSplitFunction 类中，我们用 flatMap 函数配合 KeyedStream，实现了流计算 Map/Reduce 模式中的 Map 部分。

**最后是 RuleBasedModeling 类**。下面是它的具体实现代码。

```
public static class RuleBasedModeling implements MapFunction<JSONObject, JSONObject> {
@Override
public JSONObject map(JSONObject value) throws Exception {
boolean isAnomaly = (
value.getJSONObject("features").getDouble("count(pay_account.history,1h)") > 5 && value.getJSONObject("features").getDouble("sum(amount#rcv_account.history,1h)") > 5000 && value.getJSONObject("features").getDouble("count_distinct(rcv_account#pay_account.history,1h)") <= 2 );
            value.put("isAnomaly", isAnomaly);
return value;
        }
    }
```

在上面的代码中，我们通过基于规则的模型，来判定本次交易事件是否异常。我们将判定的结果附加在了事件的 isAnomaly 字段。这个附加了判定结果的事件，就是风控引擎的最终风险评分输出。  
至此，一个基于 Flink 的风控引擎就实现了。本小节的完整代码可以[参见这里的代码](https://github.com/alain898/realtime_stream_computing_course/blob/main/course20/src/main/java/com/alain898/course/realtimestreaming/course20/riskengine/FlinkRiskEngine.java?fileGuid=xxQTRXtVcqtHK6j8)。

可以看到，我们在整个实现过程中，为了实现特征计算的并行化，充分运用了 flatMap、KeyedStream、Keyed State 的作用，实现了流计算的 Map/Reduce 或者说 Fork/Join 计算模式。

另外，我们在计算过程中，为了记录历史业务信息，还使用了 Keyed State 来保存流信息状态，这正是 Keyed State 最大的价值所在！如果没有 Flink 提供的 Keyed State，可能我们又会采用类似于 Redis 这样的外部数据库了，这会使系统复杂很多，并且一定程度会降低处理的性能以及系统的水平扩展能力。

### 小结

今天，我们使用 Flink 实现了一个包含特征提取和风险评分功能的风控引擎。

在实现过程中，我们充分运用了 Flink 对流的逻辑划分功能以及状态管理功能，它们分别对应着 KeyedStream 和 Keyed State。其中，KeyedStream 实现了计算能力的水平扩展，而 Keyed State 实现了状态存储的水平扩展。

如果从资源的角度看，KeyedStream 的扩展相当于是对 CPU 的水平扩展，而 Keyed State 的扩展则相当于是对“内存”的扩展。是的，你没有看错，我的意思就是“内存”，而不是“磁盘”。对于这点，你可以先仔细体会下。我在课程后续的彩蛋 1 中，对于这点还会专门详细讲解。

另外，用 flatMap、KeyedStream、Keyed State 来实现流计算的 Map/Reduce 或者说 Fork/Join 计算模式，以及用 Keyed State 来保存各种流信息状态，这两点是我希望你能够重点且熟练掌握的内容，所以你一定要好好理解下。

最后，我们今天的例子在描述特征时运用了 DSL，也就是领域定义语言。你在工作中用到过 DSL 吗？使用 DSL 有什么好处呢？可以将你的想法或问题写在留言区。

下面是本课时的知识脑图，以帮助你理解。

![Drawing 5.png](https://s0.lgstatic.com/i/image6/M01/27/AA/CioPOWBdg8WAcuRmAALNmz21mcs289.png)
