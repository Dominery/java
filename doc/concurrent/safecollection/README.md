# 线程安全集合

如果多线程要并发地修改一个数据结构，那么很容易会破坏这个数据结构。可以通过提供锁来保护共享数据结构，但是选择线程安全的实现作为替代更容易。

* 实现

  java.util.concurrent包提供了映射、有序集和队列的高效实现：ConcurrentHashMap、ConcurrentSkipListMap、ConcurrentSkipListSet和ConcurrentLinkedQueue。

  这些集合使用复杂的算法，通过允许并发地访问数据结构的不同部分来使竞争极小化。

* 特性

  确定这样的集合当前的大小通常需要遍历。

  集合返回弱一致性（weakly consistent）的迭代器。这意味着迭代器不一定能反映出它们被构造之后的所有的修改，但是，它们不会将同一个值返回两次，也不会抛出Concurrent ModificationException异常。