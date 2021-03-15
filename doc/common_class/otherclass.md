# 其他类

### System类

成员变量

* System类内部包含in、out和err三个成员变量，分别代表标准输入流(键盘输入)，标准输出流(显示器)和标准错误输出流(显示器)。 

成员方法

* native long currentTimeMillis()： 

  该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时间和GMT时间(格林威治时间)1970年1月1号0时0分0秒所差的毫秒数。

*  void exit(int status)： 

  该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等。

* void gc()**：** 

  该方法的作用是请求系统进行垃圾回收。至于系统是否立刻回收，则取决于系统中垃圾回收算法的实现以及系统执行时的情况。

* String getProperty(String key)**：** 

  该方法的作用是获得系统中属性名为key的属性对应的值。
  
  | 属性名       | 说明               |
  | ------------ | ------------------ |
  | java.version | java运行时环境版本 |
  | java.home    | java安装目录       |
  | os.name      | 操作系统名称       |
  | os.version   | 操作系统版本       |
  | user.name    | 用户账户名称       |
  | user.home    | 用户主目录         |
  | user.dir     | 当前工作目录       |

### Math类

`Math`类中主要包含静态方法。

abs 绝对值

acos,asin,atan,cos,sin,tan 三角函数

sqrt 平方根

pow(double a,doble b) a的b次幂

log 自然对数

exp e为底指数

max(double a,double b)

min(double a,double b)

random() 返回0.0到1.0的随机数

long round(double a) double型数据a转换为long型（四舍五入）

toDegrees(double angrad) 弧度—>角度

toRadians(double angdeg) 角度—>弧度

### BigInteger和BigDecimal

`BigInteger`可以表示任意精度的整数。

`BigDecimal`支持任意精度的有符号十进制浮点数。