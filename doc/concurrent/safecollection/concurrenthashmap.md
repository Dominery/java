# ConcurrentHashMap

ConcurrentHashMap允许并发地进行新增和更新操作，因为它仅对内部数据结构的某些部分上锁。因此，和另一种选择，即同步式的Hashtable比较起来，它具有更高的读写性能。

### 原子更新操作

Java SE 8提供了一些可以更方便地完成原子更新的方法。

| 方法             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| compute          | 提供一个键和一个计算新值的函数。这个函数接收键和相关联的值（如果没有值，则为null），它会计算新值。 |
| computeIfPresent | 只在已经有原值的情况下计算新值                               |
| computeIfAbsent  | 只有没有原值的情况下计算新值                                 |
| merge            | 提供键、初始值、函数，如果键不存在，使用初始值，否则调用函数结合原值和初始值 |

如果传入compute或merge的函数返回null，将从映射中删除现有的条目。

使用不同方法实现映射值自增。

```java
ConcurrentHashMap<String,Long> map = new ConcurrentHashMap<>();
ConcurrentHashMap<String,LongAdder>mapl = new ConcurrentHashMap<>();
// in some threads
map.compute(word,(key,value)->value==null?1:vlue++);
mapl.putIfAbsent(word,new LongAdder()).increment();
mapl.computeIfAbsent(word,k->new LongAdder()).increment();
map.merge(word,1L,Long::sum);
```

### 批操作

Java SE 8为并发散列映射提供了批操作，即使有其他线程在处理映射，这些操作也能安全地执行。批操作会遍历映射，处理遍历过程中找到的元素。

1. forEach——对每个键值对进行特定的操作

   > forEach方法有两种形式。
   >
   > 第一个只为各个映射条目提供一个消费者函数；第二种形式还有一个转换器函数，这个函数要先提供，其结果会传递到消费者。转换器可以用作为一个过滤器。只要转换器返回null，这个值就会被跳过。

2. reduce——使用给定的精简函数（reductionfunction），将所有的键值对整合出一个结果

   > reduce操作用一个累加函数组合其输入，也可以提供一个转换器函数。

3. search——对每一个键值对执行一个函数，直到函数的返回值为一个非空值

以上每一种操作都支持四种形式，接受使用键、值、Map.Entry以及键值对的函数：

* 使用键和值的操作（forEach、reduce、search）
* 使用键的操作（forEachKey、reduceKeys、searchKeys）
* 使用值的操作（forEachValue、reduceValues、searchValues）
* 使用Map.Entry对象的操作（forEachEntry、reduceEntries、searchEntries）

对于上述各个操作，需要指定一个阈值。如果映射包含的元素多于这个阈值，就会并行完成批操作。如果希望批操作在一个线程中运行，可以使用阈值Long.MAX_VALUE。如果希望用尽可能多的线程运行批操作，可以使用阈值1。

> 这些操作不会对ConcurrentHashMap的状态上锁。它们只会在运行过程中对元素进行操作。应用到这些操作上的函数不应该对任何的顺序，或者其他对象，抑或在计算过程发生变化的值，有依赖。
>

* 计数

  ConcurrentHashMap类提供了一个mappingCount方法，它以长整型long返回map中映射的数目。

### 集合视图

如果需要的是一个线程安全的集而不是映射。并没有一个ConcurrentHashSet类，静态newKeySet方法会生成一个Set<K>，这实际上是ConcurrentHashMap<K, Boolean>的一个包装器。

如果原来有一个映射，ConcurrentHashMap类提供了一个名为KeySet的新方法，该方法以Set的形式返回ConcurrentHashMap的一个视图。不能向视图增加元素，因为没有相应的值可以增加。Java SE 8为ConcurrentHashMap增加了第二个keySet方法，包含一个默认值，可以在为视图增加元素时使用。