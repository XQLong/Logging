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

### 1.5 代码示例

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

