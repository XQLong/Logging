# Java并发

## 线程概述

进程是处于运动过程中的程序，是系统进行资源分配和调度的一个独立单元。具有如下特征：

- 独立性
- 动态性
- 并发性：多个进程可在单个处理器上并发执行，多个进程间不互相影响。

线程也被称为轻量级进程，线程是进程的执行单元。线程的执行是抢占式的；一个线程可以创建和撤销另一个线程，同一个进程中多个线程可以并发执行。

## 线程的创建和启动

### 通过继承Thread类

- 定义Thread类的子类，并重写该类的run()方法，该run()方法的方法体就代表了线程需要完成的任务。因此把run()方法称为线程的执行体；
- 创建子类的实例，即创建了线程对象；
- 调用线程对象的start()方法来启动该线程；

使用继承Thread类的方法来创建线程时，多个线程之间无法共享线程类的实例变量。

### 实现Runnable接口
- 定义Runnable接口的实现类，并重写该接口的run()方法，该run()方法同样是线程的执行体；
- 创建Runnable实现类的实例，并以此作为Thread的Target来创建Thread对象，该Thread对象才是真正的线程对象；
- 调用线程对象的start()方法来启动该线程。

采用Runnable接口的方式创建的多个线程可以共享线程类的实例变量（多个线程共享线程的target）。

### 使用Callable和Future
- 创建Callable接口的实现类，并实现Call()方法，该call()方法将作为线程执行体，且该caLL()方法有返回值，再创建Callable类的实例；
- 使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()返回值；
- 使用FutureTask对象作为Thread的target来创建并启动新线程；
- 调用FutureTask对象的get()方法来获得子线程执行结束后的返回值。

### 创建线程的三种方法的对比

实现Runnable和Callable接口的方式基本相同，知识Callable接口定义的方法有返回值，可以声明抛出异常。可以将实现Runnable接口和实现Callable接口归为一类方式。

实现Runnable、Callable接口方式创建多线程的优缺点：

- 线程类只实现了Runnable和Callable接口，还可以继承其他类；
- 在这种方式下，多个线程可以共享一个target对象，所以非常适合多个线程同时处理同一份资源的情况，从而可以将CPU、代码和数据分开，形成清晰的模型，较好地体现了面向对象的思想。

采用继承Thread类的方式创建多线程的优缺点：

- 劣势是因为线程类已经继承了Thread类而不能再继承其他类；
- 优势是编写简单，可直接使用this获取当前线程。

## 线程的生命周期



![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/14/1565785133309-1565785133663.png)






