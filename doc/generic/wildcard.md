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

### 无限定通配符

如果某泛型类的类型参数只使用通配符`<?>`，则只能对其数据进行访问，无法进行写入操作。

> G<? >和G本质的不同在于：可以用任意Object对象调用原始G类的setObject方法。可以调用G\<?\>的setFirst(null)。