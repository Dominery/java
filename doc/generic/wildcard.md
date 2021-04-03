# 通配符

如果某个方法的参数或者返回类型需要为泛型，且该泛型的类型参数有限定(该类的父类或子类)，可以使用通配符实现这个需求。

```java
public static Map<String,Class<? extends Actor>> chars= new HashMap<>(); // Player、Coin、Lava all implements Actor
static {
    chars.put("@",Player.class);
    chars.put("o",Coin.class);
    chars.put("v",Lava.class);
}
```

### 子类型限定通配符

如果泛型类型声明为`G<? extends ClassType>`，那么该泛型类型为子类型通配符，可以匹配所有`ClassType`的子类的泛型对象，可以使用方法的返回值，但不能为方法提供参数。

> \<? extends ClassType\>必然是ClassType的子类，所以借助多态，其返回值可以用ClassType类型的引用接收，而提供的参数类型是ClassType的子类但不一定为\<? extends ClassType\>的子类，方法体内无法接收。

### 超类型限定通配符

如果泛型类型声明为`G<? super ClassType>`，那么该泛型类型为超类型通配符，可以匹配所有`ClassType`的父类的泛型对象，可以为方法提供参数，但不能使用返回值。

> \<? super ClassType\>是ClassType的父类且无法确定其类型，其返回值只能用Object类型的引用接收，而提供的参数如果为ClassType的子类，则其一定为\<? super ClassType\>的子类，方法体内能够接收。

### 无限定通配符

如果泛型类型声明为`G<?>`，那么该泛型类型为无限定通配符，只能对其对象数据进行访问，无法进行写入操作。

> G<? >和G本质的不同在于：可以用任意Object对象调用原始G类的setObject方法。可以调用G\<?\>的setFirst(null)。