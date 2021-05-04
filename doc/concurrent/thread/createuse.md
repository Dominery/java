# 线程创建与使用

JVM允许程序执行多线程，用户可以借助`java.lang.Thread`类实现多线程的创建。

### 创建方式

线程创建一共有4种方式。

`Thread`类本身可以通过两种方式创建多线程。在jdk5.0后新增两种线程创建方法。

1. 继承`Thread`方式

   ```java
   //1.extend the Tread class
   class MyThread extends Thread{
       //2.ovrride run function of Thread
       public void run(){
           //code for running
       }
   }
   
   public class ThreadTest{
       public static void main(String[] args){
           //3.create object of subclass of Thread
           MyThread t1 = new MyThread();
           //4.start the thread
           t1.start();// start()will start the thread and then make the thread execute the run()
           
           //another choice,use anonymous class to replace inheritance
           new Thread{
               public void run(){
                   //code for running
               }
           }.start();
       }
   }
   ```

   * 不能通过直接调用`run()`方法启动线程
   * 线程只能启动一次

2. 实现`Runnable`接口方式

   ```java
   //1.create a class which implements Runnable
   class MyThread1 implements Runnable{
       //2.complete the run method
       public void run(){
           //code for running
       }
   }
   
   public class ThreadTest{
       public static void main(String[] args){
           //3.create an object of the class
           MyThread1 t1 = new MyThread1();
           //4.transfer the object to Thread constructor
           new Thread(t1).start();//5.start thread
           
           //another choice,using lambda expression to replace the interface
           new Thread(()->{
               //code here for executing
           }).start();
       }
   }
   ```

3. 实现`Callable`接口

    ```java
    class NumThread implements Callable{
        public Object call() throws Exception{
            return "";
        }
    }
    public class ThreadTest{
        NumThread numThread = new NumThread();
        FutureTask futureTask = new FutureTask(numThread);
        new Thread(futureTask).start();
        try{
            futureTask.get();
        }catch(Exception e){
            e.printStackTrace();
        }
    }
    ```
    
    `Callable`接口实现方式比`Runnable`接口实现方式更强大，可以有返回值、能够抛出异常、支持泛型。
    
4. 线程池

    如果经常创建和销毁线程，对程序性能的影响很大。

    这种问题可以通过提前创建多个线程，放入线程池，使用时获取，使用完放回的方式得到解决。

    ```java
    public class ThreadPool{
        public static void main(String[] args){
            //create thread pool
            ExecutorService service = Exectors.newFixedThreadPool(10);
            // transfer an object interfaced Runnable
            service.execute(()->{});
            //transfer an object interfaced Callable
            service.submit(()->"hello");
            service.shutdown();//close the pool
        }
    }
    ```

在开发中，如果在前两种方法选择，一般使用第二种方式实现多线程，原因如下：

1. 如果使用第一种继承方式，由于java单继承机制，要求类的父类只能是`Thread`，但是该类可能存在自身的继承链，`Thread`与其没有必然的`is-a`关系。
2. 如果使用第二种接口方式，由于同一个对象可以多次被传入`Thread`构造器中，多个线程共同调用同一个对象的`run`方法，实现了多个线程数据共享的功能。

前两种方法的联系：

* `Thread`类本身实现了`Runnable`接口。

	> public class Thread implements Runnable

* 两种方法都需要重写`run`方法，将线程的执行逻辑写在`run`方法中。

开发中一般使用线程池的方式创建多线程，原因如下：

1. 提高响应速度
2. 降低资源消耗
3. 便于线程管理

### 线程使用

#### Thread方法

| 常用方法        | 方法说明                                                     |
| --------------- | ------------------------------------------------------------ |
| start()         | 启动当前线程，调用run()方法                                  |
| run()           | 存放需要线程执行的代码                                       |
| currentThread() | 静态方法，返回执行当前代码的线程名字                         |
| getName()       | 获取当前线程的名字                                           |
| setName()       | 设置线程的名字                                               |
| yield()         | 释放当前CPU的执行权                                          |
| join()          | 在线程a中调用线程b的join(),线程a进入阻塞状态，直到线程b执行完，线程a解阻塞 |
| sleep()         | 使线程阻塞固定时间                                           |
| isAlive()       | 判断当前线程是否存活                                         |

