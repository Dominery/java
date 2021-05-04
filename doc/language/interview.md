# 面试题

1. `sleep()`与`wait()`的异同？

   * 相同点：调用该两个方法都会使线程进入阻塞状态

   * 不同点：

     两个方法声明的位置不同，`sleep()`在`Thread`类中声明，`wait()`在`object`中声明

     调用的位置要求不同，`sleep()`在任何位置都可以调用，`wait()`只能在同步方法或同步代码块中调用

     如果两个都在同步代码块或同步方法中调用，`wait()`会释放同步监视器，`sleep()`不会
   
2. ArrayList、LinkedList、Vector三者的异同？

   > 同：都实现List接口，存储的数据特点相同
   >
   > 异：
   >
   > ​	ArrayList是List接口的主要实现类，线程不安全、效率高，底层使用Object[] 存储
   >
   > ​	Vector是List接口的古老实现类，线程安全、效率低，底层使用Object[] 存储
   >
   > ​	LinkedList底层使用双向链表存储，如果频繁插入和删除，其效率高

3. Collection和Collections的区别？

   > Collection是集合的接口，Collections是操作Collection的工具类。

4. String s = new String("abc");方式在内存中创建了几个对象？

   两个。一个是字符串对象存储于堆空间，一个是char[]存储于方法区中的字符串常量池。

5. ```java
   public class StringTest{
       String str = new String("good");
       char[] ch = {'t','e','s','t'};
       public void change(String str,char[]){
           str = "test ok";
           char[0] = 'b';
       }
       public static void main(String[] args){
           StringTest ex = new StringTest();
           ex.change(ex.str,ex.ch);
           System.out.println(ex.str);// good
           System.out.println(ex.ch); // best
       }
   }
   ```

6. `throw`和`throws`之间的区别是什么？

   `throw`是抛出异常的方式，`throws`是异常处理的一种方式。`throw`用于异常的生成阶段，`throws`声明方法可能抛出的各种异常类。