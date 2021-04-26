# 枚举类的使用

如果类的对象只有确定的有限个数，那么把这个类称为枚举类。

如果枚举类中只有一个对象，那么该枚举类可以作为单例模式的实现方式。

### 定义枚举类

* 自定义枚举类(jdk5.0以前)

  ```java
  class Season{
      //1.make the constructor private
      private Season(String seasonName){
          this.seasonName = seasonName;
      };
      private final String seasonName;
      //2.initialise objects with static and final
      public static final Season SPRING = new Season("spring");
      public static final Season SUMMER = new Season("summer");
      public static final Season AUTUMN = new Season("autumn");
      public static final Season WINTER = new Season("winter");
      
      public String toString(){
          return seasonName;
      }
  }
  ```

* 使用关键字`enum`

  定义的枚举类默认继承于`java.lang.Enum`。

  ```java
  enum Season{
      SPRING("spring"),SUMMER("summer"),AUTUMN("autumn"),WINTER("winter");
      private final String seasonName;
      private Season(String seasonName){
          this.seasonName = seasonName;
      }
  }
  ```

### Enum的主要方法

| 方法名               | 说明                                 |
| -------------------- | ------------------------------------ |
| values()             | 返回枚举类型的对象数组               |
| valuesOf(String str) | 可以把一个字符串转为对应的枚举类对象 |
| toString()           | 返回当前枚举类对象常量的名称         |



### 实现接口的枚举类

1. 在枚举类中重写抽象方法
2. 让枚举类的对象分别实现抽象方法

```java
enum Season implements Info{
    SPRING("spring"){
        public void show(){}
    }
        ,SUMMER("summer"){
            public void show(){}
        },AUTUMN("autumn"){
            public void show(){}
        },WINTER("winter");
    private final String seasonName{
        public void show(){}
    };
    private Season(String seasonName){
        this.seasonName = seasonName;
    }
}
interface Info{
    void show();
}
```

