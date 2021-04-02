# 自定义

### 定义泛型类

自定义泛型类需要通过尖括号声明类型变量，类型变量可以声明多个。类定义中的类型变量指定方法的返回类型以及域和局部变量的类型。

> 在Java库中，使用变量E表示集合的元素类型，K和V分别表示表的关键字与值的类型。T（需要时还可以用临近的字母U和S）表示“任意类型”。

```java
public class Pair<T> {
    private T first;
    private T second;

    public Pair(){
        first = null;
        second = null;
    }
    public Pair(T first,T second){
        this.first = first;
        this.second = second;
    }
    public T getFirst(){return first;}
    public void setFirst(T first) {this.first = first;}
    public T getSecond() {return second;}
    public void setSecond(T second) { this.second = second;}
}
```

### 定义泛型方法

如果一个方法声明了类型变量，则该方法为泛型方法。

泛型方法中的类型变量与类的类型变量没有关系，定义泛型方法时，类型变量放在修饰符后面，返回类型前面。

因为泛型方法中的类型变量是在调用方法时确定，并非实例化类时确定，所以泛型方法可以为静态方法。

```java
class ArrayAlg{
    public static <T extends Comparable<? super T>> Pair<T> minmax(T[] a){
        if(a==null||a.length==0)return null;
        T min = a[0];
        T max = a[0];
        for(int i=1;i<a.length;i++){
            if(min.compareTo(a[i])>0)min = a[i];
            if(max.compareTo(a[i])<0)max = a[i];
        }
        return new Pair<>(min,max);
    }
}
```

### 限定类型变量

如果泛型类中的方法或者泛型方法需要对类型变量进行某操作，该操作需要类型变量满足实现某个接口，那么可以通过在声明时限定类型变量加以实现。

限定类型变量的格式为`<T extends BoundingType >`或`<T extends Comparable&Serializable>`。

> T应该是绑定类型的子类型（subtype）。T和绑定类型可以是类，也可以是接口。选择关键字extends的原因是更接近子类的概念。