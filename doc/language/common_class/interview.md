# 面试题

1. String s = new String("abc");方式在内存中创建了几个对象？

   两个。一个是字符串对象存储于堆空间，一个是char[]存储于方法区中的字符串常量池。

2. ```java
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

3. 