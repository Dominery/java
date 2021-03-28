# Collection子接口

### List接口

List集合中元素有序、可重复。List的具体实现类有三个：ArrayList、LinkedList、Vector。

#### ArrayList

* 源码分析

    jdk7中的ArrayList对象的创建类似于单例模式的饿汉式，jdk8中的ArrayList对象创建类似于单例模式的懒汉式，节省内存。

    * jdk7

        ```java
        public ArrayList(){
            this(10); //create array of ten length
        }
        ```

        调用空参构造器，底层创建长度为10的Object[] 数组。

        如果数组容量不够，默认扩容至原来的1.5倍，同时将原来数组的元素复制到新数组中。

    * jdk8

      调用空参构造器，底层没有创建数组，Object[] element初始化为{}

      第一次调用add()底层才创建了10的数组，并添加数据
    
* 常用方法

    1. 增：add(Object obj)
    2. 删：remove(int index)/remove(Object obj)
    3. 改：set(int index,Object obj)
    4. 查：get(int index)
    5. 插：add(int index,Object ele)
    6. 长度：size()
    7. 遍历：Iterator迭代器、增强for循环、普通循环

#### LinkedList源码分析

调用空参构造器，内部声明了Node类型的first、last属性，值为null，当调用add()，创建Node对象，并连接到last上。

```java
private static class Node<E>{
    E item;
    Node<E> next;
    Node<E> prev;
    Node(Node<E>prev,E element,Node<E>next){
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### Set接口

Set集合中元素无序、不可重复。Set具体实现类有三个：HashSet、LinkedHashSet、TreeSet。

> HashSet：Set接口的主要实现类，线程不安全，可以存储null
>
> LinkedHashSet：HashSet的子类，遍历其内部数据时，可以按照添加的顺序
>
> TreeSet：使用红黑树存储，可以按照添加对象的指定属性进行排序。

Set接口没有额外定义新的方法，使用的都是Collection中声明的方法。

* Set的特性

  * 无序性

    > 不等于随机性，存储的数据在内存中的顺序与添加顺序无关

  * 不可重复性

    > 保证添加的元素按照equals()方法判断是，不能返回true

* 添加元素的过程：以HashSet为例

  1. 调用元素所在类的hashCode()方法，计算其哈希值
  2. 通过哈希函数计算该哈希值在底层数组中的存放位置
  3. 判断该位置有无元素
     * 如果该位置无元素，则添加成功 ——1
     * 如果该位置有元素（或以链表形式存在的多个元素），则依次与之比较哈希值
       * 如果不同，则添加成功 ——2
       * 如果相同，则调用元素所在类的equals()方法
         * equals()返回true，添加失败
         * equals()返回false，添加成功 ——3

  > 对于添加成功的2、3而言，添加的元素与已经存在指定索引位置上的元素以链表的形式进行存储。

  jdk7：添加的元素放在数组中，指向原来的元素

  jdk8：原来的元素在数组中，指向添加的元素

  添加的对象的要求

  1. 类中必须重写hashCode()和equals()
  2. hashCode()和equals()保持一致性

* LinkedHashSet

  在向LinkedHashSet中添加数据时，每个数据维护了两个引用，指向前一个和后一个数据。因此，对于频繁的遍历操作，其效率高于HashSet。
  
* TreeSet

  1. 在向TreeSet中添加数据时，数据的类型必须一致。
  2. TreeSet存在两种排序方式：自然排序和定制排序
     * 自然排序时，对象需要实现Comparable接口
     * 定制排序需要向TreeSet构造器传入Comparator对象

### Map接口

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