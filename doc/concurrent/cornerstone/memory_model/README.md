# 内存模型

在Java中，所有实例域、静态域和数组元素都存储在堆内存中，堆内存在线程之间共享

### 并发编程的两个关键问题

在并发编程中，需要处理两个关键问题：线程之间如何通信及线程之间如何同步（这里的线程是指并发执行的活动实体）。

> 通信是指线程之间以何种机制来交换信息。
>
> 同步是指程序中用于控制不同线程间操作发生相对顺序的机制。

在命令式编程中，线程之间的通信机制有两种：共享内存和消息传递。

1. 在共享内存的并发模型里，线程之间共享程序的公共状态，通过写-读内存中的公共状态进行隐式通信。在该模型里，同步是显式进行的。程序员必须显式指定某个方法或某段代码需要在线程之间互斥执行。
2. 在消息传递的并发模型里，线程之间没有公共状态，线程之间必须通过发送消息来显式进行通信。在该模型里，由于消息的发送必须在消息的接收之前，因此同步是隐式进行的。

Java的并发采用的是共享内存模型，Java线程之间的通信总是隐式进行，整个通信过程对程序员完全透明。