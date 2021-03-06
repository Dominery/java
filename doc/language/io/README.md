# IO流

I/O是input和output的缩写，是处理设备之间的数据传输。

> 如果数据从网络或者硬盘等外部存储设备中流向程序(内存)，那么这种数据传输方式为input。
>
> 如果数据从程序(内存)中流向外部存储设备，那么这种数据传输方式为output。

Java程序中，数据的输入/输出操作以“流“`stream`的方式进行。

### 流的体系

java中io流相关的类都是由四个抽象基类派生的，其名称都是以父类名为后缀。

|        | 字节流       | 字符流 |
| ------ | ------------ | ------ |
| 输入流 | InputStream  | Reader |
| 输出流 | OutputStream | Writer |

> 在Java API中，可以从其中读入一个字节序列的对象称做输入流，而可以向其中写入一个字节序列的对象称做输出流。

### 流的特性

流的read和write方法在执行时都将阻塞，直至字节确实被读入或写出。available方法使我们可以去检查当前可读入的字节数量，从而避免阻塞。

完成对输入/输出流的读写时，应该通过调用close方法来关闭它，这个调用会释放掉十分有限的操作系统资源。关闭一个输出流的同时还会冲刷用于该输出流的缓冲区：所有被临时置于缓冲区中，以便用更大的包的形式传递的字节在关闭输出流时都将被送出。也可以调用flush方法冲刷输出。