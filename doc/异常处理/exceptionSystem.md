# 异常体系

异常处理的任务就是将控制权从错误产生的地方转移给能够处理这种情况的错误处理器。

所有的异常都是由Throwable继承而来，但在下一层立即分解为两个分支：Error和Exception。

* Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误。Error不编写代码进行处理。

* Exception层次结构又分解为两个分支：一个分支派生于RuntimeException；另一个分支包含其他异常。

  划分两个分支的规则是：由程序错误导致的异常属于RuntimeException，称为运行时异常；而程序本身没有问题，但像I/O错误这类问题导致的异常属于编译时异常。

Java语言规范将派生于Error类或RuntimeException类的所有异常称为非受查（unchecked）异常，所有其他的异常称为受查（checked）异常。

