# 对象流

对象流解决了数据流只能对基本数据类型进行传输，无法传输对象的问题。对象流可以把java中的对象存储到数据源中，或者从数据源中读取出java对象，这两个过程称为序列化和反序列化。

> 序列化：用ObjectOutputStream类保存基本数据类型或对象的机制。
>
> 反序列化：用ObjectOutputStream类读取基本数据类型或对象的机制。

### 对象序列化

如果一个类实现了Serializable或Externalizable接口，那么该类实例化的对象与此对象对应的二进制流之间可以相互转换，从而实现传输对象的目的。我们把java对象能与二进制流之间相互转换的能力称为**对象序列化机制**。

#### 自定义类实现序列化

自定义类如果要实现序列化，需要满足以下条件：

1. 实现Serializable接口
2. 自定义类提供serialVersionUID全局常量
3. 自定义类中的所有属性必须保证都是可序列化的

> Serializable接口是标识接口，没有抽象方法声明。
>
> SerialVersionUID，作为类的唯一标识，可以表明序列化版本间的兼容性。如果没有显式声明该静态常量，java运行时会自动生成，如果类的实例变量作了修改，该静态常量会发生改变。
>
> 反序列化时，JVM会把传来的字节流的UID与本地相应实体类的UID进行比较，如果不同，就会出现InvalidCastException异常。

如果类的某个成员变量使用static或transient修饰，则该成员变量不可序列化。

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

