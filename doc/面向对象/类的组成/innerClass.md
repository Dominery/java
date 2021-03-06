# 内部类

如果一个类A声明在类B中，那么称类A为内部类，为简便考虑，将类B称为外部类。

### 内部类的分类

依据内部类声明的位置可以将内部类分为**成员内部类**和**局部内部类**两种。

* 成员内部类

  如果一个内部类的声明与外部类的方法声明并列，那么称这个内部类为成员内部类。

  ```java
  class A{
      public class B{}
      public void method(){}
  }
  ```

  成员内部类具备以下特性：

  1. 作为外部类的成员
     * 调用外部类的属性、方法
     * 可以被`static`修饰
     * 可以被四种权限修饰符修饰
  2. 作为一个类
     * 类内可以定义属性、方法、构造器
     * 可以被`final`、`abstract`等修饰

### 内部类使用问题

* 如何在外部类之外实例化成员内部类的对象

  ```java
  class A{
      public static class B{}
      public class C{}
  }
  
  public class Test{
      public static main(String[] args){
          A.B b = new A.B(); // if the inner class is modified by static
          A.C c = A.new C();
      }
  }
  ```

* 如何在成员内部类中调用外部类的结构

  ```java
  class A{
      private int age;
      public class B{
          private int age;
          public void method1(){ // if the outer class and inner class have the same method
              A.this.method();
              // code for self
          }
          public void method2(){ // if the outer class and inner class don't have same method
              method();
          }
          public void showAttr(int age){
              System.out.println("local variety:"+age);
              System.out.println("inner class variety:"+this.age);
              System.out.println("outer class variety:"+A.this.age);
          }
      }
      public void method1(){}
      public void method(){}
  }
  ```

* 局部内部类的使用

  ```java
  class CompareFactory{
      public Comparable getComparable(){
          class MyComparable implements Comparable{
              @Override
              public int compareTo(Object o){
                  return 0;
              }
          }
          return new MyComparable();
      }
  }
  ```

  

