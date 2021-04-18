# 使用流

本节将介绍Stream API的一些操作以及如何创建特殊的流。

流操作按照用户提供的Lambda或方法引用是否有内部可变状态分为无状态与有状态。

> 诸如map或filter等操作会从输入流中获取每一个元素，并在输出流中得到0或1个结果。这些操作一般都是无状态的。
>
> 诸如reduce、sum、max等操作需要内部状态来累积结果。不管流中有多少元素要处理，内部状态都是有界的。
>
> 诸如sort或distinct等操作从流中排序和删除重复项时都需要知道先前的历史，内部状态是无界的。

## 流的创建

流可以从集合、数值范围、值序列、数组、文件、生成函数创建流。

1. 集合

   通过调用集合的stream方法，将创建一个流。

   ```java
   List<Integer> intL = Arrays.as(1,2,3,4,5,6);
   Stream<Integer> intstream = intL.stream();
   ```

2. 数值范围

   Java 8引入了两个可以用于IntStream和LongStream的静态方法，帮助生成数值范围：range和rangeClosed。这两个方法都是第一个参数接受起始值，第二个参数接受结束值。但range是不包含结束值的，而rangeClosed则包含结束值。

   ```java
   IntStream intStream = IntStream.rangeClosed(1, 6);
   ```

3. 值序列

   静态方法Stream.of，通过显式值创建一个流。

   ```java
   Stream<Integer> intstream = Stream.of(1,2,3,4,5,6);
   ```

4. 数组

   静态方法Arrays.stream从数组创建一个流。

   ```java
   int[] nums = {1,2,3,4,5,6};
   Stream<Integer> intstream = Arrays.stream(nums);
   ```

5. 文件

   java.nio.file.Files中的很多静态方法都会返回一个流。静态的Files.lines方法会返回一个包含了文件中所有行的Stream。

   ```java
   try(Stream<String> lines = Files.lines(path)){
       
   }
   ```

6. 函数生成流

   Stream API提供了两个静态方法来从函数生成流：Stream.iterate和Stream.generate。

   * 迭代

     求斐波那契数列

     ```java
     Stream.iterate(new int[]{0,1},
                    b->new int[]{b[1],b[0]+b[1]}
                   ).
         limit(10).
         mapToInt(b->b[1]);
     ```

   * 生成

     generate不是依次对每个新生成的值应用函数的。它接受一个Supplier<T\>类型的Lambda提供新的值。

     Supplier<T\>供应源不一定是无状态的，用户可以创建存储状态的供应源，它可以修改状态，并在为流生成下一个值时使用。

     随机数流生成

     ```java
     Stream.iterate(Math::random).
         limit(20);
     ```

     

## 流的中间操作

### 筛选和切片

筛选与切片都是从流中选取一些元素的操作。

#### 筛选

筛选是选取元素符合特定要求的流，按照该元素的选取是否与已选取元素存在关系并且要符合筛选结果与流元素顺序无关的规则划分，则存在关系的选取只有筛选不同元素，不存在关系的选取为谓词筛选。

1. 谓词筛选

   Streams接口支持filter方法，该操作会接受一个谓词作为参数，并返回一个包括所有符合谓词的元素的流。

2. 筛选不同的元素

   一个叫作distinct的方法会返回一个元素各异（根据流所生成元素的hashCode和equals方法实现）的流。

#### 切片

切片是选取一定长度连续的流，其可以分为两种操作：一种是跳过切片区域之前的流，一种是获取指定长度的流。

1. 截断流

   流支持limit(n)方法，该方法会返回一个不超过给定长度的流。

2. 跳过流

   流还支持skip(n)方法，返回一个扔掉了前n个元素的流。如果流中元素不足n个，则返回一个空流。

### 映射

映射是一种获取流中元素信息并以此创建返回流中元素的操作。

1. 对流中每一个元素应用函数

   流支持map方法，它会接受一个函数作为参数。这个函数会被应用到每个元素上，并将其映射成一个新的元素。

2. 流的扁平化

   使用流时，flatMap方法接受一个函数作为参数，这个函数的返回值是另一个流。这个方法会应用到流中的每一个元素，最终形成一个新的流的流。但是flagMap会用流的内容替换每个新生成的流。换句话说，由方法生成的各个流会被合并或者扁平化为一个单一的流。

   flapMap与map相当于List中的addAll和add之间的关系。

### 其他操作

#### 排序

| 方法                   | 描述                                 |
| ---------------------- | ------------------------------------ |
| sorted()               | 产生一个新流，其中按照自然顺序排序   |
| sorted(Comparator com) | 产生一个新流，其中按照比较器顺序排序 |

#### 方法调用

peek方法会产生另一个流，它的元素与原来流中的元素相同，但是在每次获取一个元素时，都会调用一个函数。

## 流的终端操作

### 查找与匹配

查找与匹配都是获取到流中元素是否满足一定要求并返回相关信息的终端操作。

#### 匹配

匹配操作相当于与或非在流中的应用，都有短路属性，返回boolean值。

| 方法名      | 说明                               |
| ----------- | ---------------------------------- |
| anyMatch()  | 流中是否有一个元素能匹配给定的谓词 |
| allMatch()  | 流中是否所有元素匹配给定的谓词     |
| noneMatch() | 流中是否所有元素都不匹配给定的谓词 |

#### 查找

查找操作为避免返回null，返回值为Optional对象。

> Optional<T>类（java.util.Optional）是一个容器类，代表一个值存在或不存在。

| 方法名      | 说明                               |
| ----------- | ---------------------------------- |
| findAny()   | 返回流中一个任意元素的Optional对象 |
| findFirst() | 返回流中第一个元素的Optional对象   |

### 归约

归约是利用流中所有元素，将它们的信息汇总成一个值的终端操作。

#### reduce通用归约方法

该操作通过reduce方法实现。

reduce接受两个参数：

>  1. 一个初始值
>2. 一个Lambda来把两个流元素结合起来并产生一个新值

1. 元素求和

   ```java
   int sum = intergers.stream().
       reduce(0,(a,b)->a+b);
   Optional<Integer> sum = intergers.stream().
       reduce(Integer::sum);
   ```

2. 求元素个数

   ```java
   int num = intergers.stream().
       map(a->1).
       reduce(0,Integer::sum);
   ```

相比于逐步迭代求和，使用reduce的好处在于，这里的迭代被内部迭代抽象掉了，这让内部实现得以选择并行执行reduce操作。

#### 特化归约方法

| 方法名            | 描述                         |
| ----------------- | ---------------------------- |
| max(Comparator c) | 返回流中按照比较器顺序最大值 |
| min(Comparator c) | 同上                         |
| count()           | 返回流中元素总个数           |

## 特殊的流

### 数值流

如果流中的元素是数值，那么流操作会涉及到装箱拆箱，损耗性能，可以通过将流转化为数值流解决这个问题。

#### 原始类型流特化

Stream API提供了原始类型流特化接口将流转化为数值流(特化流)，数值流提供专门支持处理原始类型的方法。

1. 映射到数值流

   mapToInt、mapToDouble、mapToLong方法接收Stream<T\>返回一个特化流。

2. 转回非特化流

   使用boxed()方法或mapToObj()方法会将特化流转回一般流

3. Optional

   对于三种类型的数值流，Optional类提供相应的原始类型特化类：OptionalInt、OptionalDouble、OptionalLong。
