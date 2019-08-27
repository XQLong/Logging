## 一、同步方法

### 1、同步代码块

使用synchronized引入同步监视器：
```
synchronized(obj){
...
//此处的代码就是同步代码块
}
```

### 2、同步方法

使用synchronized关键字来修饰某个方法，则该方法称为同步方法。

### 3、使用同步锁

通过显示定义同步锁对象来实现同步，如最常用的可重入锁（ReentranLock）。

## 二、Volatile关键字

## 三、Synchronized底层同步原理

### CAS指令

CAS（Compare-and-Swap），即比较并交换，是一种实现并发算法时常用到的技术，Java并发包中很多类都使用到了CAS技术。

