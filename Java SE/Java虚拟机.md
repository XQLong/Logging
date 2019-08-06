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

<div align="center">

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/06/1565061363292-1565061363296.png)

</div>