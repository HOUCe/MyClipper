---
created: 2021-10-11
tags: []
source: https://kaiwu.lagou.com/course/courseInfo.htm?courseId=81#/detail/pc?id=2035
author: 
---

# [42讲轻松通关 Flink - 资深大数据工程师 - 拉勾教育](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=81#/detail/pc?id=2035)


本课时我们主要讲解 Flink 中的海量数据高效去重。

**消除重复数据**是我们在实际业务中经常遇到的一类问题。在大数据领域，重复数据的删除有助于减少存储所需要的存储容量。而且在一些特定的业务场景中，重复数据是不可接受的，例如，精确统计网站一天的用户数量、在事实表中统计每天发出的快递包裹数量。在传统的离线计算中，我们可以直接用 SQL 通过 DISTINCT 函数，或者数据量继续增加时会用到类似 MapReduce 的思想。那么在实时计算中，去重计数是一个增量和长期的过程，并且不同的场景下因为效率和精度问题方案也需要变化。

针对上述问题，我们在这里列出几种常见的 Flink 中实时去重方案：

-   基于状态后端
    
-   基于 HyperLogLog
    
-   基于布隆过滤器（BloomFilter）
    
-   基于 BitMap
    
-   基于外部数据库
    

下面我们依次讲解上述几种方案的适用场景和实现原理。

### 基于状态后端

我们在第 09 课时“Flink 状态与容错”中曾经讲过 Flink State 状态后端的概念和原理，其中状态后端的种类之一是 **RocksDBStateBackend**。它会将正在运行中的状态数据保存在 RocksDB 数据库中，该数据库默认将数据存储在 TaskManager 运行节点的数据目录下。

RocksDB 是一个 K-V 数据库，我们可以利用 MapState 进行去重。这里我们模拟一个场景，计算每个商品 SKU 的访问量。

整体的实现代码如下：

```
public class MapStateDistinctFunction extends KeyedProcessFunction<String,Tuple2<String,Integer>,Tuple2<String,Integer>> {
private transient ValueState<Integer> counts;
@Override
public void open(Configuration parameters) throws Exception {
        StateTtlConfig ttlConfig = StateTtlConfig
                .newBuilder(org.apache.flink.api.common.time.Time.minutes(24 * 60))
                .setUpdateType(StateTtlConfig.UpdateType.OnCreateAndWrite)
                .setStateVisibility(StateTtlConfig.StateVisibility.NeverReturnExpired)
                .build();
        ValueStateDescriptor<Integer> descriptor = new ValueStateDescriptor<Integer>("skuNum", Integer.class);
        descriptor.enableTimeToLive(ttlConfig);
        counts = getRuntimeContext().getState(descriptor);
super.open(parameters);
    }
@Override
public void processElement(Tuple2<String, Integer> value, Context ctx, Collector<Tuple2<String, Integer>> out) throws Exception {
        String f0 = value.f0;
if(counts.value() == null){
            counts.update(1);
        }else{
            counts.update(counts.value()+1);
        }
        out.collect(Tuple2.of(f0, counts.value()));
    }
}
```

在上面的这段代码里，我们实现了这样的逻辑：定义了一个 MapStateDistinctFunction 类，该类继承了 KeyedProcessFunction。核心的处理逻辑在 processElement 方法中，当一条数据经过，我们会在 MapState 中判断这条数据是否已经存在，如果不存在那么计数为 1，如果存在，那么在原来的计数上加 1。

这里需要注意的是，我们自定义了状态的过期时间是 24 小时，在实际生产中大量的 Key 会使得状态膨胀，我们可以对存储的 Key 进行处理。例如，使用加密方法把 Key 加密成几个字节进再存储。

### 基于 HyperLogLog

> HyperLogLog 是一种估计统计算法，被用来统计一个集合中不同数据的个数，也就是我们所说的去重统计。HyperLogLog 算法是用于基数统计的算法，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2 的 64 方个不同元素的基数。HyperLogLog 适用于大数据量的统计，因为成本相对来说是更低的，最多也就占用 12KB 内存。

我们在不需要 100% 精确的业务场景下，可以使用这种方法进行统计。首先新增依赖：

```
<dependency>
<groupId>net.agkn</groupId>
<artifactId>hll</artifactId>
<version>1.6.0</version>
</dependency>
```

我们还是以上述的商品 SKU 访问量作为业务场景，数据格式为 <SKU, 访问的用户 id>:

```
public class HyperLogLogDistinct implements AggregateFunction<Tuple2<String,Long>,HLL,Long> {
@Override
public HLL createAccumulator() {
return new HLL(14, 5);
    }
@Override
public HLL add(Tuple2<String, Long> value, HLL accumulator) {
        accumulator.addRaw(value.f1);
return accumulator;
    }
@Override
public Long getResult(HLL accumulator) {
long cardinality = accumulator.cardinality();
return cardinality;
    }
@Override
public HLL merge(HLL a, HLL b) {
        a.union(b);
return a;
    }
}
```

在上面的代码中，addRaw 方法用于向 HyperLogLog 中插入元素。如果插入的元素非数值型的，则需要 hash 过后才能插入。accumulator.cardinality() 方法用于计算 HyperLogLog 中元素的基数。

需要注意的是，HyperLogLog 并不是精准的去重，如果业务场景追求 100% 正确，那么一定不要使用这种方法。

### 基于布隆过滤器（BloomFilter）

> BloomFilter（布隆过滤器）类似于一个 HashSet，用于快速判断某个元素是否存在于集合中，其典型的应用场景就是能够快速判断一个 key 是否存在于某容器，不存在就直接返回。

需要注意的是，和 HyperLogLog 一样，布隆过滤器不能保证 100% 精确。但是它的插入和查询效率都很高。

我们可以在非精确统计的情况下使用这种方法：

```
public class BloomFilterDistinct extends KeyedProcessFunction<Long, String, Long> {
private transient ValueState<BloomFilter> bloomState;
private transient ValueState<Long> countState;
@Override
public void processElement(String value, Context ctx, Collector<Long> out) throws Exception {
        BloomFilter bloomFilter = bloomState.value();
        Long skuCount = countState.value();
if(bloomFilter == null){
            BloomFilter.create(Funnels.unencodedCharsFunnel(), 10000000);
        }
if(skuCount == null){
            skuCount = 0L;
        }
if(!bloomFilter.mightContain(value)){
            bloomFilter.put(value);
            skuCount = skuCount + 1;
        }
        bloomState.update(bloomFilter);
        countState.update(skuCount);
        out.collect(countState.value());
    }
}
```

我们使用 Guava 自带的 BloomFilter，每当来一条数据时，就检查 state 中的布隆过滤器中是否存在当前的 SKU，如果没有则初始化，如果有则数量加 1。

### 基于 BitMap

上面的 HyperLogLog 和 BloomFilter 虽然减少了存储但是丢失了精度， 这在某些业务场景下是无法被接受的。下面的这种方法不仅可以减少存储，而且还可以做到完全准确，那就是使用 BitMap。

> Bit-Map 的基本思想是用一个 bit 位来标记某个元素对应的 Value，而 Key 即是该元素。由于采用了 Bit 为单位来存储数据，因此可以大大节省存储空间。
> 
> 假设有这样一个需求：在 20 亿个随机整数中找出某个数 m 是否存在其中，并假设 32 位操作系统，4G 内存。在 Java 中，int 占 4 字节，1 字节 = 8 位（1 byte = 8 bit）
> 
> 如果每个数字用 int 存储，那就是 20 亿个 int，因而占用的空间约为 (2000000000\*4/1024/1024/1024)≈7.45G
> 
> 如果按位存储就不一样了，20 亿个数就是 20 亿位，占用空间约为 (2000000000/8/1024/1024/1024)≈0.233G

在使用 BitMap 算法前，如果你需要去重的对象不是数字，那么需要先转换成数字。例如，用户可以自己创造一个映射器，将需要去重的对象和数字进行映射，最简单的办法是，可以直接使用数据库维度表中自增 ID。

首先我们新增一个依赖：

```
<dependency>
<groupId>org.roaringbitmap</groupId>
<artifactId>RoaringBitmap</artifactId>
<version>0.8.0</version>
</dependency>
```

然后，我们还以商品的 SKU 的访问记录举例：

```
public class BitMapDistinct implements AggregateFunction<Long, Roaring64NavigableMap,Long> {
@Override
public Roaring64NavigableMap createAccumulator() {
return new Roaring64NavigableMap();
    }
@Override
public Roaring64NavigableMap add(Long value, Roaring64NavigableMap accumulator) {
        accumulator.add(value);
return accumulator;
    }
@Override
public Long getResult(Roaring64NavigableMap accumulator) {
return accumulator.getLongCardinality();
    }
@Override
public Roaring64NavigableMap merge(Roaring64NavigableMap a, Roaring64NavigableMap b) {
return null;
    }
}
```

在上述方法中，我们使用了 Roaring64NavigableMap，其是 BitMap 的一种实现，然后我们的数据是每次被访问的 SKU，把它直接添加到 Roaring64NavigableMap 中，最后通过 accumulator.getLongCardinality() 可以直接获取结果。

### 基于外部数据库

假如我们的业务场景非常复杂，并且数据量很大。为了防止无限制的状态膨胀，也不想维护庞大的 Flink 状态，我们可以采用外部存储的方式，比如可以选择使用 Redis 或者 HBase 存储数据，我们只需要设计好存储的 Key 即可。同时使用外部数据库进行存储，我们不需要关心 Flink 任务重启造成的状态丢失问题，但是有可能会出现因为重启恢复导致的数据多次发送，从而导致结果数据不准的问题。

### 总结

这一课时我们讲解了多种不同的 Flink 大数据下的去重方法，并且详细比较了它们的异同。在实际的业务场景中，精确去重和非精确去重需要灵活选择不同的方案，在准确性和效率上达到统一。

[点击这里下载本课程源码](https://github.com/wangzhiwubigdata/quickstart)。
