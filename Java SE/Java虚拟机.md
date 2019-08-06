## JVM整体架构

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/06/1565058630843-1565058630847.png)

</div>

ava虚拟机本质是就是一个程序，当它在命令行上启动的时候，就开始执行保存在某字节码文件中的指令。Java语言的可移植性正是建立在Java虚拟机的基础上。任何平台只要装有针对于该平台的Java虚拟机，字节码文件（.class）就可以在该平台上运行。这就是“一次编译，多次运行”。

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/06/1565059831048-1565059831224.png)

</div>

### 1、Java虚拟机栈

- 在数据结构中：限定在表尾进行插入或删除操作的线性表；
- 计算机系统中：栈也称栈内存，是一个动态内存区域，存储函数内部的局部变量，方法调用和函数函数参数值，是由系统自动分配的，一般速度较快。存储地址连续存在有限栈容量。

例如对于下面程序：

```
public class Test {
    public static final Integer CONSTANT = 666;
    public int compute(){
        int a = 1;
        int b = 2;
        int c = (a+b)*10;
        return  c;
    }
    public static void main(String[] args) {
        Test test = new Test();
        test.compute();
    }
}
```
程序开始运行main（）方法，java虚拟机底层在栈空间为线程单独开辟一块main线程单独的栈区域，然后在此区域存储main方法的局部变量，当调用到新方法时，会单独划分一块内存区域栈帧，来存储方法的局部变量等。如下图所示：

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/06/1565073728856-1565073728869.png)

</div>

栈帧：每一次函数的调用都会在调用栈（call stack）上维护一个独立的栈帧（stack frame）。栈帧存储了方法的局部变量表、操作数栈、动态连接和方法返回地址等信息。每一个方法从调用开始至执行完成的过程,都对应着一个栈帧在虚拟机栈里面从入栈到出栈的过程。



### 2、方法区

用于存放已被加载的类信息、常量、静态变量、即时编译器，编译后的代码等数据。方法区不需要连续的内存，并可以动态扩展。

### 3、本地方法栈

本地方法栈与Java虚拟机栈类似，它们的区别在于本地方法栈为本地方法服务。

### 4、程序计数器

记录正在执行的虚拟机字节码指令的地址（如果正在执行的是本地方法则为空）。

### 5、堆

所有对象都在这里分配内存，是垃圾收集的主要区域（“GC堆”）。堆不需要连续的内存，