# 面试题

1. ArrayList、LinkedList、Vector三者的异同？

   > 同：都实现List接口，存储的数据特点相同
   >
   > 异：
   >
   > ​	ArrayList是List接口的主要实现类，线程不安全、效率高，底层使用Object[] 存储
   >
   > ​	Vector是List接口的古老实现类，线程安全、效率低，底层使用Object[] 存储
   >
   > ​	LinkedList底层使用双向链表存储，如果频繁插入和删除，其效率高