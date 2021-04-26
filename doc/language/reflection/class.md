# Class类

### 获取Class实例

1. 调用运行时类属性

   ```java
   Class cl = Person.class;
   ```

2. 调用运行时类对象的getClass()方法

   ```java
   Person p1 = new Person();
   Class c2 = p1.getClass();
   ```

3. 调用Class的forName()静态方法

   ```java
   Class c3 = Class.forName("demo.Person");
   ```

4. 使用类的加载器

   ```java
   ClassLoader cL = ReflectionTest.class.getClassLoader();
   Class c4 = cL.loadClass("demo.Person");
   ```

### 可以有Class对象的类型

（1）class： 

外部类，成员(成员内部类，静态内部类)，局部内部类，匿名内部类

（2）interface：接口

（3）[]：数组

（4）enum：枚举

（5）annotation：注解@interface

（6）primitive type：基本数据类型

（7）void

