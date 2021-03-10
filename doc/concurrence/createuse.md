# 线程创建与使用

JVM允许程序执行多线程，用户可以借助`java.lang.Thread`类实现多线程的创建。

### 创建方式

`Thread`类可以通过两种方式创建多线程。

1. 继承`Thread`方式

   ```java
   //1.extend the Tread class
   class MyThread extends Thread{
       //2.ovrride run function of Thread
       public void run(){
           for(int i=0;i<100;i++){
               System.out.println(i);
           }
       }
   }
   
   public class ThreadTest{
       public static void main(String[] args){
           //3.create object of subclass of Thread
           MyThread t1 = new MyThread();
           //4.start the thread
           t1.start();// start()will start the thread and then make the thread execute the run()
       }
   }
   ```

   * 不能通过直接调用`run()`方法启动线程
   * 线程只能启动一次

2. 匿名子类方式

   ```java
   public class ThreadTest{
       public static void main(String[] args){
           new Thread{
               public void run(){
                   for(int i=0;i<100;i++){
                       System.out.println(i);
                   }
               }
           }.start();
       }
   }
   ```

### Thread方法

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

