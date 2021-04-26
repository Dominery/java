# File类的使用

File类是文件和文件目录的抽象表现形式。

> File能够新建、删除、重命名文件和目录，但File不能访问文件内容。
>
> 如果在java中想要表示一个真实存在的文件和目录，那么必须使用File对象，但是File对象不一定对应真实存在的文件和目录。

* File构造器

  > 为了解决操作系统之间路径分隔符的不同，File类提供separator常量，根据操作系统动态分配分隔符。

  ```java
  File file1 = new File("demo.txt"); //relative path
  File file2 = new File("C:"+File.separator+"demo.txt");//absolute path
  File file3 = new File("C:","demo");
  File file4 = new File(file3,"demo.jpg");
  ```

  在普通方法中使用相对路径创建文件对象，默认相对于当前工程目录，在Test方法中使用相对路径创建文件对象，相对于当前module。

