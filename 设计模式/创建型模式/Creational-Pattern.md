# 创建型模式（Creational Pattern）

创建型模式对类的实例化进行了抽象，能够将软件模块中对象的创建和对象的使用分离。为了使软件的结构更加清晰，外界对于这些对象只需要我们知道它们共同的接口，而不清楚具体实现细节，使整个系统的设计更加符合单一职责原则。

创建型模式在创建什么(What)，由谁创建(Who)，何时创建(When)等方面都为软件设计者提供了尽可能大的灵活性。创建型模式隐藏了类的实例的创建细节，通过隐藏对象如何被创建和组合在一起达到使整个系统独立的目的。

## 1、简单工厂模式（Simply Factory Pattern）

### 1.1 模式定义
简单工厂模式又称静态工厂方法（Static Factory Method）模式，属于类创建模式。在简单工厂模式中，可以根据参数的不同返回不同的类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。

### 1.2模式结构

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/10/1562728646127-1562728646133.png)

</div>

如图所示，简单工厂模式主要包括**工厂（Factory）**、**具体产品（ConcretProduct）**、**抽象产品（Product）** 三个角色。工厂角色负责实现创造所有实例的内部逻辑；具体产品角色是创建目标，所有创建的对象都充当这个角色的每个具体实例；抽象产品角色是所创建的所有对象的父类，负责描述所有实例所有的公共接口。下图为简单工厂模式的时序图。

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/10/1562729479420-1562729479424.png)

</div>

### 1.3 代码示例

- 创建抽象产品类（抽象类）：

```
public interface Product {
}
```
- 创建具体产品类：

```
public class ConcreteProduct1 implements Product {
}
public class ConcreteProduct1 implements Product {
}
public class ConcreteProduct1 implements Product {
}
```

- 创建工厂类来生成对象：

```
public class SimpleFactory {
    public Product createProduct(int type){
        if(type==1){
            return new ConcreteProduct1();
        }else if(type==1){
            return new ConcreteProduct2();
        }else {
            return new ConcreteProduct3();
        }
    }
}
```

- 在客户端使用工厂类创建对象：

```
public class Client {
    public static void main(String[] args){
        SimpleFactory sf = new SimpleFactory();
        Product product = sf.createProduct(1);
        //use product of type 1
    }
}
```

### 1.4 模式优缺点

优点：

- 将对象创建和对象本身业务分离可以降低系统的耦合度，使得两者修改起来都相对容易；
- 客户端无需知道所创建的具体产品的类名，只需要知道具体产品类所对应的参数；
- 通过引入配置文件，可以在不修改客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性。

缺点：

- 简单工厂模式最大的问题在于工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑；
- 使用简单工厂模式会增加系统中类的个数，在一定程度上增加了系统的复杂度；
- 系统扩展困难，一旦添加新产品就不得不修改工厂逻辑，在产品类型较多时，可能造成工厂逻辑过于复杂，不利于系统的扩展和维护。

综合简单工厂模式的上述优缺点，可以总结出使用简单工厂模式的主要应用场景为：

- 工厂类负责创建的对象比较少；
- 客户端只知道传入工厂类的参数，对于如何创建对象不关心。

## 2、工厂方法模式（Facatory Method Pattern）

### 2.1模式定义

工厂方法模式又称为工厂模式，也叫虚拟构造器（Virtual Constructor）模式或者多态工厂（Polymorphic Factory）模式，它属于类创造型模式。在工厂方法模式中，工厂父类负责定义创建产品对象的公共接口，而工厂子类则负责生成具体的产品对象，这样做的目的是将产品类的实例化操作延迟到工厂子类中完成，即通过工厂子类来确定究竟是哪一个具体产品类。通俗地来讲，就是在上述简单工厂模式地基础上，将工厂类分为抽象工厂类和具体工厂类，这种抽象化的结果使这种结构在不修改工厂类地情况下引进新的产品，从而使得工厂方法模型具有超越简单工厂模型地优越性，更加符合“开闭原则”。

### 2.2模式结构

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/10/1562760146610-1562760146946.png)

</div>

如图所示，工厂方法模式主要包含抽象产品（Product）、具体产品（ConcreteProduct）、抽象工厂（Factory）、具体工厂（ConcreteFactory）四个角色组成。与简单工厂模式类似，只是将工厂分为了抽象工厂和具体工厂，下图是工厂方法模式地时序图：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/10/1562760691056-1562760691061.png)

</div>

### 2.3代码示例

- 创建抽象产品类；
- 创建具体产品类;
- 创建抽象工厂类：

```

public abstract class Factory {
    abstract public Product factoryMethod();
    public void doSomething(){
        Product product = factoryMethod();
        //do something with the product
    }
}

```

- 创建具体工厂类：
（注：三个具体工厂类分别对应用来创建三个产品）

```
public class ConcreteFactory1 extends Factory{
    @Override
    public Product factoryMethod() {
        return new ConcreteProduct1();
    }
}
public class ConcreteFactory2 extends Factory{
    @Override
    public Product factoryMethod() {
        return new ConcreteProduct2();
    }
}
public class ConcreteFactory3 extends Factory{
    @Override
    public Product factoryMethod() {
        return new ConcreteProduct3();
    }
}
```

- 在客户端中使用产品：

```
public class Client {
    public static void main(String[] args){
        /*使用工厂方法模式*/
        Factory factory1 = new ConcreteFactory1();
        factory1.doSomething();
    }
}
```


### 2.4模式优缺点

工厂方法模式是简单工厂模式地进一步抽象和推广。由于使用了面向对象地多态性，工厂方法模式保持了简单工厂模式地优点，而且客服了它的一些缺点。

优点：
- 在工厂方法模式中，工厂方法用来创建客户所需要的产品，同时还向客户隐藏了哪种具体的产品类将被实例化这一细节，用户只需关心所需产品对应的工厂，无需关心创建细节，甚至无需知道具体产品的类名。
- 基于工厂角色和产品角色的多态设计是工厂方法模式的关键。它能够是工厂可以自主确定创建何种产品对象，而如何创建这个对象的细节则被完全封装在具体工厂内部。
- 在系统中加入新产品时，无需修改抽象工厂和抽象产品提供的接口，无需修改客户端，也无需修改其他具体工厂和具体产品，而只需要添加一个具体工厂和一个具体产品，使得系统拥有很好的扩展，完全符合“开闭原则”。

缺点：
- 在添加新产品时，需要编写新的具体产品类，而且还要提供与之对应的具体工厂类，系统中类的个数成对增加，在一定程度上增加了系统的复杂度，有更多的类需要编译和运行，会给系统带来一些额外的开销。
- 由于考虑到系统的可扩展性，需要引入抽象层，在客户端代码中均使用了抽象层进行定义，增加了系统的抽象性和理解难度。

基于上述优缺点，工厂方法模式在如下几个情况下比较适用：一个类不知道它所需要的对象的类，只需要知道对应的工厂；一个类通过其子类来创建对象。

## 3.抽象工厂模式（Abstract Factory）

### 3.1模式定义

抽象工厂模式：提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为Kit模式，属于对象创建型模式。通俗地，从与工厂方法模式比较来讲抽象工厂模式，在工厂方法模式中具体工厂负责生产具体的产品，每一个具体工厂对应一种具体产品，工厂方法也具有唯一性，一般情况下，一个具体工厂中只有一个工厂方法或者一组重载的工厂方法。当系统所提供的工厂所需生产的具体产品并不是一个简单的对象，而是多个位于不同产品等级结构中属于不同类型的具体产品时需要使用抽象工厂模式。因而抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形态。

### 3.2模式结构

抽象工厂模式包含的角色有：抽象工厂（AbstractFactory）、具体工厂（ConcreteFactory）、抽象产品（AbstractProduct）、具体产品（Product），如下图所示：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562811411788-1562811412304.png)

</div>

可以看到在抽象工厂模式结构中与工厂方法模式具有相同的四个角色，不同的是抽象工厂模式中的具体工厂可以创建多个产品等级(反应在本图中即为ProductA和ProductB)，构成了一个产品族。

### 3.3代码示例

- 创建不同产品等级的产品抽象类：

```
public interface ProductA {
}
public interface ProductB {
}
```

- 创建不同产品等级的具体产品类：


```
public class ConcreteProductA1 implements ProductA {
}
public class ConcreteProductA2 implements ProductA {
}
public class ConcreteProductB1 implements ProductB {
}
public class ConcreteProductB2 implements ProductB {
}
```

如上代码所示创建了两个不同的具体产品等级ConcreteProductA和ConcreteProductB。

- 创建抽象工厂类：


```
abstract public class AbstractFactory {
    abstract ProductA createProductA();
    abstract ProductB createProductB();
    public void doSomethingWithA(){
        ProductA productA = createProductA();
        //do something with ProductA
    }
    public void doSomethingWithB(){
        ProductB productB = createProductB();
        //do something with ProductB
    }
}
```

如上代码所示抽象工厂类AbstractFactory中定义了createProductA和createProductB两个抽象方法，子类（具体工厂类）通过实现这两个方法来创建两个不同产品等级（ProductA和ProductB）的具体产品。抽象工厂模式的时序图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562815908424-1562815908432.png)

</div>

- 创建具体工厂类：


```
public class ConcreteAbstractFactory1 extends AbstractFactory{
    @Override
    ProductA createProductA() {
        return new ConcreteProductA1();
    }

    @Override
    ProductB createProductB() {
        return new ConcreteProductB1();
    }
}
public class ConcreteAbstractFactory2 extends AbstractFactory{
    @Override
    ProductA createProductA() {
        return new ConcreteProductA2();
    }

    @Override
    ProductB createProductB() {
        return new ConcreteProductB2();
    }
}
```

如上代码所示具体工厂类ConcreteAbstractFactory1创建了两个不同产品等级的具体产品，ConcreteAbstractFactory2 同样也是。

- 在客户端中使用产品：

```
public class Client {
    public static void main(String[] args){
        /*使用抽象工厂模式*/
        AbstractFactory af = new ConcreteAbstractFactory1();
        af.doSomethingWithA();
        af.doSomethingWithB();
    }
}
```

### 3.4模式优缺点
抽象工厂模式的主要优点是：

- 隔离了具体类的生成，使得客户并不需要知道什么被创建，而且每次可以通过具体工厂类创建一个产品族中的多个对象，增加或者替换产品族比较方便，增加新的具体工厂和产品族很方便；

主要缺点在于：

- 增加新的产品等级结构很复杂，需要修改抽象工厂和所有的具体工厂类，对“开闭原则”的支持呈现倾斜性。

基于以上特点抽象工厂模式的主要适用情况包括：一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节；系统中有多于一个的产品族，而每次只使用其中某一产品族；属于同一个产品族的产品将在一起使用；系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。

## 4.建造者模式（Builder Pattern）

### 4.1模式定义

建造者模式：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。建造者模式是一步步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。建造者模式属于对象创建型模式。

### 4.2模式结构

建造者模式主要包含抽象建造者（Builder）、具体建造者（ConcreteBuilder）、指挥者（Director）、产品（Product）四个角色。建造者模式类图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562828060455-1562828060463.png)

</div>

如图所示，抽象建造者类中定义了产品的创建方法和返回方法；建造者模式的结构中还引入了一个指挥者Director，该类的作用主要有两个：一方面隔离了客户与生产过程；另一方面负责控制产品的生成过程。指挥者针对抽象建造者编程，客户端只需要知道具体建造者的类型，即可以通过指挥者类调用建造者的相关方法，返回一个完整的产品对象。以下是建造者模式创建和使用一个产品对象的时序图：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562828946333-1562828946337.png)

</div>

### 4.3代码示例

这里的代码示例使用一个建造者模式用于描述KFC如何创建套餐的情景来进行说明。套餐是一个复杂的对象，它一般包括主食（如汉堡、鸡肉卷等）和饮料（如果汁、可乐等）组成部分，不同的套餐有不同的组成成分，而KFC的服务员可以根据顾客的要求，一步一步装配这些组成部分，构造一份完整的套餐，然后返回给顾客。其结构的类图为：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562845907133-1562845907138.png)

</div>

- 建造者模式的产品类，对应在本例中即为套餐类：

```
public class Meal {
    //套餐内包含食物和饮料
    private String food;
    private String drink;
    public void setFood(String food) {
        this.food = food;
    }
    public void setDrink(String drink) {
        this.drink = drink;
    }
    public String getFood() {
        return food;
    }
    public String getDrink() {
        return drink;
    }
}
```

- 抽象建造者类:

```
abstract public class Builder {
    abstract void createFood();
    abstract void createDrink();
    abstract Meal getMeal();
}
```

- 具体建造者类：

```
public class ConcreteBuilderA extends Builder{
    private Meal meal = new Meal();
    @Override
    void createFood() {
        meal.setFood("hamburger");
    }
    @Override
    void createDrink() {
        meal.setDrink("Coco cola");
    }
    @Override
    Meal getMeal() {
        return meal;
    }
}

public class ConcreteBuilderB extends Builder{
    private Meal meal = new Meal();
    @Override
    void createFood() {
        meal.setFood("french fries");
    }

    @Override
    void createDrink() {
        meal.setDrink("Pepsi cola");
    }

    @Override
    Meal getMeal() {
        return meal;
    }
}
```

如代码所示，两个具体建造者类分别用来创建两种不同的套餐（A和B）。

- 指挥者类：

```
public class Director {
    private Builder builder;
    public Director(Builder builder){
        this.builder = builder;
    }
    public Meal buildMeal(){
        builder.createFood();
        builder.createDrink();
        return builder.getMeal();
    }
}
```

如代码所示，指挥者类首先利用构造函数初始化指挥者类中的建造者，然后利用它来建造具体的产品（套餐）。

- 在客户端中使用：

```
public class Client {
    public static void main(String[] args){
        /*使用建造者模式*/

        //创建汉堡加可口可乐的套餐A
        Builder builderA = new ConcreteBuilderA();
        Director director = new Director(builderA);
        Meal mealA = director.buildMeal();
        //创建汉堡加可口可乐的套餐A
        Builder builderB = new ConcreteBuilderA();
        director = new Director(builderB);
        Meal mealB = director.buildMeal();
    }
}
```

### 4.4模式优缺点

- 建造者模式的主要优点在于客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象，每一个具体建造者都相对独立，而与其他的具体建造者无关，因此可以很方便地替换具体建造者或增加新的具体建造者，符合“开闭原则”，还可以更加精细地控制产品的创建过程；
- 其主要缺点在于由于建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，因此其使用范围受到一定的限制，如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大。

基于上述几点，建造者模式一般的适用情况包括：需要生成的产品对象有复杂的内部结构，这些产品对象通常包含多个成员属性；需要生成的产品对象的属性相互依赖，需要指定其生成顺序；对象的创建过程独立于创建该对象的类；隔离复杂对象的创建和使用，并使得相同的创建过程可以创建不同类型的产品。

## 5.单例模式（Singleton Pattern）

### 5.1模式定义

单例模式：单例模式确保某一个类只有一个实例，而且自行实例化，并向整个系统提供这个实例。这个类称为单例类，它提供全局的访问方法。其要点有以下三条：

- 某个类只能有一个实例；
- 这个类必须自行创建这个实例；
- 必须自行向整个系统提供这个实例。

单例模式是一种对象创建型模式。

### 5.2模式结构

单例模式包含的角色主要为：单例（Singleton）。单例模式的类图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562848464265-1562848464269.png)

</div>

单例模式的目的是保证一个类仅有一个实例，并提供一个访问它的全局访问点。单例模式包含的角色只有一个，就是单例类——Singleton。单例类拥有一个私有构造函数，确保用户无法通过new关键字直接实例化它。除此之外，该模式中包含一个静态私有成员变量与静态公有的工厂方法，该工厂方法负责检验实例的存在性并实例化自己，然后存储在静态成员变量中，以确保只有一个实例被创建。单例模式的时序图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562848818040-1562848818045.png)

</div>

### 5.3代码示例

以下给出在Java中常见的几种单例模式的实现方式。

- 懒汉式：

```
public class Singleton1 {
    //懒汉式
    private static Singleton1 uniqueInstance;
    private Singleton1(){}
    public static Singleton1 getUniqueInstance(){
        if(uniqueInstance == null){
            uniqueInstance  = new Singleton1();
        }
        return uniqueInstance;
    }
}
```
懒汉式实现方式中，私有静态变量uniqueInstance被延迟实例化，这样当没有用到该类时就不会被实例化，从而节约了资源。这个实现在多线程的情况下是不安全的如果多个线程能够同时进入 if (uniqueInstance == null) ，并且此时 uniqueInstance 为 null，那么会有多个线程执行 uniqueInstance = new Singleton(); 语句，这将导致实例化多次 uniqueInstance。

- 饿汉式


```
public class Singleton2 {
    //饿汉式
    private static Singleton2 uniqueInstance = new Singleton2();
    private Singleton2(){}
    public static Singleton2 getUniqueInstance(){
        return uniqueInstance;
    }
    public void testPrint(){
        System.out.println("ok");
    }
}

```

直接讲私有静态变量 uniqueInstance 实例化，这样便不会产生线程安全的问题，但是此种方式也丢失了延迟实例化带来的节约资源的好处。

- 线程安全的懒汉式

```
public class Singleton3 {
    private static Singleton3 uniqueSingleton3;
    private Singleton3(){};
    public synchronized static Singleton3 getUniqueSingleton3(){
        if(uniqueSingleton3==null){
            uniqueSingleton3 = new Singleton3();
        }
        return uniqueSingleton3;
    }
}

```

使用synchronized关键字进行同步公共静态方法getUniqueSingleton3，从而保证线程安全。

- 双重校验锁懒汉式（线程安全）

```
public class Singleton4 {
    //使用volatile关键字禁止JVM指令重排
    private volatile static Singleton4 uniqueSingleton4;
    private Singleton4(){};
    public static Singleton4 getUniqueSingleton4(){
        if(uniqueSingleton4==null){
            //双重校验同步
            synchronized (Singleton4.class){
                if(uniqueSingleton4==null){
                    uniqueSingleton4 = new Singleton4();
                }
            }
        }
        return uniqueSingleton4;
    }
}

```

uniqueInstance 只需要被实例化一次，之后就可以直接使用了。加锁操作只需要对实例化那部分的代码进行，只有当 uniqueInstance 没有被实例化时，才需要进行加锁。双重校验锁先判断 uniqueInstance 是否已经被实例化，如果没有被实例化，那么才对实例化语句进行加锁。使用 volatile 可以禁止 JVM 的指令重排，保证在多线程环境下也能正常运行。

- 静态内部类实现

```
public class Singleton5 {
    private Singleton5(){}
    private static class SingletonHolder{
        private static final Singleton5 INSTANCE = new Singleton5();
    }
    public static Singleton5 getUniqueInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

当 Singleton 类被加载时，静态内部类 SingletonHolder 没有被加载进内存。只有当调用 getUniqueInstance() 方法从而触发 SingletonHolder.INSTANCE 时 SingletonHolder 才会被加载，此时初始化 INSTANCE 实例，并且 JVM 能确保 INSTANCE 只被实例化一次。这种方式不仅具有延迟初始化的好处，而且由 JVM 提供了对线程安全的支持。

### 5.4模式优缺点

- 单例模式的主要优点在于提供了对唯一实例的受控访问并可以节约系统资源；
- 其主要缺点在于因为缺少抽象层而难以扩展，且单例类职责过重。

单例模式适用情况包括：系统只需要一个实例对象；客户调用类的单个实例只允许使用一个公共访问点。





















