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

   获取月份时：一月对应0；获取星期时：周日对应1

   * 实例化

     调用子类`GregorianCaiendar`的构造器

     使用`getInstance()`方法

   * 常用方法

     `get()`：获取日期字段信息

     `set()`

     `add()`

     `getTime()`：日历类-->Date

     `setTime()`：Date-->日历类

这些API遇到的共同问题是：

1. 可变性：日期和时间应该是不可变的
2. 偏移性：Date中年份从1900开始，月份都从0开始
3. 格式化：格式化只对Date有用

### jdk8中的日期时间API

1. `LocalDate`、`LocalTime`、`LocalDateTIme`

   它们的实例是不可变的对象，分别表示使用 ISO-8601日历系统的日期、时间、日期和时间。

   它们提供了简单的本地日期或时间，并不包含当前的时间信息，也不包含与时区相关的信息

   * 实例化
   
       `now()`：返回当前日期时间的对象
   
       `of()`：返回指定日期时间的对象
       
   * 方法
   
       `get`：获取日期时段的信息
   
       `with`：返回设置日期时段的对象
   
       `plus`：返回添加日期时段的对象
   
       `minus`：返回减去日期时段的对象
   
2. `Instant`

   用来记录应用程序中的事件时间戳

   * 实例化

     `now()`：返回默认UTC时区的Instant类的对象

     `offEpochMilli()`：通过给定的时间戳创建对象

   * 方法

     `atOffset(ZoneOffset offset)`：结合即时的偏移来创建一个 OffsetDateTime

     `toEpochMilli()`：获取(UTC)时间戳

3. `DateTimeFormatter`

   格式化并解析日期时间

   * 实例化

     1. 预定义的标准格式
     2. 本地化相关的格式
     3. 自定义的格式

     ```java
     LocalDateTime now = LocalDateTime.now();
     
     DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;//first method
     formatter.format(now);
     formatter.parse("2021-03-13T08:12:05.361Z");
     
     DateTimeFormatter formatter1 = DateTImeFormatter.ofLocalLizedDateTime(FormatStyle.SHORT);//seconde method
     formatter1.format(now);
     
     DateTimeFormatter formatter2 = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
     formatter2.format(now);
     ```

     

