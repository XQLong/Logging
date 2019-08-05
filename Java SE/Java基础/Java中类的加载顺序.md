类的加载顺序：
1. 父类静态对象和静态代码块
2. 子类静态对象和静态代码块
3. 父类非静态对象和非静态代码块
4. 父类构造函数
5. 子类非静态对象和非静态代码块
6. 子类构造函数

例1：

```
public class Test
{
    //类的静态对象
    public static Test t1 = new Test();
    //类的非静态代码块
    {
        System.out.println("blockA");
    }
    //类的静态代码块
    static
    {
        System.out.println("blockB");
    }
    public static void main(String[] args)
    {
        Test t2 = new Test();
    }
}
```

程序输出：
blockA		//创建类的静态对象他t1时执行非静态代码块的输出
blockB		//创建
blockA	


首先区别<clinit>是类初始化，<init>是实例初始化
类装载准备阶段->类初始化clinit阶段
类装载准备阶段：变量赋系统要求的初始值（0或者null等），final变量会在此时赋用户定义的初始值
类初始化<clinit>过程：
- Step1 编译器收集t1的赋值动作，static语句块，main方法语句块
- Step2 执行收集好的语句块：
- Step2.1 执行t1 = new Test，发现clinit过程已经调用，于是可以开始触发init过程，执行成员变量的初始化（此处没有），以及非静态代码块，输出blockA，最后调用构造函数（此处没有）
- Step2.2 按照顺序，执行static语句块，输出blockB
Step2.3 按照顺序，执行main方法语句块，执行t2 = new Test，触发init方法，执行非静态代码块,输出blockA