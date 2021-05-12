# 处理异常

处理异常依据异常是否在发生异常的代码块中被处理可以分为三类：异常捕获、异常传递、捕获传递。

如果某个异常发生的时候没有在任何地方进行捕获，那程序就会终止执行，并在控制台上打印出异常信息，其中包括异常的类型和堆栈的内容。

_异常处理一般只处理受检异常_

## 类别

### 异常捕获

如果某段代码会抛出异常，该异常通过`try/catch`关键字进行捕获处理，那么称这种异常处理方式为异常捕获。

* `try/catch`使用

  1. `catch`代码块执行完后，如果没有`finally`代码块，那么跳出`try/catch`结构。
  2. `catch`中的异常如果没有继承关系，那么无所谓先后；如果存在继承关系，那么父类异常需要声明在后。
  3. 变量如果在`try`代码块中声明，那么在代码块之外无法使用。

  ```java
  try{
      //code that may throw exception
  }catch(ExceptionType e){
      //deal with the specific exception
      e.printStackTrace();
  }
  ```

* `finally`使用

  1. `finally`保证程序离开异常捕获方法前必然会执行`finally`代码块中的代码。
  2. 如果某些资源需要手动释放，那么释放资源的代码需要写在`finally`代码块中。

  ```java
  try{
      //code
  }catch(Exception e){}
  finally{
      // process 
  }
  ```

* 异常捕获进行文件读入

  ```java
  FileInputStream fis = null;
  try{
      File file = new File("demo.txt");
      fis = new FileInputStream(file);

      int data = fis.read();
      while(data != -1){
          System.out.println(data);
          data = fis.read();
      }
  }catch(FileNotFoundException e){
      e.printStackTrace();
  }catch(IOException e){
      e.printStackTrace();
  }finally{
      try{
          if(fis!=null)fis.close();   
      }catch(IOException e){
          e.printStackTrace();
      }
  }
  ```

### 异常传递

如果某段代码会抛出异常，该异常所在方法通过`throws`声明将异常向上一级抛出，那么称这种异常处理方式为异常传递。

* `throws`使用

  1. 如果异常发生的方法体内不需要处理异常，那么可以通过`throws`把异常传递给调用者。
  2. 子类重写的方法抛出的异常类型不大于父类被重写的方法抛出的异常类型\(多态的保持）。

  ```java
  public void method() throws IOException{

  }
  ```

### 捕获传递

如果某段代码会抛出异常，该异常通过`try/catch`关键字进行捕获，包装成新的异常将异常向上一级抛出，那么称这种异常处理方式为捕获传递。

```java
try{
    
}catch(Exception e){
    throw new Exception("");
}
```

## 选择

1. 如果父类中被重写的方法没有`throws`异常，子类中的代码可能会抛出异常，那么子类必须捕获异常。
2. 如果 func1() 抛出的异常是可以恢复，且 func2() 的调用方并不关心此异常，可以在 func2() 内将 func1() 抛出的异常捕获
3. 如果 func1() 抛出的异常对 func2() 的调用方来说，也是可以理解的、关心的 ，并且在业务概念上有一定的相关性，可以选择异常传递
4. 如果 func1() 抛出的异常太底层，对 func2() 的调用方来说，缺乏背景去理解、且业务概念上无关，可以使用捕获传递，包装成调用方可以理解的新异常

异常是否往上继续传递，要看上层代码是否关心这个异常。

是否需要包装成新的异常抛出，看上层代码是否能理解这个异常、是否业务相关。如果能理解、业务相关就可以直接抛出，否则就封装成新的异常抛出。