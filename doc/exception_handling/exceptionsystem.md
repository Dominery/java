# 异常体系

异常处理的任务就是将控制权从错误产生的地方转移给能够处理这种情况的错误处理器。

所有的异常都是由Throwable继承而来，但在下一层立即分解为两个分支：Error和Exception。

* Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误。Error不编写代码进行处理。
* Exception层次结构又分解为两个分支：一个分支派生于RuntimeException；另一个分支包含其他异常。

  划分两个分支的规则是：由程序错误导致的异常属于RuntimeException，称为运行时异常；而程序本身没有问题，但像I/O错误这类问题导致的异常属于编译时异常。

Java语言规范将派生于Error类或RuntimeException类的所有异常称为非受检（unchecked）异常，所有其他的异常称为受检（checked）异常。

### 常见的异常

* 运行时异常\(非受检异常\)

  > unchecked Exception/Error
  >
  > |----NullPointerException
  >
  > |----IndexOutOfBoundsException
  >
  > ​		|----ArrayIndexOutOfBoundsException
  >
  > ​		|----StringIndexOutOfBoundsException
  >
  > |----ClassCastException
  >
  > |----NumberFormatException
  >
  > |----InputMismatchException

  ```java
    public class ExceptionTest{
        //NullPointerExcetion
        @Test
        public void test1(){
            int[] m = null;
            m[0];
        }
        //IndexOutOfBoundsException
        @Test
        public void test2(){
            int[] m = new int[3];
            m[3];//ArrayIndexOutOfBoundsException
            String n = "abc";
            n.charAt(3);//StringIndexOutOfBoundsException
        }
        //ClassCastException
        @Test
        public void test3(){
            Object obj = new Date();
            String str = (String) obj;
        }
        //NumberFormatException
        @Test
        public void test4(){
            String n = "abc";
            Integer.parseInt(n);
        }
        //InputMismatchException
        @Test
        public void test5(){
            Scanner scanner = new Scanner(System.in);
            scanner.nextInt(); // if the input string is not numberal,Exception will occur
        }
  
    }
  ```

* 编译时异常\(受检异常\)

  > checked Exception
  >
  > |----IOException
  >
  > ​		|----FileNotFoundException

  ```java
  @Test
  public void tesgt6(){
      File file = new File("hello.txt");
      FileInputStream fis = new FileInputStream(file);//FileNotFoundException
      int data = fis.read();//IOException
      while(data != -1){
          System.out.println((char)data);
          data = fis.read();
      }
      fis.close();
  }
  ```

