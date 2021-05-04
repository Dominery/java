# 线程的同步

线程同步是为了解决线程安全问题。线程安全问题是线程操作的数据被破坏。

线程安全问题出现的原因：

1. 多个线程共享数据

2. 数据操作过程不是原子操作

   > 一个线程操作数据的过程还未结束，另一个线程参与进来，操作数据

线程安全问题可以通过使同一时刻只有一个线程能够操作数据的方法得到解决。

> 如果向一个变量写入值，而这个变量接下来可能会被另一个线程读取，或者，从一个变量读值，而这个变量可能是之前被另一个线程写入的，此时必须使用同步

### java同步方法

#### 同步监视器

在java中，通过同步机制解决线程的安全问题，共有三种实现方法。根据`synchronized`关键字实现同步有两种，第三种为`Lock`实现。

1. 同步代码块

   ```java
   synchronized(同步监视器){
       //code need to be synchronized 
   }
   ```

   * 操作共享数据的代码需要被同步
   * 同步监视器，锁，任何一个类的对象，都可以充当锁
   * 共享数据的多个线程必须共用同一把锁
   * 如果使用`Runnable`实现多线程，锁可以是`this`；如果使用继承实现多线程，锁可以是`object.class`

2. 同步方法

   如果操作共享数据的代码完整声明在一个方法中，可以将该方法声明为同步的。

   ```java
   private synchronized void processData(){}
   ```

   * 同步方法需要同步监视器，不需要显示声明
   * 实现多线程，`synchronized`修饰非静态方法，同步监视器是`this`；如果使用继承实现多线程，`synchronized`修饰静态方法，同步监视是`object.class`
   * 内部对象锁只有一个相关条件，调用wait相当于调用条件对象的await
   * 方法局限性
     1. 不能中断一个正在试图获得锁的线程。
     2. 试图获得锁时不能设定超时。
     3. 每个锁仅有单一的条件，可能是不够的。

3. Lock锁

   ```java
   class MyThread implements Runnable{
       private ReentrantLock lock = new ReentrantLock();
       public void run(){
           // code
           try{
               lock.lock();
               //code sharing data for running
           }finally{
               lock.unlock();
           }
       }
   }
   ```
   
   从jdk5.0开始，java提供锁对象来实现同步。`java.util.concurrent.locks.Lock`接口是控制多个线程对共享资源进行访问的工具。
   
   `ReentrantLock`类实现`Lock`，在线程安全的控制中，经常被使用。
   
   **锁测试**
   
   线程在调用lock方法来获得另一个线程所持有的锁的时候，很可能发生阻塞。tryLock方法试图申请一个锁，在成功获得锁后返回true，否则，立即返回false。可以对该方法传递超时参数。
   
   **读/写锁**
   
   java.util.concurrent.locks包定义了两个锁类，ReentrantLock类和ReentrantReadWriteLock类。如果很多线程从一个数据结构读取数据而很少线程修改其中数据的话，可以使用后者。
   
   1. 创建锁对象
   2. 获取读写锁
   3. 对获取方法加读锁，对修改方法加写锁

`synchronized`和`Lock`的异同：

* 共同点：都用来解决线程安全问题
* 不同：`synchronized`机制自动管理同步监视器，`Lock`需要手动启动和结束同步

#### 条件对象

一个锁对象可以有一个或多个相关的条件对象。可以用对Lock对象调用newCondition方法获得一个条件对象。每个条件对象管理那些已经进入被保护的代码段但还不能运行的线程。

当条件不满足时，调用条件对象await方法，使当前线程阻塞，放弃锁。

等待获得锁的线程和调用await方法的线程存在本质上的不同。一旦一个线程调用await方法，它进入该条件的等待集。当锁可用时，该线程不能马上解除阻塞。相反，它处于阻塞状态，直到另一个线程调用同一条件上的signalAll方法时为止。

> 调用signalAll不会立即激活一个等待线程。它仅仅解除等待线程的阻塞，以便这些线程可以在当前线程退出同步方法之后，通过竞争实现对对象的访问。

当一个线程调用await时，它没有办法重新激活自身。它寄希望于其他线程。如果没有其他线程来重新激活等待的线程，它就永远不再运行了。这将导致死锁（deadlock）现象。

#### 其他同步方法

1. volatile关键字

   volatile关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。假设对共享变量除了赋值之外并不完成其他操作，那么可以将这些共享变量声明为volatile。

2. 原子性

   java.util.concurrent.atomic包中有很多类使用了很高效的机器级指令（而不是使用锁）来保证其他操作的原子性。

   如果有大量线程要访问相同的原子值，性能会大幅下降，因为更新需要太多次重试。Java SE 8提供了LongAdder和LongAccumulator类来解决这个问题。

   * LongAdder包括多个变量（加数），其总和为当前值。可以有多个线程更新不同的加数，线程个数增加时会自动提供新的加数。通常情况下，只有当所有工作都完成之后才需要总和的值，对于这种情况，这种方法会很高效。性能会有显著的提升。

   * LongAccumulator将这种思想推广到任意的操作。在构造器中，可以提供这个操作以及它的零元素。要加入新的值，可以调用accumulate。调用get来获得当前值。

     ```java
     LongAccumulator addr = new LongAccumulator(Long::sum,0);
     //In some thread
     addr.accumulate(value)
     ```

     一般地，这个操作必须满足结合律和交换律。

### 同步的使用

* 最好既不使用Lock/Condition也不使用synchronized关键字。在许多情况下你可以使用java.util.concurrent包中的一种机制，它会为你处理所有的加锁。
* 如果synchronized关键字适合你的程序，那么请尽量使用它，这样可以减少编写的代码数量，减少出错的几率。
* 如果特别需要Lock/Condition结构提供的独有特性时，才使用Lock/Condition。

### 死锁问题

> 死锁：不同线程占用对方需要的同步资源，都在等待对方放弃自己需要的同步资源，这种情况称为死锁。

解决方法：

1. 避免锁的嵌套
2. 减少同步资源的定义
3. 算法

### 线程局部变量

如果在线程中要避免共享变量，那么需要使用ThreadLocal辅助类为各个线程提供各自的实例。。

```java
ThreadLocal<SimpleDateFormat> dateFormat = ThreadLocal.withInitial(()->new SimpleDateFormat("yyyy-MM-dd"));
```

在一个给定线程中首次调用get时，会调用initialValue方法。在此之后，get方法会返回属于当前线程的那个实例。