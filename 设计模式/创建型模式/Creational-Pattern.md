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

<div align="center">
```
public interface ProductA {
}
public interface ProductB {
}
```
</div>

- 创建不同产品等级的具体产品类：

<div align="center">

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
</div>

如上代码所示创建了两个不同的具体产品等级ConcreteProductA和ConcreteProductB。

- 创建抽象工厂类：

<div align="center">

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

</div>

如上代码所示抽象工厂类AbstractFactory中定义了createProductA和createProductB两个抽象方法，子类（具体工厂类）通过实现这两个方法来创建两个不同产品等级（ProductA和ProductB）的具体产品。抽象工厂模式的时序图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/11/1562815908424-1562815908432.png)

</div>

- 创建具体工厂类：

<div align="center">

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

</div>

如上代码所示具体工厂类ConcreteAbstractFactory1创建了两个不同产品等级的具体产品，ConcreteAbstractFactory2 同样也是。

- 在客户端中使用产品：

<div align="center">
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
</div>

### 3.4模式优缺点









