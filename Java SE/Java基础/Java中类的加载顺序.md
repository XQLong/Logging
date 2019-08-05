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
