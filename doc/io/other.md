# 其他流

### 标准输入、输出流

`System.in`:标准输入流，默认从控制台输入

`System.out`:标准输出流，默认从控制台输出

> System类的setIn()/setOut()方法可以重新指定标准输入和输出流

读取键盘输入

```java
public static String next(){
    BufferedReader bis = new BufferedReader(new InputStreamReader(System.in));
    String str = "";

    try {
        str = bis.readLine();
    } catch (IOException exception) {
        exception.printStackTrace();
    }
    return str;
}
```

### 打印流

实现将基本数据类型的数据格式转化为字符串输出。

`PrintStream`和`PrintWriter`都是输出流。

`System.out`是一个`PrintStream`对象，调用的是打印流中的方法。

`PrintStream`构造器可以传入OutputStream对象，再通过`System.setOut()`方法可以重新指定标准输出流。

```java
try (PrintStream printStream = new PrintStream(new FileOutputStream("hello.txt"))) {
    System.setOut(printStream);
    System.out.println("hello I'm suyu what's your name");
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

### 数据流

`DataInputStream`和`DataOutputStream`是数据流的类，属于处理流，可以读取或写出基本数据类型和String的数据。

存入与读取时需要顺序、类型相同。

```java
@Test
public void testDataOutStream(){
    try (DataOutputStream dataOutputStream = new DataOutputStream(new FileOutputStream("data.txt"))) {
        dataOutputStream.writeInt(23);
        dataOutputStream.writeUTF("hello");
        dataOutputStream.writeBoolean(true);
    } catch (IOException exception) {
        exception.printStackTrace();
    }

}
@Test
public void testDataInputStream(){
    try (DataInputStream dis = new DataInputStream(new FileInputStream("data.txt"))) {
        int i = dis.readInt();
        String s = dis.readUTF();
        boolean b = dis.readBoolean();

        System.out.println("int:" + i + "\nstring:" + s + "\nboolean:" + b);
    } catch (IOException exception) {
        exception.printStackTrace();
    }
}
```

