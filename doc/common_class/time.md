# 日期时间

### jdk8之前日期时间API

1. `java.lang.System`

   `System`类提供的`currentTimeMillis()`方法返回当前时间与1970.1.1零点之间以毫秒为单位的时间。

2. `java.util.Date`

   表示特定的瞬间，精确到毫秒。

   * 两个构造器的使用

     `Date()`：创建一个对应当前时间的Date对象

     `Date(long )`：创建指定时间戳的Date对象

   * 两个方法的使用

     `toString()`：显示当前对象的年月日

     `getTime()`：获取当前对象的时间戳

3. `java.sql.Date`

   对应数据库中的日期类型的变量。

   * 创建对象

     `java.sql.Date(long )`：创建指定时间戳的对象

   * 与`java.util.Date`转换

     1. 多态

        ```java
        Date date = new java.sql.Date(23454323L);
        java.sql.Date date2 = (java.sql.Date)date;
        ```

     2. 构造器

        ```java
        Date date = new Date();
        java.sql.Date date2 = new java.sql.Date(date.getTime());
        ```

4. `java.text.SimpleDateFormat`

   允许对`Date`进行格式化和解析：日期-->文本、文本-->日期。

   ```java
   //use the default constructor
   SimpleDateFormate sdf = new SimpleDateFormate();
   Date date = new Date();
   sdf.format(date);
   
   String s1 = "20-1-23 下午3:00";
   Date date1 = sdf.parse(s1);
   
   //y:year M:month d:day h:hour m:minute s:second
   SimpleDateFormate sdf1 = new SimpleDateFormate("yyyy-MM-dd hh:mm:ss");
   sdf.format(date);
   ```

5. `java.util.Calendar`

   `Calendar`是抽象类，用于完成日期字段之间相互操作的功能。

   * 实例化

     调用子类`GregorianCaiendar`的构造器

     使用`getInstance()`方法

   * 常用方法

     `get()`：获取日期字段信息

     `set()`

     `add()`

     `getTime()`：日历类-->Date

     `setTime()`：Date-->日历类

