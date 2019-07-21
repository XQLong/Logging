# 行为型模式（Behavioral Pattern）

行为型模式，是对在不同的对象之间划分责任和算法的抽象化。

行为型模式不仅仅关注类和对象的结构，而且重点关注它们之间的相互作用。通过行为型模式，可以更加清晰地划分类与对象的职责，并研究系统在运行时实例对象之间的交互。在系统运行时，对象并不是孤立的，它们可以通过相互通信与协作完成某些复杂功能，一个对象在运行时也将影响到其他对象的运行。

行为型模式分为类行为型模式和对象行为型模式两种：

- 类行为模式：类的行为型模式使用继承关系在几个类之间分配行为，类行为型模式主要通过多态等方式来分配父类与子类的职责。
- 对象行为模式：对象的行为型模式则使用对象的聚合关联关系来分配行为，对象行为型模式主要是通过对象关联等方式来分配两个或多个类的职责。根据“合成复用原则”，系统中要尽量使用关联关系来取代继承关系，因此大部分行为型设计模式都属于对象行为型设计模式。

## 1.命令模式（Command Pattern）

### 1.1 模式定义

命令模式：将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为模式，其别名为动作（Action）模式或事物（Transaction）模式。

### 1.2 模式结构

命令模式中主要包含抽象命令类（Command）、具体命令类（ConcreteCommand）、调用者（Invoker）、接收者（Receiver）和客户类（Client）。

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/20/1563591705570-1563591705574.png)

</div>

命令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开。命令模式的时序图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/20/1563592173245-1563592173248.png)

</div>

### 1.3 代码示例

以下以一个设计遥控器来控制电灯开关为例：

抽象命令类：

```
public interface Command {
    void execute();
}
```

具体命令类：
```
public class LightOnCommand implements Command{
    Light light;
    public LightOnCommand(Light light){
        this.light = light;
    }
    @Override
    public void execute() {
        light.on();
    }
}
public class LightOffCommand implements Command{
    Light light;
    public LightOffCommand(Light light){
        this.light = light;
    }
    @Override
    public void execute() {
        light.off();
    }
}
```

命令接收者类：

```
public class Light {
    public void on(){
        System.out.println("Light is on");
    }
    public void off(){
        System.out.println("Light is off");
    }
}
```

命令模式调用者：

```
public class Invoker {
    private Command[] onCommands;
    private Command[] offCommands;
    private final int slotNum = 7;
    public Invoker() {
        this.onCommands = new Command[slotNum];
        this.offCommands = new Command[slotNum];
    }
    public void setOnCommand(Command command, int slot) {
        onCommands[slot] = command;
    }
    public void setOffCommand(Command command, int slot) {
        offCommands[slot] = command;
    }
    public void onButtonWasPushed(int slot) {
        onCommands[slot].execute();
    }
    public void offButtonWasPushed(int slot) {
        offCommands[slot].execute();
    }
}
```

命令模式客户端：

```
public class Client {
    public static void main(String[] args) {
        Invoker invoker = new Invoker();
        Light light = new Light();
        Command lightOnCommand = new LightOnCommand(light);
        Command lightOffCommand = new LightOffCommand(light);
        invoker.setOnCommand(lightOnCommand, 0);
        invoker.setOffCommand(lightOffCommand, 0);
        invoker.onButtonWasPushed(0);
        invoker.offButtonWasPushed(0);
    }
}
```

### 1.4模式优缺点

- 命令模式的主要优点在于降低系统的耦合度，增加新的命令很方便，而且可以比较容易地设计一个命令队列和宏命令，并方便地实现对请求的撤销和恢复；
- 命令模式的主要缺点在于可能导致某些系统有过多的具体命令类。
- 命令模式的适用场景主要包括：需要将请求调用者和请求接收者解耦，使得调用者和接收者不直接交互；需要在不同的时间指定请求、将请求排队和执行请求；需要支持命令的撤销操作和恢复操作，需要将一组操作组合在一起，即支持宏命令。

## 2.观察者模式（Observer Pattern）

### 2.1 模式定义

观察者模式：定义对象之间的一种多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式又叫做发布-订阅模式、模式-视图模式、源-监听器模式或从属者模式。观察者模式是一种对象行为模式。

### 2.2 模式结构

观察者模式主要包括目标（Subject）、具体目标（ConcreteSubject）、观察者（Observer）、具体观察者（ConcreteObserver）四个角色，观察者模式的类图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/20/1563614639406-1563614639410.png)

</div>

观察者模式描述了如何建立对象与对象之间的依赖关系，如何构造满足这种需求的系统。这一模式中的关键对象是观察目标和观察者，一个目标可以有任意数目的与之相依赖的观察者，一旦目标状态发生改变，所有的观察者都将得到通知。作为这个通知的响应，每个观察者都将即使更新自己的状态，以与目标状态同步，这种交互也称为发布-订阅。目标是通知的发布者，它发出通知时并不知道需要知道谁是它的观察者，可以有任意数目的观察者订阅并接收通知。其时序图如下：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/20/1563615666452-1563615666456.png)

</div>

### 2.3代码示例

以如下场景为例：天气数据布告板会在天气信息发生改变时更新其内容，布告板有多个，并且在将来会继续增加。

目标类：

```
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObserver();
}
```

具体目标类：

```
public class WeatherData implements Subject{
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;
    public WeatherData() {
        observers = new ArrayList<>();
    }
    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObserver();
    }
    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }
    @Override
    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i >= 0) {
            observers.remove(i);
        }
    }
    @Override
    public void notifyObserver() {
        for (Observer o : observers) {
            o.update(temperature, humidity, pressure);
        }
    }
}
```

观察者类：

```
public interface Observer {
    void update(float temp, float humidity, float pressure);
}
```

具体观察者类：

```
public class CurrentConditionsDisplay implements Observer{
    public CurrentConditionsDisplay(Subject weatherData) {
        weatherData.registerObserver(this);
    }
    @Override
    public void update(float temp, float humidity, float pressure) {
        System.out.println("CurrentConditionsDisplay.update: " + temp + " " + humidity + " " + pressure);
    }
}
public class StatisticsDisplay implements Observer {
    public StatisticsDisplay(Subject weatherData) {
        weatherData.registerObserver(this);
    }
    @Override
    public void update(float temp, float humidity, float pressure) {
        System.out.println("StatisticsDisplay.update: " + temp + " " + humidity + " " + pressure);
    }
}
```
客户端类：

```
public class WeatherStation {
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();
        CurrentConditionsDisplay currentConditionsDisplay = new CurrentConditionsDisplay(weatherData);
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);

        weatherData.setMeasurements(0, 0, 0);
        weatherData.setMeasurements(1, 1, 1);
    }
}
```

### 2.4模式优缺点

- 观察者模式的主要优点在于可以实现表示层和数据逻辑层的分离，并在观察目标和观察者之间建立一个抽象的耦合，支持广播通信；
- 其主要缺点在于如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间，而且如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
- 观察者模式适用情况包括：一个抽象模型有两个方面，其中一个方面依赖于另一个方面；一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变；一个对象必须通知其他对象，而并不知道这些对象是谁；需要在系统中创建一个触发链。


## 3.策略模式（Strategy Pattern）

### 3.1模式定义

策略模式：定义一系列算法，将每一个算法封装起来，并让他们可以互相替换。策略模式让算法独立于使用它的客户端而变化，也称为政策模式（Policy）。

### 3.2模式结构

策略模式主要包含环境类（Context）、抽象策略类（Strategy）、具体策略类（ConcreteStrategy）三个角色，策略模式的类图如下图所示：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/21/1563708036760-1563708036769.png)

</div>

策略模式是一个比较好理解的设计模式，策略模式是对算法的封装，它把算法的责任和算法本身分割开，委派给不同的对象管理。策略模式通常把一系列的算法封装到一系列的策略类里面，作为一个抽象策略类的子类。策略模式的时序图如下：


<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/07/21/1563710012538-1563710012541.png)

</div>

在策略模式中，应当由客户端自己决定在什么情况下使用什么样的具体策略角色。

### 3.3代码示例

设计一个鸭子，它可以动态地改变叫声，这里的算法是鸭子的动态叫声行为。

抽象策略类：

```
public interface QuackBehavior {
    void quack();
}
```

具体策略类：

```
public class Quack implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("quack!");
    }
}
public class Squeak implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("squeak!");
    }
}
```

环境类：

```
public class Duck {
    private QuackBehavior quackBehavior;
    public void performQuack() {
        if (quackBehavior != null) {
            quackBehavior.quack();
        }
    }
    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }
}
```

客户端类：

```
public class Client {
    public static void main(String[] args) {
        Duck duck = new Duck();
        duck.setQuackBehavior(new Squeak());
        duck.performQuack();
        duck.setQuackBehavior(new Quack());
        duck.performQuack();
    }
}
```

###模式优缺点

- 策略模式主要优点在于对“开闭原则”的完美支持，在不修改原有系统的基础上可以更换算法或者增加新的算法，它很好地管理算法族，提高了代码的复用性，是一种替换继承，避免多重条件转移语句的实现方式；
- -其缺点在于客户端必须知道所有的策略类，并理解其区别，同时在一定程度上增加了系统中类的个数，可能会存在很多策略类。
 

