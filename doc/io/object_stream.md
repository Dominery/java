# 对象流

对象流解决了数据流只能对基本数据类型进行传输，无法传输对象的问题。对象流可以把java中的对象存储到数据源中，或者从数据源中读取出java对象，这两个过程称为序列化和反序列化。

> 序列化：用ObjectOutputStream类保存基本数据类型或对象的机制。
>
> 反序列化：用ObjectOutputStream类读取基本数据类型或对象的机制。

### 对象序列化

如果一个类实现了Serializable或Externalizable接口，那么该类实例化的对象与此对象对应的二进制流之间可以相互转换，从而实现传输对象的目的。我们把java对象能与二进制流之间相互转换的能力称为**对象序列化机制**。

### 对象流操作

对象流操作与流处理的流程一致，如下展示将List\<String\>对象保存到磁盘，再将其读取到内存的过程。

```java
@Test
public void testObjectOutputStream(){
    ObjectOutputStream objectOutputStream = null;
    try {
        objectOutputStream = new ObjectOutputStream(new FileOutputStream("data.dat"));
        objectOutputStream.writeObject(Arrays.asList("hello","world","who","do","you","love"));
        objectOutputStream.flush();
    } catch (IOException exception) {
        exception.printStackTrace();
    } finally {
        try {
            if(objectOutputStream!=null)
                objectOutputStream.close();
        } catch (IOException exception) {
            exception.printStackTrace();
        }
    }
}

@Test
public void testObjectInputStream(){
    ObjectInputStream ois = null;
    try {
        ois = new ObjectInputStream(new FileInputStream("data.dat"));
        Object o = ois.readObject();
        List<String> s = (List<String>) o;
        System.out.println(s);
    } catch (IOException exception) {
        exception.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } finally {
        try {
            if(ois!=null)
                ois.close();
        } catch (IOException exception) {
            exception.printStackTrace();
        }
    }
}
```

