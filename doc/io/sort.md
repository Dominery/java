# 流的分类

| 分类标准 | 分类内容                      |
| -------- | ----------------------------- |
| 数据单位 | 字节流(8 bit)、字符流(16 bit) |
| 数据流向 | 输入流、输出流                |
| 流的角色 | 节点流、处理流                |

> 字符流，即流的数据单位是char类型，java中char类型一般为2个字节。

## 流的体系结构

| 抽象基类     | 节点流(文件流)   | 缓冲流               |
| ------------ | ---------------- | -------------------- |
| InputStream  | FileInputStream  | BufferedInputStream  |
| OutputStream | FileOutputStream | BufferedOutputStream |
| Reader       | FileReader       | BufferedReader       |
| Writer       | FileWriter       | BufferedWriter       |

