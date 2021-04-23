# 并发相关包

java8更新了java.util.concurrent.atomic包并引入了ConcurrentHashMap类来增加对并发操作的支持。

### 原子操作

java.util.concurrent.atomic包提供了多个对数字类型进行操作的类，比如Atomic-Integer和AtomicLong，它们支持对单一变量的原子操作。

这些类在Java 8中新增了更多的方法支持。

| 方法             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| getAndUpdate     | 以原子方式用给定的方法更新当前值，并返回变更之前的值。       |
| updateAndGet     | 以原子方式用给定的方法更新当前值，并返回变更之后的值。       |
| getAndAccumulate | 以原子方式用给定的方法对当前及给定的值进行更新，并返回变更之前的值。 |
| accumulateAndGet | 以原子方式用给定的方法对当前及给定的值进行更新，并返回变更之后的值。 |

* Adder和Accumulator

  多线程的环境中，如果多个线程需要频繁地进行更新操作，且很少有读取的动作（比如，在统计计算的上下文中）, Java API文档中推荐大家使用新的类LongAdder、LongAccumulator、Double-Adder以及DoubleAccumulator，尽量避免使用它们对应的原子类型。

### ConcurrentHashMap

ConcurrentHashMap允许并发地进行新增和更新操作，因为它仅对内部数据结构的某些部分上锁。因此，和另一种选择，即同步式的Hashtable比较起来，它具有更高的读写性能。

* 类流操作

  ConcurrentHashMap支持三种新的操作，这些操作在流中所见的很像：

  1. forEach——对每个键值对进行特定的操作
  2. reduce——使用给定的精简函数（reductionfunction），将所有的键值对整合出一个结果
  3. search——对每一个键值对执行一个函数，直到函数的返回值为一个非空值

  以上每一种操作都支持四种形式，接受使用键、值、Map.Entry以及键值对的函数：

  * 使用键和值的操作（forEach、reduce、search）
  * 使用键的操作（forEachKey、reduceKeys、searchKeys）
  * 使用值的操作（forEachValue、reduceValues、searchValues）
  * 使用Map.Entry对象的操作（forEachEntry、reduceEntries、searchEntries）

  > 这些操作不会对ConcurrentHashMap的状态上锁。它们只会在运行过程中对元素进行操作。应用到这些操作上的函数不应该对任何的顺序，或者其他对象，抑或在计算过程发生变化的值，有依赖。
  >
  > 除此之外，需要为这些操作指定一个并发阈值。

* 计数

  ConcurrentHashMap类提供了一个新的方法，名叫mappingCount，它以长整型long返回map中映射的数目。

* 集合视图

  ConcurrentHashMap类还提供了一个名为KeySet的新方法，该方法以Set的形式返回ConcurrentHashMap的一个视图。