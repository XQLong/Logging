# 结构型模式（Structural Pattern）

结构型模式描述如何将类或者对象结合在一起形成更大的结构，就像搭积木，可以通过简单积木的组合形成复杂的、功能更为强大的结构。结构型模式可以分为类结构型模式和对象结构型模式：

- 类结构型模式关心类的组合，由多个类可以组合成一个更大的系统，在类结构型模式中一般只存在继承关系和实现关系。
- 对象结构型模式关心类与对象的组合，通过关联关系使得在一个类中定义另一个类的实例对象，然后通过该对象调用其方法。 根据“合成复用原则”，在系统中尽量使用关联关系来替代继 承关系，因此大部分结构型模式都是对象结构型模式。

## 1.适配器模式(Adapter Pattern)

### 1.1模式定义

适配器模式：将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作，其别名为包装器(Wrapper)。适配器模式既可以作为类结构型模式，也可以作为对象结构型模式。

### 1.2模式结构

- 类适配器模式

类的适配器模式把适配的类的API转换成为目标类的API。其结构图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/12/1562898115905-1562898116113.png)

</div>

在上图中可以看出，Adaptee类并没有sampleOperation2()方法，而客户端则期待这个方法。为使客户端能够使用Adaptee类，提供一个中间环节，即类Adapter，把Adaptee的API与Target类的API衔接起来。Adapter与Adaptee是继承关系，这决定了这个适配器模式是类的。模式中所涉及到的角色主要包括目标(Target)、源（Adaptee）、适配器（Adapter）三个角色：目标就是所期待得到的接口；源就是需要适配的接口；适配器把源接口转换成目标接口。

- 对象适配器模式

　与类的适配器模式一样，对象的适配器模式把被适配的类的API转换成为目标类的API，与类的适配器模式不同的是，对象的适配器模式不是使用继承关系连接到Adaptee类，而是使用委派关系连接到Adaptee类。其结构图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/12/1562901303308-1562901303313.png)

</div>

上图可以看出，Adaptee类并没有sampleOperation2()方法，而客户端则期待这个方法。为使客户端能够使用Adaptee类，需要提供一个包装(Wrapper)类Adapter。这个包装类包装了一个Adaptee的实例，从而此包装类能够把Adaptee的API与Target类的API衔接起来。Adapter与Adaptee是委派关系，这决定了适配器模式是对象的。

### 1.3代码示例

- 类适配器模式


目标类（所期待得到的接口）:

```
public interface Target {
    /*源类Adaptee中有的方法*/
    public void sampleOperation1();

    /*源类Adaptee中没有的方法*/
    public void sampleOperation2();
}

```

源类(需要适配的接口):

```
public class Adaptee {
    public void sampleOperation1(){}
}
```

适配器类：

```
public class Adapter extends Adaptee implements Target{
    @Override
    public void sampleOperation2() {

    }
}
```

适配器角色Adapter继承了Adaptee,同时又实现了目标(Target)接口。由于Adaptee没有提供sampleOperation2()方法，而目标接口又要求这个方法，因此适配器角色Adapter实现了这个方法。

- 对象适配器模式



目标类（所期待得到的接口），与类适配器模式相同:

```
public interface Target {
    /*源类Adaptee中有的方法*/
    public void sampleOperation1();

    /*源类Adaptee中没有的方法*/
    public void sampleOperation2();
}

```


源类(需要适配的接口)，与类适配器模式相同:

```
public class Adaptee {
    public void sampleOperation1(){}
}
```

适配器类：

```
public class Adapter_Obj implements Target{
    private Adaptee adaptee;
    public Adapter_Obj(Adaptee adaptee){
        this.adaptee = adaptee;
    }
    //源类Adaptee有方法sampleOperation1,因此适配器类直接委派即可
    @Override
    public void sampleOperation1() {
        this.adaptee.sampleOperation1();
    }

    @Override
    public void sampleOperation2() {

    }
}

```

### 1.4模式优缺点

- 适配器模式的主要优点是将目标类和适配者类解耦，增加了类的透明性和复用性，同时系统的灵活性和扩展性都非常好，更换适配器或者增加新的适配器都非常方便，符合“开闭原则”；
- 类适配器模式的缺点是适配器类在很多编程语言中不能同时适配多个适配者类，对象适配器模式的缺点是很难置换适配者类的方法。

适配器模式适用情况包括：系统需要使用现有的类，而这些类的接口不符合系统的需要；想要建立一个可以重复使用的类，用于与一些彼此之间没有太大关联的一些类一起工作。

## 2.外观模式（Facade Pattern）

### 2.1模式定义

外观模式（Facade Pattern）：外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称门面模式，它是一种对象结构模式。

### 2.2模式结构

外观模式包含外观（Facade）和子系统（SubSystem）两个角色。如下图所示，为外观模式的类图：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/18/1563449640757-1563449640947.png)

</div>

外观角色是在客户端直接调用的角色，在外观角色中可以知道相关子系统的功能和责任，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理；在软件系统中可以同时有一个或多个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能。下图是外观模式工作的时序图：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/18/1563451232132-1563451232138.png)

</div>

### 2.3代码示例

以观看电影为例，看电影需要有很多操作，使用外观模式一键实现看电影的功能：

子系统类：

```
public class SubSystem {
    public void turnOnTV() {
        System.out.println("turnOnTV()");
    }
    public void setCD(String cd) {
        System.out.println("setCD( " + cd + " )");
    }
    public void startWatching(){
        System.out.println("startWatching()");
    }
}

```

外观类：

```
public class Facade {
    private SubSystem subSystem = new SubSystem();
    public void watchMovie(){
        subSystem.turnOnTV();
        subSystem.setCD("Games of Thrones");
        subSystem.startWatching();
    }
}
```

客户端类：

```
public class Client {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.watchMovie();
    }
}

```

### 2.4模式优缺点

- 外观模式主要优点在于对客户屏蔽子系统组件，减少了客户处理的对象数目并使得子系统使用起来更加容易，它实现了子系统与客户之间的松耦合关系，并降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程；

- 其缺点在于不能很好地限制客户使用子系统类，而且在不引入抽象外观类的情况下，增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。

- 外观模式适用情况包括：要为一个复杂子系统提供一个简单接口；客户程序与多个子系统之间存在很大的依赖性；在层次化结构中，需要定义系统中每一层的入口，使得层与层之间不直接产生联系。

## 3.代理模式(Proxy Pattern)

### 3.1模式定义

代理模式：给某一个对象提供一个代理，并由代理对象