# 行为参数化

java8中的行为参数化，解决了数据处理需求变化的问题，通过将一个写定的代码块作为参数传递给另一方法，则该方法的行为就基于被参数化了的代码块。

如果没有已经写定的方法，使用行为参数化，需要大量的模板代码，而Lambda则用于解决这个问题。

该节给出了代码示例，讲解如何对代码改进，应对不断变化的需求。

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

这代码的实现很烂，如果查找的人不要求65岁，需要重复修改，因此加了一个参数`int age` 替换了65。接下来需求开始变化，需要从代码中查找**姓“王”的人**，为了避免复制黏贴，通过加个`String name`参数，添加`boolean flag`参数区别是查找年龄还是姓名来解决。

最终代码是这样的：

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

这样的代码实现很糟糕，无法让调用者清楚知道方法各参数的含义，且无法适应其他选择。

#### 2. 行为参数化

> 让方法接受多种行为（或战略）作为参数，并在内部使用，来完成不同的行为。

一种可能的解决方案是对选择标准建模：需要的Person，根据其属性返回一个boolean值。我们把这样的选择标准称为谓词(返回一个boolean值的函数)。

定义接口实现该选择标准，接口的不同实现代表不同的选择，从而令`selectPeople`方法只需要两个参数。这样的解决方式称为行为参数化。

行为参数化的好处在于你可以把迭代要筛选的集合的逻辑与对集合中每个元素应用的行为区分开来。

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

传入参数有以下方案：

1. 类实例化传参

   每次需要一个选择逻辑，先定义相应的类，再将类实例化，传给`selectPeople`方法。

   ```java
   class PersonSelectedByName implements PersonPredict{
       public boolean test(Person person){
           return person.getName().startsWith("王");
       }
   }
   ```

   这样的实现解决了代码的复用性问题，无论需求是什么，只要额外定义一个类，而无需改变原有代码。可是这种只实例化一次的类，给出类的定义，真的有必要吗？

2. 匿名类

   需要类的时候直接实例化它。

   ```java
   //3. also using the select method 2 but anonymity class
   System.out.println(selectPeople(people,new PersonPredict(){
       public boolean test(Person person){
           return person.getTel().startsWith("187");
       }
   }));
   ```

   然而这种实现也重复了很多，为了一个方法，不得不写许多格式化代码。

3. lambda

   将Lambda作为参数传给方法。

   ```java
   //4. using lambda expression as the arguments
   System.out.println(selectPeople(people,person->person.getTel().startsWith("130")&&person.getAge()>30));
   ```

### 泛型

通过使用泛型，使集合中可以是其他类型的对象，增加了代码的适用性。

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


### 辅助代码

一个记载成员信息的列表，实现如下：

```java
public static final ArrayList<Person> people = new ArrayList<>();
static{
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
