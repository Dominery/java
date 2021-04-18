# Optional

 null引用在历史上被引入到程序设计语言中，目的是为了表示变量值的缺失。对null调用方法或者数据段会引发NullPointerException，为了避免这个异常，需要对获得的对象引用做检查，检查代码牺牲了代码本身的可读性，java.util.Optional\<T>的引入是为了解决这个问题。

### null带来的问题

* NullPointerException是目前Java程序开发中最典型的异常。
* 它让你的代码充斥着深度嵌套的null检查，代码的可读性糟糕透顶。
* null自身没有任何的语义，尤其是，它代表的是在静态类型语言中以一种错误的方式对缺失变量值的建模。
* Java一直试图避免让程序员意识到指针的存在，唯一的例外是：null指针。
* null并不属于任何类型，这意味着它可以被赋值给任意引用类型的变量。

### Optional简介

变量存在时，Optional类只是对类简单封装。变量不存在时，缺失的值会被建模成一个“空”的Optional对象，由方法Optional.empty()返回。Optional.empty()方法是一个静态工厂方法，它返回Optional类的特定单一实例。

使用Optional而不是null的一个非常重要而又实际的语义区别是方法声明表明了这里发生变量缺失是允许的。

引入Optional类的意图并非要消除每一个null引用。与此相反，它的目标是帮助你更好地设计出普适的API，让程序员看到方法签名，就能了解它是否接受一个Optional的值。

由于Optional类设计时就没特别考虑将其作为类的字段使用，所以它也并未实现Serializable接口

### Optional的使用

#### 创建Optional对象

1. 声明一个空的Optional对象

   ```java
   Optional<Person> p = Optional.empty();
   ```

2. 依据一个非空值创建Optional对象

   ```java
   Optional<Person> p = Optional.of(per);
   ```

   如果per值是null在创建Optional对象就会抛出NullPointerException。

3. 可接受null的Optional

   ```java
   Optional<Person> p = Optional.ofNullable(per);
   ```

#### 从Optional提取信息

* map

  Optional提供了一个map方法，map操作会将提供的函数应用于流的每个元素，如果Optional包含一个值，那函数就将该值作为参数传递给map，对该值进行转换。如果Optional为空，就什么也不做。

  ```java
  Optional<Person> p = Optional.ofNullable(per);
  Optional<String> pName = p.map(Person::getName);
  ```

* flatMap

  flapMap方法接受函数作为参数，该函数返回值为Optional对象，形成一个Optional对象的Optional对象，flapMap会用函数生成的Optional对象的内容替换Optional对象，将生成的Optional对象扁平化。

* filter

  如果Optional对象为空，它不做任何操作，反之，它就对Optional对象中包含的值施加谓词操作。如果该操作的结果为true，它不做任何改变，直接返回该Optional对象，否则就将该值过滤掉，将Optional的值置空。

#### 获取Optional变量的值

| 方法                                                | 说明                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| get()                                               | 如果变量存在，它直接返回封装的变量值，否则就抛出一个NoSuchElementException异常。 |
| orElse(T other)                                     | 它允许你在Optional对象不包含值时提供一个默认值。             |
| orElseGet(Supplier<? extends T> other)              | Supplier方法只有在Optional对象不含值时才执行调用。           |
| orElseThrow(Supplier<? extends X>exceptionSupplier) | 遭遇Optional对象为空时都会抛出一个异常                       |
| ifPresent(Consumer<? super T>)                      | 在变量值存在时执行一个作为参数传入的方法，否则就不进行任何操作。 |

