# 流的分类

| 分类标准 | 分类内容                      |
| -------- | ----------------------------- |
| 数据单位 | 字节流(8 bit)、字符流(16 bit) |
| 数据流向 | 输入流、输出流                |
| 流的角色 | 节点流、处理流                |

> 字符流，即流的数据单位是char类型，java中char类型一般为2个字节。

### 流的体系结构

| 抽象基类     | 节点流(文件流)   | 缓冲流               |
| ------------ | ---------------- | -------------------- |
| InputStream  | FileInputStream  | BufferedInputStream  |
| OutputStream | FileOutputStream | BufferedOutputStream |
| Reader       | FileReader       | BufferedReader       |
| Writer       | FileWriter       | BufferedWriter       |

对于文本文件，使用字符流处理。对于非文本文件，使用字节流处理。

> 使用字节流处理文本文件可能出现乱码，因为一个字符不一定为1个字节，如果一个字符是多个字节存储的，那么字节流可能会截断该字符，如果输出到控制台会导致乱码。

### 节点流

#### 输入流以FileReader为例

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

#### 输出流以FileWriter为例

* 判断File对应的文件是否存在
  * 如果不存在，在输出时，自动创建该文件，写入内容
  * 如果存在
    * 如果流使用构造器为FileWriter(file,true)，在文件中追加内容
    * 如果流使用构造器为FileWriter(file)，覆盖文件

```java
FileWriter writer = null;
try {
    File file = new File("hello1.txt");

    writer = new FileWriter(file);

    writer.write("hello how are you");
    writer.write("I'm fine,thank you and you?");
} catch (IOException exception) {
    exception.printStackTrace();
} finally {
    try {
        if(writer!=null)
            writer.close();
    } catch (IOException exception) {
        exception.printStackTrace();
    }
}
```

### 处理流

#### 缓冲流

缓冲流内部提供了缓冲区，可以提高流的读取和写入速度，开发上一般使用缓冲流。

流关闭操作需要从外层流到内层流，关闭外层流时，内层流同时会关闭。

* 字节流

    ```java
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try {
        File file = new File("little_prince.png");
        File file1 = new File("little_prince2.png");

        FileInputStream fis = new FileInputStream(file);
        FileOutputStream fos = new FileOutputStream(file1);

        bis = new BufferedInputStream(fis);
        bos = new BufferedOutputStream(fos);

        byte[] bytes = new byte[1024];
        int len;
        while ((len=bis.read(bytes))!=-1){
            bos.write(bytes,0,len);
        }
    } catch (IOException exception) {
        exception.printStackTrace();
    } finally {
        try {
            if(bis!=null)bis.close();
        } catch (IOException exception) {
            exception.printStackTrace();
        }
        try {
            if(bos!=null) bos.close();
        } catch (IOException exception) {
            exception.printStackTrace();
        }
    }
    ```

* 字符流

  字符流读取操作除了char[]数组之外，可以使用readLine()方法。

  ```java
  String data;
  while((data = br.readLine())!=null){
      bw.write(data+"\n");
      /*
      bw.write(data);
      bw.newLine();
      */
  }
  ```

#### 转换流

