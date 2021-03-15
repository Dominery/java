# 比较器

在开发场景中，需要对多个对象依照某个属性进行排序，即需要比较对象的大小，可以通过实现`Comparable`或`Comparator`接口达到目的。

两种接口实现的区别：

1. `Comparable`接口实现后，可以保证对象在任何时候都可以比较大小
2. `Comparator`接口是一种临时性的比较策略

### Comparable使用

实现`Comparable`接口的类需要重写`compareTo(obj)`方法，重写的规则是：如果当前对象this大于形参对象obj，则返回正整数，如果小于，则返回负整数，如果相等，返回0。

```java
class Goods implements Comparable{
    private double price;
    public int compareTo(Object o){
        if(o isinstanceof Goods){
            Goods goods = (Goods)o;
            if(this.price >goods.price){
                return 1;
            }else if(this.price <goods.price){
                return -1;
            }else{
                return 0;
            }
        }
        throw new RuntimeException("Unsurported class");
    }
}
```

### Comparator的使用

当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码，或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，那么可以考虑使用 Comparator 的对象来排序。

重写compare(Object o1,Object o2)方法，比较o1和o2的大小：如果方法返回正整数，则表示o1大于o2；如果返回0，表示相等；返回负整数，表示o1小于o2

```java
Strin[] arr = new String[]{"AA","CC","KK"};
Arrays.sort(arr,new Comaparator<String>(){
    public int compare(String o1,String o2){
        return -ol.compareTo(o2);
    }
})
```

