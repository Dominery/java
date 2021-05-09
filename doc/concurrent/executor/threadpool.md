# 线程池

一个线程池中包含许多准备运行的空闲线程。将Runnable对象交给线程池，就会有一个线程调用run方法。当run方法退出时，线程不会死亡，而是在池中准备为下一个请求提供服务。

使用线程池是为了减少并发线程的数目。创建大量线程会大大降低性能甚至使虚拟机崩溃。

### 线程池的创建

执行器（Executors）类有许多静态工厂方法用来构建线程池。

| 方法                             | 线程池特点                          |
| -------------------------------- | ----------------------------------- |
| newCachedThreadPool              | 必要时创建新线程，空闲线程会保留60s |
| newFixedThreadPool               | 线程数量固定的线程                  |
| newSingleThreadExecutor          | 只有一个线程的线程池                |
| newScheduledThreadPool           | 用于预定执行而构建的固定线程池      |
| newSingleThreadScheduledExecutor | 用于预定执行而构建的单线程池        |

newCached-ThreadPool方法构建了一个线程池，对于每个任务，如果有空闲线程可用，立即让它执行任务，如果没有可用的空闲线程，则创建一个新线程。

newFixedThreadPool方法构建一个具有固定大小的线程池。如果提交的任务数多于空闲的线程数，那么把得不到服务的任务放置到队列中。当其他任务完成以后再运行它们。

newSingleThreadExecutor是一个退化了的大小为1的线程池：由一个线程执行提交的任务，一个接着一个。

这3个方法返回实现了ExecutorService接口的ThreadPoolExecutor类的对象。

ScheduledExecutorService接口具有为预定执行（ScheduledExecution）或重复执行任务而设计的方法。它是一种允许使用线程池机制的java.util.Timer的泛化。

Executors类的newScheduledThreadPool和newSingleThreadScheduledExecutor方法将返回实现了ScheduledExecutorService接口的对象。可以预定Runnable或Callable在初始的延迟之后只运行一次。也可以预定一个Runnable对象周期性地运行。

### 线程池的使用

可用submit方法将一个Runnable对象或Callable对象提交给ExecutorService，调用submit时，会得到一个Future对象，可用来查询该任务的状态。

当用完一个线程池的时候，调用shutdown。该方法启动该池的关闭序列。被关闭的执行器不再接受新的任务。当所有任务都完成以后，线程池中的线程死亡。另一种方法是调用shutdownNow。该池取消尚未开始的所有任务并试图中断正在运行的线程。

invokeAny方法提交所有对象到一个Callable对象的集合中，并返回某个已经完成了的任务的结果。

invokeAll方法提交所有对象到一个Callable对象的集合中，并返回一个Future对象的列表，代表所有任务的解决方案。

ExecutorCompletionService可以用来进行排列结果，将其按可获得顺序保存。