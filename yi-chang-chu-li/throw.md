# 抛出异常

如果在某个方法中需要抛出异常，其步骤是：

1. 找到合适的异常类
2. 创建该异常类的对象
3. 通过`throw`关键字将对象抛出
4. 如果该异常是受检异常，对方法用`throws`关键字进行声明

## 难点

1. 在非受检异常与受检异常中选择

   由业务逻辑校验引起的错误（例如，错误的格式或运算）应该是不受检查的异常，因为它们会在你的代码中添加大量的try/catch混乱。

   系统错误（例如，磁盘空间耗尽）也应该是不受检查的异常，因为客户端对此无能为力。

   建议使用不受检查的异常，并且尽可能少量使用受检查的异常，以避免代码中有明显的混乱。

2. `Notification模式`

   Notification模式的目的是在你使用太多不受检查的异常情况时提供一个解决方案。该解决方案引入了一个领域类来收集错误信息。

## 示例

```java
public BankTransaction parse(final String line){
    final String[] columns = line.split(",");
    if(column.length<EXPECTED_COLUMN){
        throw new CSVSyntaxException("Not have enough columns:"+line);
    }
    //else code
}
class CSVSyntaxException extends SyntaxException{
    public CSVSyntaxException(String message){
        super(message);
    }
}
```

