# Lambda

java8以前提供的实现行为参数化的方法很繁琐，java8中的新工具Lambda表达式解决了这个问题，使方法作为参数传递变得简洁。

### 使用Lambda

Lambda表达式是一种简洁的、有参数列表、函数主体、返回类型的匿名函数。

#### Lambda组成

Lambda表达式有三个部分。

```java
(Person p1,Person p2)->{
    return p1.getAge().compareTo(p2.getAge());
}
```

> 参数列表：`()`中的内容
>
> Lambda主体：`{}`中的内容，return的值即为Lambda的返回值。如果主体代码只有一句，可以省略花括号和return关键字。
>
> 箭头：->分隔开参数列表和Lambda主体的符号

#### 使用场合

Lambda表达式只能在接收函数式接口的地方使用。Lambda表达式能够直接以内联的形式为函数式接口的抽象方法提供实现，并把整个表达式作为函数式接口的实例。

Lambda表达式的参数列表及返回类型必须与函数式接口的抽象方法的签名一致。

#### 使用案例

Lambda表达式可以简化环绕执行模式的模板代码，使之变得简洁、灵活。

> 环绕执行模式是资源处理的固定模式，一般有三个流程：打开资源、处理资源、关闭资源。

其中资源打开与关闭的操作是资源处理的模板代码，可以通过行为参数化将资源处理的代码作为参数传入，实现一个方法可以对资源进行不同操作的效果。

```java
public void processInputStream(String filename,InputProcessor process)throws IOException{
    try(BufferedInputStream bis = new BufferedInputStream(new FileInputStream(filename))){
        process.process(bis);
    }
}

public interface InputProcessor{
    void process(BufferedInputStream bis)throws IOException;
}
```

任何符合()->{}签名的Lambda表达式都可以作为InputProcessor参数作为传递。

#### Lambda复合

java8的许多函数式接口提供了默认方法用以复合Lambda表达式。

* 比较器复合

  Comparator.comparing，根据提取用于比较的键值的Function来返回一个Comparator。

  1. 逆序

     ```java
     comparing(Person::getAge).reversed();
     ```

  2. 比较器链

     ```java
     comparing(Person::getAge)
         .reversed()
         .thenComparing(Person::name)
     ```

* 谓词复合

  谓词接口包括三个方法：negate、and和or，从而可以重用已有的Predicate来创建更复杂的谓词。

  1. 取反

     ```java
     Predicate<Person> agedPerson = person->person.getAge()>65;
     Predicate<Person> notAgedPerson = agedPerson.negate();
     ```

  2. 逻辑与

     ```java
     Predicate<Person> agedAndTallPerson = agedPerson.
         and(person->person.getHeight()>180);
     ```

  3. 逻辑或

     ```java
     Predicate<Person> agedOrTallPerson = agedPerson.
         or(person->person.getHeight()>180);
     ```

* 函数复合

  Function接口为此配了andThen和compose两个默认方法，它们都会返回Function的一个实例，从而可以实现该接口的Lambda表达式复合。

  1. andThen

     andThen方法要求Function对应的Lambda表达式传入参数的类型与返回值的类型相同。
  
  2. compose
  
     该方法处理表达式的顺序与andThen相反。

 

### java8对于Lambda的支持

#### 函数式接口

> 函数式接口是只定义了一个抽象方法的接口。

方法可以通过将参数更改为函数式接口来接收Lambda表达式。

```java
public interface Comparator<T>{
    int compare(T o1,T o2);
}
```

函数式接口可以带有@FunctionalInterface的标注，如果一个接口有该标注，则编译器会检查此接口是否是函数式接口。

下面将介绍java.util.function包中的几个函数式接口。

1. Predicate

   java.util.function.Predicate\<T\>接口定义了一个名叫test的抽象方法，它接受泛型T对象，并返回一个boolean。

2. Consumer

   java.util.function.Consumer\<T\>定义了一个名叫accept的抽象方法，它接受泛型T的对象，没有返回（void）。

3. Function

   java.util.function.Function<T, R>接口定义了一个叫作apply的方法，它接受一个泛型T的对象，并返回一个泛型R的对象。

为了避免原始数据类型拆箱装箱的性能损耗，java提供了专门的函数式接口，如IntPredicate。针对专门的输入参数类型的函数式接口的名称一般都要加上对应的原始类型前缀。

任何函数式接口都不允许抛出受检异常。

#### 类型检查

Lambda的类型是从使用Lambda的上下文推断出来的。

类型检查过程如下：

1. 找到接收函数式接口的方法声明

2. 确定目标类型，如果函数式接口有类型参数，明确该类型参数

   > 目标类型是指Lambda表达式所在上下文环境的类型。比如，将Lambda表达式赋值给一个局部变量，或传递给一个方法作为参数，局部变量或方法参数的类型就是Lambda表达式的目标类型。

3. 找到函数式接口的抽象方法签名

4. 检查该方法签名与Lambda表达式是否匹配

> 如果函数式接口的抽象方法返回类型要求void，Lambda表达式可以有其他类型的返回值。

#### 类型推断

java编译器可以从上下文中获知目标类型，推断出Lambda表达式的参数列表中参数的类型，因而使用者可以省略Lambda的参数类型声明。

* 如果只有一个可能的目标类型，由相应函数接口里的参数类型推导得出

* 如果有多个可能的目标类型，由最具体的类型推导得出
* 如果有多个可能的目标类型且最具体的类型不明确，则需人为指定类型。

#### 局部变量

Lambda可以使用定义其自身的方法体内的局部变量，实现类似闭包的操作，但有一个限制：不能修改定义Lambda的方法的局部变量的内容，实例变量可以。

> 闭包就是一个函数的实例，且它可以无限制地访问那个函数的非本地变量。

Lambda可以捕获局部变量，这些局部变量如果不是final，则该变量在既成事实上是final。

> 既成事实上的final是指只能给该变量赋值一次。换句话说，Lambda表达式引用的是值，而不是变量。

这个限制的原因在于避免线程不安全问题，局部变量存储在栈中，线程间不能共享，实例变量存储在堆中，线程间共享。

### 方法引用

方法引用可以重复使用现有的方法定义，并像Lambda一样传递它们。方法引用是Lambda的语法糖，本质上依旧是Lambda。

* 方法引用的基本思想

  如果一个Lambda代表的只是“直接调用这个方法”，那最好还是用名称来调用它，而不是去描述如何调用它。

* 使用示例

    ```java
    Persons.sort(Comparing(Person::getAge));
    Persons.sort(Comparing(p1::getAge));
    ```

#### 分类

1. 静态方法的方法引用——>类 : : 静态方法名
2. 任意类型实例方法的方法引用——>类  :  : 实例方法名
3. 现有对象的实例方法的方法引用——>对象  :  : 实例方法名

1、3中方法引用的签名必须和函数式接口抽象方法匹配。如果使用2，则实例方法参数列表个数要比抽象方法参数列表少1，抽象方法的第一个参数作为实例方法的调用者。

```java
Comparator<String> s = String::compareTo;
```

#### 构造函数引用

对于一个现有构造函数，可以利用它的名称和关键字new来创建它的一个引用：ClassName::new。

| 构造函数参数 | 函数式接口        |
| ------------ | ----------------- |
| 无参         | Supplier\<T\>     |
| 单参         | Function\<T,R>    |
| 双参         | BiFunction<T,U,R> |

如果是双参以上的构造函数，需要自定义函数式接口。

