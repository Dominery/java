# 组合式异步编程

执行比较耗时的操作时，尤其是那些依赖一个或多个远程服务的操作，使用异步任务可以改善程序的性能，加快程序的响应速度。

## Future接口

Future接口在Java 5中被引入，Future对于充分利用多核处理能力是非常有益的，因为它允许一个任务在一个新的核上生成一个新的子线程，新生成的任务可以和原来的任务同时运行。原来的任务需要结果时，它可以通过get方法等待Future运行结束（生成其计算的结果值）。

### Future的使用

要使用Future，只需要将耗时的操作封装在一个Callable对象中，再将它提交给ExecutorService，执行其他任务后，通过调用get获取耗时操作的结果。

```java
ExecutorService executor = Executors.newCachedThreadPool();
Future<Double> future = executor.submit(new Cllable<Double>(){
    public Double call(){
      return doSomeLongComputation();  
    };
});
doSomethingElse();

try{
    Double result = future.get(1,TimeUnit.SECONDS);
}catch(ExecutionException ee){
    //计算过程中抛出异常
}catch(InterruptedException ie){
    //当前线程在等待过程中被中断
}catch(TimeoutException te){
    //在Future对象完成之前已超时
}
```

使用Future的第二种是使用FutureTask具体实现类，FutureTask包装器是一种非常便利的机制，可将Callable转换成Future和Runnable，它同时实现二者的接口。将FutureTask对象提交给一个线程，启动线程后开始运算。

#### 接口方法

| 方法     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| get()    | 阻塞直至计算完成，如果中断，抛出InterruptedException。可以传入超时参数，如果运算超时，抛出TimeoutException。 |
| isDone() | 如果计算未完成，返回false，否则返回true                      |
| cancel() | 如果计算还没有开始，它被取消且不再开始。                     |

### 局限性

Future很难实现下述操作：

1.  将两个异步计算合并为一个
2.  等待Future集合中的所有任务都完成。
3. 仅等待Future集合中最快结束的任务完成（有可能因为它们试图通过不同的方式计算同一个值），并返回它的结果。
4. 通过编程方式完成一个Future任务的执行（即以手工设定异步操作结果的方式）
5. 当Future的完成事件发生时会收到通知，并能使用Future计算的结果进行下一步的操作，不只是简单地阻塞等待操作的结果

## ComputableFuture类

CompletableFuture类（它实现了Future接口）利用Java 8的新特性以更直观的方式解决了Future接口的局限性。

### 实现异步API

异步API的实现可以通过用户编程和调用工厂方法两种方式创建，一般使用工厂方法创建。

#### 编程创建CompletableFuture对象

* 将同步方法转换为异步方法

    在方法名称后添加Async，修改返回值为Future\&lt;T>类型。

    Future是一个暂时还不可知值的处理器，这个值在计算完成后，可以通过调用它的get方法取得。因为这样的设计，getPriceAsync方法才能立刻返回，给调用线程一个机会，能在同一时间去执行其他有价值的计算任务。

    ```java
    public Future<Double> getPriceAsync(String product){
        CompletableFuture<Double> futurePrice = new CompletableFuture<>();
        new Thread(()->{
            double price = calculatePrice(product);
            futurePrice.complete(price);
        }).start();
        return futurePrice;
    }
    ```

* 错误处理

    如果计算过程中产生了错误，用于提示错误的异常会被限制在试图计算商品价格的当前线程的范围内，最终会杀死该线程，而这会导致等待get方法返回结果的客户端永久地被阻塞。客户端可以使用重载版本的get方法，它使用一个超时参数来避免发生这样的情况。

    为了让客户端能了原因，你需要使用CompletableFuture的completeExceptionally方法将导致CompletableFuture内发生问题的异常抛出。

#### 使用工厂方法创建

CompletableFuture类自身提供了大量精巧的工厂方法，使用这些方法能更容易地完成整个流程，还不用担心实现的细节。

```java
public Future<Double> getPriceAsync(String product){
    return CompletableFuture.supplyAsync(()->calculatePrice(product));
}
```

supplyAsync方法接受一个生产者（Supplier）作为参数，返回一个CompletableFuture对象，该对象完成异步执行后会读取调用生产者方法的返回值。

相对于并行流，CompletableFuture具有一定的优势，因为它允许你对执行器（Executor）进行配置，尤其是线程池的大小，让它以更适合应用需求的方式进行配置，满足程序的要求，而这是并行流API无法提供的。

* CompletableFuture流处理

    CompletableFuture类中的join方法和Future接口中的get有相同的含义，并且也声明在Future接口中，它们唯一的不同是join不会抛出任何检测到的异常。

    ```java
    public List<String> findPrices(String product){
        List<CompletableFuture<String>>priceFuture = 
            shops.stream()
            .map(shop->CompletableFuture.supplyAsync(
                        ()->shop.getName()+"price is"+
                            shop.getPrice(product)))
            .collect(Collectors.toList());
        return priceFuture.stream()
                    .map(CompletableFuture::join)
                    .collect(Collectors.toList());
    }
    ```

*  使用定制的执行器

    如果要配置CompletableFuture线程池大小，需要将执行器作为第二个参数传递给supplyAsync工厂方法。

    如果线程池中线程的数量过多，最终它们会竞争稀缺的处理器和内存资源，浪费大量的时间在上下文切换上。反之，如果线程的数目过少，处理器的一些核可能就无法充分利用。

    线程池大小与处理器的利用率之比可以使用下面的公式进行估算：

    > N~threads~=N~CPU~*U~CPU~*(1+W/C)
    >
    > N~CPU~是处理器的核的数目，可以通过Runtime.getRuntime().availableProce-ssors()得到
    >
    > U~CPU~是期望的CPU利用率（该值应该介于0和1之间）
    >
    > W/C是等待时间与计算时间的比率

    ```java
    private final Executor executor = Executors.newFixedThreadPool(Math.min(shops.size(),100),
                                           new ThreadFactory(){
          public Thread newThread(Runnable r){
              Thread t = new Thread(r);
              t.setDaemon(true);
              return t;
          }
    });
    ```

    使用setDaemon方法设置线程为守护线程，意味着程序退出时它也会被回收。

* 使用场景

    对集合进行并行计算有两种方式：

    1. 将其转化为并行流，利用map这样的操作开展工作
    2. 枚举出集合中的每一个元素，创建新的线程，在Completable-Future内对其进行操作。

    这两个方式的使用建议：

    * 如果进行的是计算密集型的操作，并且没有I/O，那么推荐使用Stream接口，因为实现简单，同时效率也可能是最高的
    * 如果你并行的工作单元还涉及等待I/O的操作（包括网络连接等待），那么使用CompletableFuture灵活性更好。这种情况不使用并行流的另一个原因是，处理流的流水线中如果发生I/O等待，流的延迟特性会让我们很难判断到底什么时候触发了等待。

### 异步操作结合

CompletableFuture提供了类似Stream API的方法结合多个异步操作，以流水线方式运行。

#### CompletableFuture&lt;T>对象增加动作

| 方法         | 参数                       | 说明                             |
| ------------ | -------------------------- | -------------------------------- |
| thenApply    | T->U                       | 对结果应用一个函数               |
| thenCompose  | T->CompletableFuture&lt;U> | 对结果调用函数并执行返回的future |
| handle       | (T,Throwable)->U           | 处理结果或错误                   |
| thenAccept   | T->void                    | 类似thenApply                    |
| whenComplete | (T,Throwable)->void        | 类似于handle                     |
| thenRun      | Runnable                   | 执行Runable                      |

通常而言，名称中不带Async的方法和它的前一个任务一样，在同一个线程中运行；而名称以Async结尾的方法会将后续的任务提交到一个线程池，由不同的线程处理的。

* thenCompose

  如果有两个函数T -> CompletableFuture&lt;U>和U ->CompletableFuture&lt;V>，希望第二个函数在第一个函数完成时调用，可以通过thenCompose将它们组合为一个函数T ->CompletableFuture&lt;V>。

* handle

  CompletableFuture中抛出一个异常时，会捕获这个异常并在调用get方法时包装在一个受查异常ExecutionException中。

#### 组合多个对象

| 方法           | 参数                                | 说明                             |
| -------------- | ----------------------------------- | -------------------------------- |
| thenCombine    | CompletableFuture&lt;U>,(T,U)->V    | 执行两个动作并用给定函数组合结果 |
| thenAcceptBoth | CompletableFuture&lt;U>,(T,U)->void | 类似thenCombine                  |
| runAfterBoth   | CompletableFuture&lt;?>,Runnable    | 两个都完成后执行runnable         |
| applyToEither  | CompletableFuture&lt;T>,T->V        | 得到其中一个结果时，传入给定函数 |
| acceptEither   | CompletableFuture&lt;T>,T->void     | 类似applyToEither                |
| runAfterEither | CompletableFuture&lt;?>,Runnable    | 其中一个完成后执行runnable       |
| static allOf   | CompletableFuture&lt;?>...          | 所有给定future都完成后完成后     |
| static anyOf   | CompletableFuture&lt;?>...          | 任意给定future完成后完成         |

前3个方法并行运行一个CompletableFuture&lt;T>和一个CompletableFuture&lt;U>动作，并组合结果。

接下来3个方法并行运行两个CompletableFuture&lt;T>动作。一旦其中一个动作完成，就传递它的结果，并忽略另一个结果。

最后的静态allOf和anyOf方法取一组可完成future（数目可变），并生成一个CompletableFuture&lt;Void>，它会在所有这些future都完成时或者其中任意一个future完成时结束。不会传递任何结果。

* 等待所有任务执行完成

  allOf工厂方法接收一个由CompletableFuture构成的数组，数组中的所有Completable-Future对象执行完成之后，它返回一个CompletableFuture&lt;Void>对象。对allOf方法返回的CompletableFuture执行join操作，会阻塞主线程等待所有对象执行完成。

    ```java
    CompletableFuture[] futures = findPricesStream("myPhone")
                                    .map(f->f.thenAccept(System.out::println))
                                    .toArray(size->new CompletableFuture[size]);
    COmpletableFuture.allOf(futures).join();
    ```

* 只需要一个任务执行完成

  anyOf方法接收一个CompletableFuture对象构成的数组，返回由第一个执行完毕的CompletableFuture对象的返回值构成的Completable-Future&lt;Object>。

#### 示例程序

```java
public List<String> findPrice(String product){
    List<CompletableFuture<String>> priceFutures = 
        shops.stream()
        	.map(shop->CompletableFuture.supplyAsync(
            						()->shop.getPrice(product),executor))
        	.map(future->future.thenApply(Qutote::parse))
        	.map(future->future.thenCompose(quote->
                        CompletableFuture.supplyAsync(
                        	()->Discount.applyDiscount(quote),executor)))
        	.collect(Collectors.toList());
    
    return priceFutures.stream()
        		.map(CompletableFuture::join)
        		.collect(toList());
}

Future<Double> futurePriceInUSD = 
    CompletableFuture.supplyAsync(()->shop.getPrice(product))
    .thenCombine(
		CompletableFuture.supplyAsync(
        	()->exchangeService.getRate(Money.EUR,Money.USD),
        	(price,rate)->price*rate)
);
```





























