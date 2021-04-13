# 动态代理

动态代理是指客户通过代理类来调用其它对象的方法，并且是在程序运行时根据需要动态创建目标类的代理对象。动态代理解决了静态代理需要过多代理类及不利于程序拓展的问题。

java通过Proxy实现创建动态代理类和动态代理对象。

> Proxy ：专门完成代理的操作类，是所有动态代理类的父类。通过此类为一
>
> 个或多个接口动态地生成实现类。

动态代理示例代码

```java
interface InetWork{
    void serve();
}

class Server implements InetWork{
    public void serve() {
        System.out.println("server");
    }
}

class ServeProxy{
    public static Object getProxy(Object obj){
        Handler handler = new Handler(obj);
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(),obj.getClass().getInterfaces(),handler);
    }
}

class Handler implements InvocationHandler{
    private final Object obj;
    public Handler(Object obj){
        this.obj = obj;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        return method.invoke(obj, args);
    }
}


public class ProxyTest {
    public static void main(String[] args) {
        Server server = new Server();
        InetWork proxy = (InetWork) ServeProxy.getProxy(server);
        proxy.serve();
    }
}
```

