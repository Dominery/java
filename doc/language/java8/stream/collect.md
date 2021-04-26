# 流收集数据

流的终端操作会消耗流，可以调用collect方法，通过传入的Colletor收集器将数据归约成一个结果。

一般来说，Collector会对元素应用一个转换函数（很多时候是不体现任何效果的恒等转换，例如toList），并将结果累积在一个数据结构中，从而产生这一过程的最终输出。

### 预定义收集器

Collectors实用类提供了很多静态工厂方法，可以方便地创建常见收集器的实例。它们主要提供了三大功能： 将流元素归约和汇总为一个值、元素分组、元素分区。

#### 归约与汇总

归约操作是对流中对象应用某条件返回一个结果的操作。

另一个常见的返回单个值的归约操作是对流中对象的一个数值字段求和。这种操作被称为汇总操作

| 静态方法          | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| counting()        | 计算流中数据的个数                                           |
| maxBy(Comparator) | 接收一个比较器参数，以Optional形式返回流中最大的元素         |
| minBy(Comparator) | 同maxBy                                                      |
| summingInt()      | 接收把对象映射为int的函数，对转换后的int进行求和             |
| averagingInt()    | 接收转换函数，返回平均数                                     |
| summarizingInt()  | 接收转换函数，返回sum、average、min、max信息的IntSummaryStatics对象 |
| joining()         | 对流中每个对象应用toString方法并将字符串拼接起来，可以使用其重载方法传入分隔符 |

上述方法返回的所有收集器，都是一个可以用reducing工厂方法定义的归约过程的特殊情况而已。

reducing需要三个参数。

1. 归约操作的起始值，也是流中没有元素时的返回值。
2. 转换函数，将对象映射成另一个类型。
3. 累积函数，将两个项目累积成一个同类型的值。

单参数的reducing是一种特殊情况，它把流中的第一个元素作为起点，把恒等函数作为一个转换函数。

reduce方法旨在把两个值结合起来生成一个新值，它是一个不可变的归约。与此相反，collect方法的设计就是要改变容器，从而累积要输出的结果。如果使用reduce方法改变容器，会导致并行时线程不安全的问题。

#### 分组

分组是依据流中元素的一个或多个属性对其分类的操作。分组操作的结果是一个Map，把分组函数返回的值作为映射的键，把流中所有具有这个分类值的项目的列表作为对应的映射值。

Collectors.groupingBy工厂方法返回的收集器可以进行分组操作。

1. Collectors.collectingAndThen

   把收集器的结果转换为另一种类型，这个工厂方法接受两个参数——要转换的收集器以及转换函数，并返回另一个收集器。

2. 多级分组

   实现多级分组，我们可以使用一个由双参数版本的Collectors.groupingBy工厂方法创建的收集器，它除了普通的分类函数之外，还可以接受collector类型的第二个参数。普通的单参数groupingBy(f)（其中f是分类函数）实际上是groupingBy(f, toList())的简便写法。

#### 分区

分区是分组的特殊情况：由一个谓词（返回一个布尔值的函数）作为分类函数，它称分区函数。

Collectors.partitioningBy方法返回的收集器可以进行分区操作。

分区的好处在于保留了分区函数返回true或false的两套流元素列表。和groupingBy收集器类似，partitioningBy收集器也可以结合其他收集器使用。尤其是它可以与第二个partitioningBy收集器一起使用来实现多级分区。

#### 静态方法表

上面讨论的收集器都是对Collector接口的实现。

| 工厂方法          | 返回类型              | 用法说明                                                   |
| ----------------- | --------------------- | ---------------------------------------------------------- |
| toList            | List\<T>              | 把流中的所有元素收集到一个List中                           |
| toSet             | Set\<T>               | 把流中的所有元素收集到一个Set                              |
| toCollection      | Collection\<T>        | 把流中所有元素收集到给定的供应源创建的集合                 |
| counting          | Long                  | 计算流中元素的个数                                         |
| summingInt        | Integer               | 对流中元素的一个整数属性求和                               |
| averagingInt      | Double                | 计算流中元素一个整数属性的平均值                           |
| summarizingInt    | IntSummaryStatistics  | 收集关于流中元素一个整数属性的统计信息                     |
| joining           | String                | 连接对流中元素调用toString方法所生成的字符串               |
| maxBy             | Optional\<T>          | 包裹流中按指定比较器选出的最大元素                         |
| minBy             | Optional\<T>          | 包裹流中按指定比较器选出的最小元素                         |
| reducing          | 归约操作产生的类型    | 从初始值开始，利用转换函数对流中元素逐个转换后应用累积函数 |
| collectingAndThen | 转换函数返回的类型    | 包裹另一个收集器，对其结果应用转换函数                     |
| groupingBy        | Map<K,List\<T>>       | 根据元素一个属性对流中元素进行分组，将属性值作为结果的键   |
| partitioningBy    | Map<Boolean,List\<T>> | 根据对流中每个元素应用谓词的结果进行分区                   |

### 收集器接口

可以为Collector接口提供自己的实现，从而自由地创建自定义归约操作。

Collector接口的签名与方法声明。

```java
public interface Collector<T,A,R>{
    Supplier<A> supplier();
    BiConsumer<A,T> accumulator();
    Function<A,R> finisher();
    BinaryOperator<A> combiner();
    Set<Characteristics> characteristics();
}
```

> T是流中要收集的项目的泛型。A是累加器的类型，累加器是在收集过程中用于累积部分结果的对象。R是收集操作得到的对象（通常但并不一定是集合）的类型。

#### 接口方法

以下将说明Collector的方法，辅以toList收集器相关方法的实现。

1. supplier方法

   supplier方法建立新的结果容器，必须返回一个结果为空的Supplier，也就是一个无参数函数，在调用时它会创建一个空的累加器实例，供数据收集过程使用。

   ```java
   public Supplier<List<T>> supplier(){
       return ArrayList::new;
   }
   ```

2. accumulator方法

   accumulator方法将元素添加到结果容器，会返回执行归约操作的函数。当遍历到流中第n个元素时，这个函数执行时会有两个参数：保存归约结果的累加器（已收集了流中的前n-1个项目），还有第n个元素本身。该函数将返回void，因为累加器是原位更新，即函数的执行改变了它的内部状态以体现遍历的元素的效果。

   ```java
   public BiCosumer<List<T>,T>accumulator(){
       return List::add;
   }
   ```

3. finisher方法

   在遍历完流后，finisher方法必须返回在累积过程的最后要调用的一个函数，以便将累加器对象转换为整个集合操作的最终结果。

   ```java
   public Funcion<List<T>,List<T>>finisher(){
       return Function.identity();
   }
   ```

4. combiner方法

   combiner方法会返回一个供归约操作使用的函数，它定义了对流的各个子部分进行并行处理时，各个子部分归约所得的累加器要如何合并。

   ```java
   public BinaryOperator<List<T>> combiner(){
       return (list1,list2)->{
           list1.addAll(list2);
           return list1;
       }
   }
   ```

5. characteristics方法

   characteristics会返回一个不可变的Characteristics集合，它定义了收集器的行为——尤其是关于流是否可以并行归约，以及可以使用哪些优化的提示。

   >  UNORDERED——归约结果不受流中项目的遍历和累积顺序的影响。
   >
   >  CONCURRENT——accumulator函数可以从多个线程同时调用，且该收集器可以并行归约流。如果收集器没有标为UNORDERED，那它仅在用于无序数据源时才可以并行归约。
   >
   >  IDENTITY_FINISH——这表明完成器方法返回的函数是一个恒等函数，可以跳过。这种情况下，累加器对象将会直接用作归约过程的最终结果。这也意味着，将累加器A不加检查地转换为结果R是安全的。

   ```java
   public Set<Characteristics> characteristics(){
       return Collections.unmodifiableSet(EnumSet.of(
       IDENTITY_FINISH,CONCURRENT));
   }
   ```

   

#### 自定义收集

对于IDENTITY_FINISH的收集操作，还有一种方法可以得到同样的结果而无需从头实现新的Collectors接口。Stream有一个重载的collect方法可以接受另外三个函数——supplier、accumulator和combiner，其语义和Collector接口的相应方法返回的函数完全相同。