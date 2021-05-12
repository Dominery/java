# 线程

多线程程序具备以下优点：

1. 提高应用程序的响应，增强用户体验
2. 增强CPU的利用率
3. 改善程序结构，各个线程负责独自的功能，利于理解

何时应用多线程：

1. 程序需要同时执行两个或多个任务
2. 程序需要实现耗时的任务
3. 需要一些后台运行的程序

### 线程属性

线程的各种属性包括：线程优先级、守护线程、线程组以及处理未捕获异常的处理器。

#### 线程优先级

线程调度有时间片和抢占式两种。

Java中同优先级线程组成队列，使用时间片策略；对于不同优先级线程，使用抢占式策略。

每当线程调度器有机会选择新线程时，它首先选择具有较高优先级的线程。但是，线程优先级是高度依赖于系统的，实际表现可能不同。

* 线程优先级

  存在10个等级(1-10)，10的优先级最高，默认优先级5

  线程创建时继承父类的优先级，低优先级只是调度概率低。

* 方法调用

  * `getPriority()`：返回线程优先级
  * `setPriority()`：设置线程优先级

#### 守护线程

守护线程的唯一用途是为其他线程提供服务。当只剩下守护线程时，虚拟机就退出了。可以通过调用setDaemon方法将线程转变为守护线程。

守护线程应该永远不去访问固有资源，如文件、数据库，因为它会在任何时候甚至在一个操作的中间发生中断。

#### 未捕获异常处理器

非受查异常会导致线程终止。在线程死亡之前，可传播的异常被传递到一个用于未捕获异常的处理器。该处理器必须属于一个实现Thread.UncaughtExceptionHandler接口的类。

可以用setUncaughtExceptionHandler方法为任何线程安装一个处理器。也可以用Thread类的静态方法setDefaultUncaughtExceptionHandler为所有线程安装一个默认的处理器。