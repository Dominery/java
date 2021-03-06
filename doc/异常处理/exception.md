# 常见异常

* 运行时异常(非受检异常)

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

* 编译时异常(受检异常)

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

  