# 面向对象

面向对象是相对于面向过程而言的。面向过程，强调的是功能行为，以函数为最小单位，考虑怎么做。面向对象，将功能封装进对象，强调具备了功能的对象，以类/对象为最小单位，考虑谁来做。

面向对象分析方法分析问题的思路和步骤： 

1. 根据问题需要，选择问题所针对的现实世界中的实体。 
2. 从实体中寻找解决问题相关的属性和功能，这些属性和功能就形成了概念世界中的类。
3. 把抽象的实体用计算机语言进行描述，形成计算机世界中类的定义。即借助某种程序语言，把类构造成计算机能够识别和处理的数据结构。
4. 将类实例化成计算机世界中的对象。对象是计算机世界中解决问题的最终工具。

### 面向对象的思想

类(Class)和对象(Object)是面向对象的核心概念。

> 类是对一类事物的描述，是抽象的、概念上的定义
>
> 对象是实际存在的该类事物的每个个体，因而也称为实例(instance)。

面向对象程序设计的重点是类的设计，类的设计，其实就是类的成员的设计。

常见的类的成员有

* 属 性：对应类中的成员变量

* 行 为：对应类中的成员方法

### 对象创建和使用

创建对象语法： 类名 对象名 = new 类名();

使用“对象名.对象成员”的方式访问对象成员（包括属性和方法）

* 类的访问机制

  在一个类中的访问机制：类中的方法可以直接访问类中的成员变量。 

    > static方法访问非static，编译不通过。

    在不同类中的访问机制：先创建要访问类的对象，再用对象访问类中定义的成员

#### 对象的生命周期



* 内存解析

  堆（Heap），此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。这一点在Java虚拟机规范中的描述是：所有的对象实例以及数组都要在堆上分配。 

  通常所说的栈（Stack），是指虚拟机栈。虚拟机栈用于存储局部变量等。局部变量表存放了编译期可知长度的各种基本数据类型（boolean、byte、char 、 short 、 int 、 float 、 long 、double）、对象引用（reference类型，它不等同于对象本身，是对象在堆内存的首地址）。 方法执行完，自动释放。 

  方法区（Method Area），用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据