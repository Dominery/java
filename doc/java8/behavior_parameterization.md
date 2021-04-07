# java行为参数化

我们即使还未见过高质量代码的实现，但依旧可以想象出它的模样——灵活性、可维护性、可拓展性、低耦合。java中的行为参数化提供了一条通往优雅代码的路径，如果你有对代码质量的追求，那么请和我一起探寻路的尽头。

### 辅助代码

我们会使用一个记载成员信息的列表，实现如下：

```java
public static final ArrayList<Person> people = new ArrayList<>();
public static void addPersonInfo(){
    people.add(new Person(12,"王明","123456777"));
    people.add(new Person(45,"李四","1872445533"));
    people.add(new Person(35,"张三","13028985392"));
    people.add(new Person(78,"王五","13029839499"));
    people.add(new Person(65,"李达","12359750690"));
    people.add(new Person(2,"张一","18792490430"));
}
```

其中person类的定义如下：

```java
public class Person {
    private final int age;
    private final String name;
    private final String tel;
    public Person(int age,String name,String tel){
        this.age = age;
        this.name = name;
        this.tel = tel;
    }

    public int getAge(){
        return age;
    }

    public String getName(){
        return name;
    }

    public String getTel(){
        return tel;
    }

    @Override
    public String toString() {
        return "Person{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", tel='" + tel + '\'' +
                '}';
    }
}
```

### 代码之旅

#### 1. 多参数函数

现在假设你需要**查找年龄在65及以上的人** ，如下的代码将会在一瞬间蹦入你的脑海：

```java
// 1. the basic select method which is inflexible and repeated
public static ArrayList<Person> selectOldPeople(ArrayList<Person>people){
    ArrayList<Person>selectedPeople = new ArrayList<>();
    for(Person person:people){
        if(person.getAge()>=65){
            selectedPeople.add(person);
        }
    }
    return selectedPeople;
}
```

你知道这代码的实现很烂，如果查找的人不要求65岁，岂非要重复修改，所以你加了一个参数`int age` 替换了65。可是没想到接下来，你需要查找**姓“王”的人**，为了避免复制黏贴，你心想也许可以加个`String name`参数，可是为了区别是查找年龄还是姓名，你又不得不添加了`boolean flag`参数。

你的最终代码是这样的：

```java
public static ArrayList<Person> selectPeople(ArrayList<Person>people,int age,String name,boolean flag){
    ArrayList<Person>selectedPeople = new ArrayList<>();
    for(Person person:people){
        if(flag&&person.getAge()>=age||!flag&&person.getName().startwith(name)){
            selectedPeople.add(person);
        }
    }
    return selectedPeople;    
}
```

#### 2. 行为参数化

不，也许我可以把判断的逻辑与迭代分开，你心想。于是你设计了一个接口，从而令`selectPeople`方法只需要两个参数。

```java
interface PersonPredict{
    boolean test(Person person);
}
//2. using interface to separate the iteration and the element judgement
//   however, the select method is heavy because the class only is constructed once
public static ArrayList<Person> selectPeople(ArrayList<Person> people,PersonPredict predict){
    ArrayList<Person>selectedPeople = new ArrayList<>();
    for(Person person:people){
        if(predict.test(person)){
            selectedPeople.add(person);
        }
    }
    return selectedPeople;
}
```

可是如何传入参数呢？你想到了以下方案：

1. 类实例化传参

   每次需要一个选择逻辑，你先定义相应的类，再将类实例化，传给`selectPeople`方法。

   ```java
   class PersonSelectedByName implements PersonPredict{
       public boolean test(Person person){
           return person.getName().startsWith("王");
       }
   }
   ```

   这样的实现解决了代码的复用性问题，无论你的需求是什么，只要额外定义一个类，而无需改变原有代码。可是这种只实例化一次的类，给出类的定义，真的有必要吗？于是你想到了如下方法。

2. 匿名类

   我需要类的时候直接实例化它岂不是完美？你实现了下述代码：

   ```java
   //3. also using the select method 2 but anonymity class
   System.out.println(selectPeople(people,new PersonPredict(){
       public boolean test(Person person){
           return person.getTel().startsWith("187");
       }
   }));
   ```

   然而这种实现似乎也重复了很多，为了一个方法，你不得不写许多格式化代码。还有其他方案吗？

3. lanbda表达式

   你逐渐看到了路的尽头，将一个方法作为参数。

   ```java
   //4. using lambda expression as the arguments
   System.out.println(selectPeople(people,person->                    person.getTel().startsWith("130")&&person.getAge()>30));
   ```

   代码变得更加简单与优雅，似乎你已经走到了路的尽头。别急，如果你需要的选择不是`Person`而是`Car`呢？

4. 泛型

   ```java
   public static <T>List<T> filter(List<T> list,Predict<T>p){
       List<T> result = new ArrayList<>();
       for(T e:list){
           if(p.test(e)){
               result.add(e);
           }
       }
       return result;
   }
   // 5. make more abstract interface which can be offered any class
   interface Predict<T>{
       boolean test(T t);
   }
   ```

   现在，你终于看到了，不论是`Person`还是`Car`类，只要它是列表中的元素，只要你的需求是作出选择，这个代码实现能够完美契合后来的变动。

