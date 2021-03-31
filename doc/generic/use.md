# 使用

集合接口或集合类在jdk1.5中都修改为带泛型的结构。

在实例化集合类时，可以指明具体的泛型类型。

指明完以后，在集合类或接口中凡是定义类或接口时，内部结构使用到类的泛型位置，都指定为实例化的泛型类型。

泛型的类型必须是类，不能是基本数据类型。

如果实例化时，没有指明泛型的类型，默认类型为Object类型。

jdk1.7新增类型推断。

不能使用new E[]，实现泛型数组需要进行强转，E[] elements = (E[]) new Object[capacity]。

父类带有泛型，子类的继承存在以下情况：

```java
class Father<T1,T2>{
    
}
class Son1 extends Father{} //equals Son extends Father<Object,Object>
class Son2 extends Father<Integer,String>{} //there is no generic in subclass
class Son3<T1,T2> extends Father<T1,T2>{}
class Son4<T2> extends Father<Integer,T2>{}
class Son5<A,B> extends Father{}
class Son6<A,B> extends Father<Integer,String>{}
class Son7<T1,T2,A,B> extends Father<T1,T2>{}
class Son8<T2,A,B> extends Father<Integer,T2>{}
```

