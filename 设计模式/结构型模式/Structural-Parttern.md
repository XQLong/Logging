# 结构型模式（Structural Pattern）

结构型模式描述如何将类或者对象结合在一起形成更大的结构，就像搭积木，可以通过简单积木的组合形成复杂的、功能更为强大的结构。结构型模式可以分为类结构型模式和对象结构型模式：

- 类结构型模式关心类的组合，由多个类可以组合成一个更大的系统，在类结构型模式中一般只存在继承关系和实现关系。
- 对象结构型模式关心类与对象的组合，通过关联关系使得在一个类中定义另一个类的实例对象，然后通过该对象调用其方法。 根据“合成复用原则”，在系统中尽量使用关联关系来替代继 承关系，因此大部分结构型模式都是对象结构型模式。

## 1.适配器模式

### 1.1模式定义(Adapter Pattern)

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




