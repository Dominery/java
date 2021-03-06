# 线程的生命周期

jdk中通过`Thread.State`类定义了线程的状态。

* **新建：** 当一个Thread类或其子类的对象被声明并创建时，新生的线程对象处于新建状态

* **就绪：**处于新建状态的线程被start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源

* **运行：**当就绪的线程被调度并获得CPU资源时,便进入运行状态， run()方法定义了线程的操作和功能。

  线程调度的细节依赖于操作系统提供的服务。抢占式调度系统给每一个可运行线程一个时间片来执行任务。当时间片用完，操作系统剥夺该线程的运行权，并给另一个线程运行机会

* **阻塞：**在某种特殊情况下，被人为挂起或执行输入输出操作时，让出 CPU 并临时中止自己的执行，进入阻塞状态

* **死亡：**线程完成了它的全部工作或线程被提前强制性地中止或出现异常导致结束

![](../../../src/thread_lifecycle.png)



