# NIO.2

NIO是java1.4开始引入的新IO API，可以替换标准的IO API。NIO支持面向缓冲区，IO面向流，能够更加高效地进行文件读写操作。NIO.2是jdk7对NIO的拓展。

### Path

NIO.2为弥补File类功能有限的缺陷，引入Path接口用来替换File类。Path类可以表示文件或文件目录。

Path实例的获取需要借助Paths工具类的两个静态工厂方法：

1. Paths.get(URI uri)返回uri指定的Path路径

2. Paths.get(String first，String...more)

   多个字符串拼接成的Path路径