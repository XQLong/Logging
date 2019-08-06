对于Java中的两类容器Collection和Map：

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/06/1565056754385-1565056754391.png)

```
LinkedList 和 ArrayList 都是不同步的，线程不安全；
Vector 和 Stack 都是同步的，线程安全；
Set是线程不安全的；

Hashtable的方法是同步的，线程安全；
HashMap的方法不是同步的，线程不安全；
```
