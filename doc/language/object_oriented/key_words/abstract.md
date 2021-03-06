# abstract

`abstract` 用来修饰类、方法，不能修饰属性、构造器等结构，也不能修饰私有方法、静态方法、被`fianl`修饰的类或方法。

如果一个类被`abstract`修饰，那么这个类被称为抽象类。

如果一个方法被`abstract`修饰，那么这个类被称为抽象方法。

* 抽象类的特点
  1. 不能实例化
  2. 抽象类中必存在构造器，便于子类实例化调用
  3. 使用抽象类必须提供子类，对子类实例化后进行调用
* 抽象方法的特点
  1. 没有方法体，只有声明
  2. 包含抽象方法的类一定是抽象类
  3. 只有子类重写了抽象类的所有抽象方法，该子类才可以被实例化
  4. 抽象类可以继承抽象类，无需重写抽象方法

