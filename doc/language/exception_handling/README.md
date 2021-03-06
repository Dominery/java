# 异常处理

如果遇上处理的数据不符合预期、文件无法读取、硬件资源耗尽等状况，程序将会抛出异常。

### 异常的作用

如果没有异常，那么需要使用返回错误码的方式来表明处理不成功。这种方式有以下缺点：

1. 依赖全局共享可变状态来查找最近的错误。这使得单独理解代码的各个部分变得更加困难。
2. 容易出错，因为你要区分实际值和错误码的值。
3. 控制流和业务逻辑混在一起，这会使得代码更难以维护和单独测试。

java将异常作为一级语言特性，解决了这些问题，并带来如下好处：

1. 记录说明

   支持将异常作为方法签名的一部分。

2. 类型安全

   类型系统推算出你是否正在处理异常流。

3. 问题分离

   try/catch代码块分隔开了业务逻辑和异常恢复处理。问题是，异常作为一个语言特性也会增加复杂度。你可能了解在Java中区分两种异常类型的情况。

   > 1. 受检查的异常
   >
   >    这些错误是你期望能够恢复的。在Java中，必须声明一个方法，该方法可以抛出一个受检查异常列表。如果不声明，则必须为这个特定的异常提供合适的try/catch代码块。
   >
   > 2. 不受检查的异常
   >
   >    这些错误可以在程序执行期间的任何时候抛出。方法不必在其签名中显式地声明这些异常，并且调用者也不必像受检查的异常那样去显式地处理它们。

### 异常使用准则

1. 不要忽略异常

   忽略一个异常会无法诊断出问题的根源。如果没有一个明确的处理机制，那就抛出一个不受检查的异常来代替。

2. 不要捕获通用的异常

   尽可能多地捕获一个特定的异常，以便提高可读性，以及支持更多特定的异常处理。如果你捕获了通用的Exception，它还包含了RuntimeException。

3. 记录异常

   记录API级别的异常，包括不受检查的异常，这有助于排错。事实上，不受检查的异常会上报应该解决的问题的根源。

4. 注意特定实现的异常

   不要抛出特定实现的异常，因为它会打破你的API的封装性。

5. 异常与流程控制

   不要为了流程控制而使用异常。

### 异常的替代品

1. 使用null

   null没有向调用者提供有用的信息。它也很容易出错，因为你必须要明确地记得检查API的结果是否为null。

2. 空对象模式

   就是不要返回一个空引用来表示对象的缺失，而是返回一个实现了预期接口但其方法主体为空的对象。这种策略的优点是不用处理意外的空指针异常和一长串的空检查。

   尽管如此，这个模式也可能存在问题，因为你可能使用了一个对象隐藏掉了数据中的潜在问题，该对象只是忽略了真正的问题，从而使故障排除变得更加困难

3. Optional&lt;T&gt;

   Optional&lt;T&gt;提供了一组方法来明确地处理没有值的情况，这有助于减少bug的范围。它还允许各种Optional对象组合在一起，这些Optional对象可以作为不同API的返回类型返回。

4. Try&lt;T&gt;

   还有另外一种数据类型叫作Try&lt;T&gt;，它表示一个可能成功或失败的操作。在某种程度上，它类似于Optional&lt;T&gt;，但是处理的是操作而不是值。换句话说，Try&lt;T&gt;数据类型带来了相似代码可组合性的好处，这也有助于减少代码中的错误范围。