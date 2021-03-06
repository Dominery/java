# 定义异常

如果某个方法需要抛出异常，现有的异常类皆不能满足想要抛出的异常的需求，那么可以自定义异常类，其步骤如下：

1. 继承现有的异常体系，`RuntimeException`、`Exception`
2. 提供全局常量，`serialVersionUID`
3. 提供重载的构造器

```java
public class MyException extends RuntimeException{
    static final long serialVerionUID = -908829848032L;
    public MyException(){}
    public MyException(String message){
        super(messgae);
    }
}
```

