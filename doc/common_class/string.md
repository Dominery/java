# 字符串相关的类

### String的使用

* 注意要点

  `String`通过`final`修饰，不能被继承。

  `String`实现了`Serializable`接口和`Comparable`接口，其对象支持序列化比较大小。

  `String`对象的字符内容存储在`final char[] value`中，对象创建后不可被修改。

  通过字面量的方式给一个字符串赋值，此时的字符串声明在字符串常量池中，字符串常量池在方法区中。

  通过构造器创建的字符串对象，保存在堆空间中，其`value`值指向字符串常量池中的字符串常量。

  `String`具有不可变性：
  
    1. 当对字符串重新赋值，需要重新指定内存区域赋值，不能使用原有的value
    2. 当对现有的字符串进行连接操作时，需要重新指定内存区域赋值
    3. 当调用String的replace方法修改指定字符，需要重新指定内存区域赋值
  
  字符串常量池中不会存储相同内容的字符串。
  
* 对象创建

  ```java
  String str = "hello";
  
  //本质上this.value = new char[0];
  String s1 = new String();
  
  //this.value = original.value;
  String s2 = new String(String original);
  
  //this.value = Arrays.copyof(value,value.length)
  String s3 = new String(char[] a);
  
  String s4 = new String(char[] a,int startIndex,int count);
  ```

* 字符串`+`拼接

  * 常量与常量的拼接结果在常量池
  * 只要有一个是变量，结果就在堆空间中
  * 如果变量使用了`intern()`方法，那么结果在常量池

* `String`与其他类型的转换

    1. 与基本数据类型、包装类之间的转换

        * `String`--> 基本数据类型：调用包装类的静态方法：`parse`
        * 基本数据类型-->`String`：调用`String`重载的`valueOf`方法
    2. 与`char[]`之间的转换
        * `String`-->`char[]`：调用`String`的`toCharArray`方法
        * `char[]`-->`String`：调用`String`的构造器
    3. 与`byte[]`之间的转换
        * `String`-->`byte[]`(encode)：调用`String`的`getBytes()`方法
        * `byte[]`-->`String`(decode)：调用`String`的构造器

