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

### FileReader

文件流读取可能会抛出异常，为保证流资源最后一定关闭，防止内存泄漏，需要使用try-catch/finally异常处理。

read()方法返回读入的字符数据，如果达到文件末尾，返回-1。

```java
FileReader fr = null;
try{
	File file = new File("hello.txt"); //1.construct a File object
    fr = new FileReader(file);//2. construct a Reader Object

    int data; 
    while((data=fr.read())!=-1){ // 3. read data from stream
        System.out.println((char)data);
    }
    /*another read()
    int len;
    char[] cbuf = new char[1024];
    while((len=fr.read(cbuf))!=-1){
    	for(int i=0;i<len;i++){
    		System.out.print(cbuf[i]);
    	}
    	String str = new String(cbuf,0,len);
    	System.out.print(str);
    }
    */
}catch(IOException ex){
    ex.printStackTrace();
}finally{
    try{
        if(fr!=null)
       	fr.close(); // 4. close the stream   
    }catch(IOException ex){
        ex.printStackTrace();
    }
}
```

### FileWriter

* 判断File对应的文件是否存在
  * 如果不存在，在输出时，自动创建该文件，写入内容
  * 如果存在
    * 如果流使用构造器为FileWriter(file,true)，在文件中追加内容
    * 如果流使用构造器为FileWriter(file)，覆盖文件