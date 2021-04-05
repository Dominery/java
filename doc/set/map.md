# Map接口

Map存储双列数据。map实现类有5个，为Hashtable、Properties、HashMap、LinkedHashMap、TreeMap。

> HashMap:主要实现类，线程不安全，效率高，可存储null的key和value
>
> ​	|---LinkedHashMap:保证遍历map元素时，按照添加顺序遍历
>
> TreeMap:按照添加的key对其进行排序，底层使用红黑树
>
> Hashtable:古老的实现类，线程安全，效率低，不能存储null的key和value
>
> ​	|--Properties:常用来处理文件，key和value都是String

* Map结构的理解

  Map中的key：无序的、不可重复的，使用Set存储所有的key

  Map中的value：无序的、可重复的，使用Collection存储所有的value

  Map中的entry：key-value构成了一个Entry对象，无序的、不可重复的，使用Set存储所有的Entry

* HashMap底层

  * jdk7

    ```java
    HashMap map = new HashMap(); //create an Entry[] table array of 16 length
    ```

    调用put()方法，存放数据时

    1. 调用key所在类的hashCode()计算哈希值
    2. 哈希值经计算后得到在Entry数组中的索引
    3. 判断该位置是否有数据
       * 如果此位置为空，则数据添加成功
       * 如果此位置不为空，则与存在的数据依次比较其哈希值
         * 如果哈希值不同，添加成功
         * 如果哈希值相同，调用该数据equals()方法
           * 如果equals()返回false：添加成功
           * 如果equals()返回true：替换value

    如果容量不够，默认扩容为原来容量的2倍，并复制原有数据。

  * jdk8

    1. 调用空参构造器时，没有创建数组。

    2. 底层数组为Node[]

    3. 首次调用put()，底层创建长度为16的数组

    4. 数组+链表（jdk7）数组+链表+红黑树（jdk8）

       > 当数组某个索引位置上的元素以链表形式存储的元素个数>8&&当前数组长度>64，此时，此索引位置上的数据改为红黑树存储

* LinkedHashMap

  Entry继承了父类HashMap的Node，并添加了before和after属性用于记录元素添加的顺序。

* TreeMap

  1. 在向TreeMap中添加数据时，key数据的类型必须一致。
  2. TreeMap按照key排序，存在两种排序方式：自然排序和定制排序
     * 自然排序时，对象需要实现Comparable接口
     * 定制排序需要向TreeSet构造器传入Comparator对象

* Properties

  ```java
  Properties pros = new Properties();
  FileInputStream fis = new FileInputStream("jdbc.properties");
  pros.load(fis);
  String name = pros.getProperty("name");
  String password = pros.getProperty("password");
  ```

  

* 常用方法

  1. 添加：put(Object key,Object value) putAll(Map m)
  2. 删除：remove(Object key)
  3. 修改：put(Object key,Object value)
  4. 清空：clear()
  5. 查询：get(Object key)
  6. 遍历：
     * 遍历key：keySet()
     * 遍历value：values()
     * 遍历key-value：entrySet()