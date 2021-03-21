# 注解的使用

注解(Anotation)是代码中的特殊标记，这些标记可以在编译、类加载、运行时被读取，并执行相应处理。

Annotation 可以像修饰符一样被使用, 可用于修饰包,类, 构造器, 方 法, 成员变量, 参数, 局部变量的声明, 这些信息被保存在 Annotation 的 “name=value” 对中。

### JDK中的注解

* 三个常用注解

    1. `@override`在编译时进行校验是否重写了父类的方法。
    2. `@Deprecated`表示API(类、方法)已经过时，但因为向下兼容的缘故依旧还能使用。
    3. `@SuppressWorning`抑制编译器警告
    
* 元注解

    > 元注解是用来修饰其他注解的注解。

    1. `Retention`只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 的生命周期，共有三种修饰符：SOURCE、CLASS、RUNTIME，只有RUNTIME可以在程序运行中使用注解，默认为CLASS
    
    2. `Target`用来指定被修饰的Annotation能够修饰哪些元素
    
    3. `Documented`能够让修饰的元素注解信息在Javadoc中保留下来
    
    4. `Inherited`使注解信息具备继承性
    

    
       

### 自定义注解

参照`SuppressWorning`定义。

1. 注解声明为：`@interface`
2. 内部定义成员，通常以`value`作为成员名
3. 可以指定默认值，用`default`修饰
4. 如果自定义注解没有成员，那么该注解当标识使用

```java
@Retention(Retentionpolicy.RUNTIME)
@Target({TYPE,FIELD,METHOD})
public @interface MyAnnotation{
    String value()default "hello";
}
```

自定义注解必须配上注解的信息处理流程(反射)才有意义。一般自定义注解使用Retention和Target。

### jdk8新特性

*  `Repeatable`能够让注解可重复修饰

     ```java
     @Retention(Retentionpolicy.RUNTIME) // 2. make meta anntation  same to MyAnnotation
     @Target({TYPE,FIELD,METHOD})
     public @interface MyAnnotations{
     MyAnnotation value();
     }

    @Repeatable(MyAnnotations.class) //1.declare the Repeatale
    @Retention(Retentionpolicy.RUNTIME)
    @Target({TYPE,FIELD,METHOD})
    public @interface MyAnnotation{
    String[] value();
    }

    @MyAnnotation("hi")
    @MyAnnotation("what")
    class Person{

    }
    ```

* 类型注解：可以对泛型中的类型进行修饰

  ```java
  class Generic<@MyAnnotation T>{ // add TYPE_PARAMETER in Target
      public void show(){
          ArrayList<@MyAnntation String> strList = new ArrayList<>();// add TYPE_USE in Target
      }
  }
  ```

  