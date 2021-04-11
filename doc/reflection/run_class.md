# 运行时类的信息获取

### 创建运行时类的对象

newInstance()方法会调用相应类的空参构造器，返回该类的对象。

类必须提供权限足够的空参构造器。

```java
Class<Person> cl = Person.class;
Person p1 = cl.newInstance();
```

### 获取运行时类的结构

1. 获取运行时类的属性信息

   getFields()获取当前运行时类及父类的public的属性。

   getDeclaredFields()获取当前运行时类的所有属性信息。

   ```java
   Field[] declaredFields = cl.getDeclaredFields();
   for(Field f:declaredFields){
       int modifier = f.getModifiers();
       System.out.print(Modifier.toString(modifier)+"\t");
       
       Class type = f.getType();
       System.out.print(type.getName()+"\t");
       
       String name = f.getName();
       System.out.println(name);
   }
   ```

2. 获取运行时类的方法

   getMethods()和getDeclaredMethods()方法获取的信息与获取属性的方法类似。

3. 获取类的构造器

   getConstructors()获取当前类的public构造器

4. 获取类的带泛型的父类的泛型

   ```java
   Class cl = Person.class;
   Type genericSuperclass = cl.getGenericSuperclass();
   Type[] acualTypeArguments = paramType.getActualTypeArguments();
   System.out.println(((Class)actualTypeArguments[0]).getName())
   ```

5. 获取类的接口

   ```java
   Class cl = Person.class;
   Class[] interface1 = cl.getInterface();
   ```

### 调用运行时类的指定结构

1. 属性的设置

   ```java
   Class<Person> cl = Person.class;
   Person p = cl.newInstance();
   Field name = cl.getDeclaredField("name");
   name.setAccessible(true);
   name.set(p,"Bob");
   ```

2. 指定方法调用

   ```java
   Method eat = cl.getDeclaredMathod("eat",String.class);
   eat.setAccessible(true); // if the method is private,make it can be invoked
   eat.invoke(p,"milk");
   ```

3. 指定构造器

   运行时创建类对象一般使用class的newInstance()方法，指定参数构造器不常用。

   getDeclaredConstructor()中参数用于指明构造器参数列表。

   ```java
   Constructor cs = cl.getDeclaredConstructor(String.class);
   cs.setAccessible(true);
   Person p = cs.newInstance("Bob");
   ```

   