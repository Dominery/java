# NIO.2

NIO是java1.4开始引入的新IO API，可以替换标准的IO API。NIO支持面向缓冲区，IO面向流，能够更加高效地进行文件读写操作。NIO.2是jdk7对NIO的拓展。Path和Files类封装了在用户机器上处理文件系统所需的所有功能。

### Path

NIO.2为弥补File类功能有限的缺陷，引入Path接口用来替换File类。Path类可以表示文件或文件目录。

#### Path对象创建

Path实例的获取需要借助Paths工具类的两个静态工厂方法：

1. Paths.get(URI uri)返回uri指定的Path路径

2. Paths.get(String first，String...more)

   多个字符串拼接成的Path路径

#### 组合或解析

| 方法                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| resolve(q)             | 如果q是绝对路径，则结果就是q。 否则，根据文件系统的规则，将“p后面跟着q”作为结果。 |
| resolveSibling(q)      | 通过解析指定路径的父路径产生其兄弟路径。                     |
| normalize()            | 移除所有冗余的.和..部件                                      |
| toAbsolutePath()       | 产生给定路径的绝对路径，该绝对路径从根部件开始               |
| relativize(Path other) | 返回用this进行解析，相对于other的相对路径。                  |
| getParent()            | 返回父路径，或者在该路径没有父路径时，返回null。             |
| getFileName()          | 返回该路径的最后一个部件，或者在该路径没有任何部件时，返回null。 |

### Files类

Files类可以使得普通文件操作变得快捷。

#### 读写文件

| 方法                                         | 说明                       |
| -------------------------------------------- | -------------------------- |
| byte[] readAllBytes(path)                    | 读取文件的所有内容         |
| List\<String> readAllLines(path,charset)     | 将文件当作行序列读入       |
| write(path,bytes)                            | 写出一个字符串到文件中     |
| write(path,bytes,StandardOpenOpetion.APPEND) | 向指定文件追加内容         |
| write(path,lines)                            | 将一个行的集合写出到文件中 |

#### 创建文件和目录

| 方法                    | 说明                                               |
| ----------------------- | -------------------------------------------------- |
| createDitectory(path)   | 路径中除最后一个部件外，其他部分都必须是已存在的。 |
| createDitectories(path) | 创建路径中的中间目录                               |
| createFile(path)        | 如果文件已经存在了，那么这个调用就会抛出异常。     |

