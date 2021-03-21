# 集合

集合与数组都是存储数据的容器。

数组在存储数据时有以下缺点：

1. 初始化后长度固定
2. 提供的方法有限，不能满足操作数据的需求
3. 没有现成的方法或属性可以获取元素的个数
4. 不能兼顾到存储的数据的特性

集合能够解决数组的以上缺陷。

集合分为Collection和Map体系

* `Collection`接口

  > 单列数据，定义了存取一组对象的方法的集合

  * `List`接口：元素有序、可重复的集合

    ArrayList、linkedList、Vector

  * `Set`接口：元素无序、不可重复的集合

    HashSet、LinkedHashSet、TreeSet

* `Map`接口

  > 双列数据、保存具有映射关系"key-value"的集合

  HashMap、LinkedHashMap、TreeMap、Hashtable、Properties

