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

jak8中提供的日期-时间对象(LocalDateTime、LocalDate、LocalTime、Instant、Duration、Period)都是不可修改的，这是为了更好地支持函数式编程，确保线程安全，保持领域模式一致性而做出的重大设计决定。

#### 表示时间-日期

要表示时间日期可以使用LocalDateTime和Instant，它们是为不同的目的而设计的，一个是为了便于人阅读使用，另一个是为了便于机器处理。

1. LocalDate、LocalTime

   LocalDate类实例提供简单的日期，不包含当天的时间信息。

   LocalTime类实例提供当天的时间信息。

    * 需要通过静态工厂方法实例化

        `now()`：返回当前日期时间的对象

        `of()`：返回指定日期时间的对象

        `parse()`:LocalDate和LocalTime都可以通过解析代表它们的字符串创建，可以向parse方法传递一个DateTimeFormatter。

    * 获取日期时段信息

        要获取日期时间信息，有以下两种方法：

        1. 调用getHour()、getDayofMonth()等相关方法

        2. 单独调用get()方法，传递一个TemporalField参数，TemporalField是一个接口，它定义了如何访问temporal对象某个字段的值。ChronoField枚举实现了这一接口。

        ```java
        LocaleDate date = LocalDate.of(2021,6,30);
        date.getMonth();
        date.get(ChronoFiel.MONTH_OF_YEAR);
        ```

2. 日历转换

   ISO-8601日历系统是世界文明日历系统的事实标准。但是，Java 8中另外还提供了4种其他的日历系统。这些日历系统中的每一个都有一个对应的日志类，分别是ThaiBuddhistDate、MinguoDate、JapaneseDate以及HijrahDate。所有这些类以及LocalDate都实现了ChronoLocalDate接口，能够对公历的日期进行建模。
   
   ```java
   LocalDate date = LocalDate.of(2020,Monty.MAY,25);
   JapaneseDate jpDate = JapaneseDate.from(date);
   ```
   
3. LocalDateTime

   LocalDateTime是LocalDate和LocalTime的合体，同时表示日期和时间。

   * 创建方式

     可以直接创建，也可以通过合并日期和时间对象构造。

     ```java
     LocalDateTime dt1 = LocalDateTime.of(2021,Month.JUNE,30,12,0,0);
     LocalDateTime dt2 = LocalDateTime.of(date,time);
     LocalDateTime dt3 = date.atTime(12,0,0);
     LocalDateTime dt4 = date.atTime(time);
     LocalDateTime dt5 = time.atDate(date);
     ```

     向LocalDate传递一个时间对象，或者向LocalTime传递一个日期对象的方式，可以创建一个LocalDateTime对象。

     可以使用toLocalDate或者toLocalTime方法，从LocalDateTime中提取LocalDate或者LocalTime组件

4. Instant

   java.time.Instant类对时间建模的方式，基本上它是以Unix元年时间（传统的设定为UTC时区1970年1月1日午夜时分）开始所经历的秒数进行计算。

    * 实例化

      `now()`：返回默认UTC时区的Instant类的对象

      `offEpochMilli()`：通过给定的时间戳创建对象

    * 方法

      `atOffset(ZoneOffset offset)`：结合即时的偏移来创建一个 OffsetDateTime

      `toEpochMilli()`：获取(UTC)时间戳

#### 表示时间间隔

* duration

  如果需要以秒或纳秒为单位对时间单位进行建模，可以使用Duration类的静态工厂方法between。

* Period

  如果需要以年、月或者日的方式对多个时间单位建模，可以使用Period类的工厂方法between，可以得到两个LocalDate之间的时长。
  
* Duration类和Period类共享了很多相似的方法

  | 方法名       | 静态方法？ | 说明                                                 |
  | ------------ | ---------- | ---------------------------------------------------- |
  | between      | Y          | 创建两个时间点之间的interval                         |
  | from         | Y          | 由一个临时时间点创建interval                         |
  | of           | Y          | 由它的组成部分创建interval实例                       |
  | parse        | Y          | 由字符串创建interval实例                             |
  | addTo        | N          | 创建interval副本，并将其叠加到某个指定的temporal对象 |
  | get          | N          | 读取该interval状态                                   |
  | isNagative   | N          | 检查该interval是否为负值                             |
  | isZero       | N          | 检查该interval是否为0                                |
  | minus        | N          | 通过减去一定时间创建该interval副本                   |
  | multipliedBy | N          | 将interval值乘以某个标量创建该interval副本           |
  | negated      | N          | 以忽略某个时长的方式创建interval副本                 |
  | plus         | N          | 以增加某个指定时长的方式创建interval副本             |
  | subtractFrom | N          | 从指定的temporal对象减去该interval                   |


#### 操纵解析时间

所有表示时间的类都实现了Temporal接口，Temporal接口定义了如何读取和操纵为时间建模的对象的值。Temporal接口声明了get、with、plus、minus等抽象方法，通过这些通用的方法可以操作Temporal对象。任何对Temporal对象进行修改的操作都不会修改原本的对象，只会获取修改后返回的新对象。

1. LocalDate、LocalTime、LocalDateTime以及Instant这样表示时间点的日期-时间类提供了大量通用的方法

    | 方法名   | 静态方法？ | 说明                                                         |
    | -------- | ---------- | ------------------------------------------------------------ |
    | from     | Y          | 依据传入的Temporal对象创建对象实例                           |
    | now      | Y          | 根据系统时钟创建Temporal对象                                 |
    | of       | Y          | 由Temporal对象的某个部分创建该对象的实例                     |
    | parse    | Y          | 由字符串创建Temporal对象实例                                 |
    | atOffset | N          | 将Temporal对象和某个时区偏移结合                             |
    | atZone   | N          | 将Temporal对象和某个时区结合                                 |
    | format   | N          | 使用指定的格式器将Temporal对象转换为字符串(Instant类不提供该方法) |
    | get      | N          | 读取Temporal对象某一部分的值                                 |
    | minus    | N          | 创建一个将当前Temporal对象的值减去一定时长后的副本           |
    | plus     | N          | 创建一个将当前Temporal对象的值加上一定时长后的副本           |
    | with     | N          | 创建一个将当前Temporal对象的值修改后的副本                   |

2. 如果需要对日期进行复杂的操作，可以使用重载版本的with方法，向其传递一个提供了更多定制化选择的TemporalAdjuster对象，更加灵活地处理日期。

   对于最常见的用例，日期和时间API已经提供了大量预定义的TemporalAdjuster。

#### 打印输出及解析时间对象

DateTimeFormatter用于格式化并解析日期时间。和老的java.util.DateFormat相比较，所有的DateTimeFormatter实例都是线程安全的。

DateTimeFormatter实例都能用于以一定的格式创建代表特定日期或时间的字符串，也可以通过解析代表日期或时间的字符串重新创建该日期对象。

* 创建格式器

  1. 预定义的标准格式
  
     通过它的静态工厂方法以及常量

  2. 本地化相关的格式
  
     通过解析代表日期或时间的字符串重新创建该日期对象。
  
  3. 自定义的格式
  
     DateTimeFormatter类支持一个静态工厂方法，它可以按照某个特定的模式创建格式器
  
  4. 构造格式器
  
     如果需要更加细粒度的控制，DateTimeFormatterBuilder类还提供了更复杂的格式器构造方法。
  
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
  

#### 处理不同时区

时区的处理是新版日期和时间API新增加的重要功能。新的java.time.ZoneId类是老版java.util.TimeZone的替代品。ZoneId类也是无法修改的。

1. 创建

    时区是按照一定的规则将区域划分成的标准时间相同的区间。在ZoneRules这个类中包含了40个这样的实例。你可以简单地通过调用ZoneId的getRules()得到指定时区的规则。每个特定的ZoneId对象都由一个地区ID标识。

    ```java
    ZoneId romeZone = ZoneId.of("Europe/Rome");
    ```

    > 地区ID都为“{区域}/{城市}”的格式，这些地区集合的设定都由英特网编号分配机构（IANA）的时区数据库提供。
    
2. 使用

    可以将ZoneId对象与LocalDate、LocalDateTime或者是Instant对象整合起来，构造为一个ZonedDateTime实例，它代表了相对于指定时区的时间点。

    ```java
    LocalDate date = LocalDate.of(2020,Month.May,25);
    ZonedDateTime zdt1 = date.atStartOfDay(romeZone);
    
    LocalDateTime dateTime = LocalDateTime.of(2020,Month.May,25,0,0);
    ZonedDateTime zdt2 = dateTime.atZone(romeZone);
    ```

    通过ZonId，可以将LocalDateTime和Instant相互转换。

    ```java
    Instant inst = dateTime.toInstant(romeZone);
    LocalDateTime time = LocalDateTime.ofIntant(inst,romeZone);
    ```

3. 其他方式

    另一种比较通用的表达时区的方式是利用当前时区和UTC/格林尼治的固定偏差。这种情况下，可以使用ZoneOffset类，它是ZoneId的一个子类，表示的是当前时间和伦敦格林尼治子午线时间的差异。

    ```java
    ZoneOffset newYorkOffset = ZoneOffset.of("-05:00");
    ```

    >使用这种方式定义的ZoneOffset并未考虑任何日光时的影响，所以在大多数情况下，不推荐使用。













