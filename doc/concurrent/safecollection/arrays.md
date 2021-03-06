# 数组并行操作

在Java SE 8中，Arrays类提供了大量并行化操作。

| 方法           | 说明                                                   |
| -------------- | ------------------------------------------------------ |
| parallelSort   | 可以对一个基本类型值或对象的数组排序。                 |
| parallelSetAll | 用由一个函数计算得到的值填充一个数组                   |
| parallelPrefix | 用对应一个给定结合操作的前缀的累加结果替换各个数组元素 |

### 写数组的拷贝

CopyOnWriteArrayList和CopyOnWriteArraySet是线程安全的集合，其中所有的修改线程对底层数组进行复制。如果在集合上进行迭代的线程数超过修改线程数，这样的安排是很有用的。当构建一个迭代器的时候，它包含一个对当前数组的引用。如果数组后来被修改了，迭代器仍然引用旧数组，但是，集合的数组已经被替换了。因而，旧的迭代器拥有一致的（可能过时的）视图，访问它无须任何同步开销。