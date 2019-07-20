# 行为型模式（Behavioral Pattern）

行为型模式，是对在不同的对象之间划分责任和算法的抽象化。

行为型模式不仅仅关注类和对象的结构，而且重点关注它们之间的相互作用。通过行为型模式，可以更加清晰地划分类与对象的职责，并研究系统在运行时实例对象之间的交互。在系统运行时，对象并不是孤立的，它们可以通过相互通信与协作完成某些复杂功能，一个对象在运行时也将影响到其他对象的运行。

行为型模式分为类行为型模式和对象行为型模式两种：

- 类行为模式：类的行为型模式使用继承关系在几个类之间分配行为，类行为型模式主要通过多态等方式来分配父类与子类的职责。
- 对象行为模式：对象的行为型模式则使用对象的聚合关联关系来分配行为，对象行为型模式主要是通过对象关联等方式来分配两个或多个类的职责。根据“合成复用原则”，系统中要尽量使用关联关系来取代继承关系，因此大部分行为型设计模式都属于对象行为型设计模式。

## 1、命令模式（Command Pattern）

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
- 命令模式的适用场景主要包括：需要将请求调用者和请求接收者解耦，使得调用者和接收者不直接交互；需要在不同的时间指定请求、将请求排队和执行请求；需要支持命令的撤销操作和恢复操作，


