# 线程的同步

线程同步是为了解决线程安全问题。线程安全问题是线程操作的数据被破坏。

线程安全问题出现的原因：

1. 多个线程共享数据
2. 一个线程操作数据的过程还未结束，另一个线程参与进来，操作数据

线程安全问题可以通过使同一时刻只有一个线程能够操作数据的方法得到解决。

### java同步方法

在java中，通过同步机制解决线程的安全问题，共有三种实现方法。

根据`synchronized`关键字实现同步分为两种：

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

从jdk5.0开始，java提供锁对象来实现同步。`java.util.concurrent.locks.Lock`接口是控制多个线程对共享资源进行访问的工具。

`ReentrantLock`类实现`Lock`，在线程安全的控制中，经常被使用。

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

`synchronized`和`Lock`的异同：

* 共同点：都用来解决线程安全问题
* 不同：`synchronized`机制自动管理同步监视器，`Lock`需要手动启动和结束同步

### 死锁问题

> 死锁：不同线程占用对方需要的同步资源，都在等待对方放弃自己需要的同步资源，这种情况称为死锁。

解决方法：

1. 避免锁的嵌套
2. 减少同步资源的定义
3. 算法