## 一、进程同步的常见方法

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

## 二、volatile关键字

volatile关键字可以说是Java虚拟机提供的最轻量级的同步机制。

### 1、缓存一致性

由于计算机的存储设备与处理器的运算速度有几个数量级的差距，所以现代计算机系统都不得不加入一层读写速度尽可能接近处理器运算速度的高速缓存（Cache）来作为内存与处理器之间的缓冲：将运算需要使用到的数据复制到缓存中，让运算能快速进行，当运算结束后再从缓存同步回内存之中，这样处理器就无须等待缓慢的内存读写了。

基于高速缓存的存储交互很好地解决了处理器与内存的速度矛盾，但是也为计算机系统带来更高的复杂度，因为它引入了一个新的问题：缓存一致性（Cache Coherence）。在多处理器系统中，每个处理器都有自己的高速缓存，而它们又共享同一主内存（Main Memory），当多个处理器的运算任务都涉及同一块主内存区域时，将可能导致各自的缓存数据不一致。

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/27/1566908560553-1566908560818.png)

不同架构的物理机器可以拥有不一样的内存模型，而Java虚拟机也有自己的内存模型，并且这里介绍的内存访问操作与硬件的缓存访问操作具有很高的可比性。

### 2、指令重排

除了增加高速缓存之外，为了使得处理器内部的运算单元能尽量被充分利用，处理器可能会对输入代码进行乱序执行（Out-Of-Order Execution）优化，处理器会在计算之后将乱序执行的结果重组，保证该结果与顺序执行的结果是一致的，但并不保证程序中各个语句计算的先后顺序与输入代码中的顺序一致，因此，如果存在一个计算任务依赖另外一个计算任务的中间结果，那么其顺序性并不能靠代码的先后顺序来保证。与处理器的乱序执行优化类似，Java虚拟机的即时编译器中也有类似的指令重排序（Instruction Reorder）优化。

### 3、Java内存模型

Java虚拟机规范中试图定义一种Java内存模型来屏蔽掉各种硬件和操作系统的内存访问差异，以实现让Java程序在各种平台下都能达到一致的内存访问效果。在此之前，主流程序语言（如C/C++等）直接使用物理硬件和操作系统的内存模型。

Java内存模型规定了所有的变量都存储在主内存（Main Memory）中。每条线程还有自己的工作内存（Working Memory，可与前面讲的处理器高速缓存类比），线程的工作内存中保存了被该线程使用到的变量的主内存副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量。不同的线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成。

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/27/1566909369919-1566909369923.png)

主内存直接对应于物理硬件的内存，而为了获取更好的运行速度，虚拟机（甚至是硬件系统本身的优化措施）可能会让工作内存优先存储于寄存器和高速缓存中，因为程序运行时主要访问读写的是工作内存。

### 4、原子性、可见性和有序性

- 原子性（Atomicity）：一个操作或者多个操作，要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行；
- 可见性（Visiblity）：可见性是指当一个线程修改了共享变量的值，其他线程能够立即得知这个修改；
- 有序性（Ordering）：一个线程中所有操作必须按照程序的顺序来执行。

### 5、先行发生（Happens-Before）原则

先行发生原则是判断数据是否存在竞争、线程是否安全的主要依据，依靠这个原则，我们可以通过几条规则解决并发环境下两个操作之间是否可能存在冲突的所有问题。

先行发生是Java内存模型中定义的两项操作之间的偏序关系，如果说操作A先行发生于操作B，其实就是说在发生操作B之前，操作A产生的影响能被操作B观察到，“影响”包括修改了内存中共享变量的值、发送了消息、调用了方法等。

Java内存模型下存在一些“天然的”先行发生关系，这些先行发生关系无须任何同步器协助就已经存在，可以在编码中直接使用。如果两个操作之间的关系不在此列，并且无法从下列规则推导出来的话，它们就没有顺序性保障，虚拟机可以对它们随意地进行重排序。

### 6、volatile关键字特性

Java内存模型对volatile专门定义了一些特殊的访问规则，当一个变量定义为volatile之后，它将具备两种特性。

- 保证此变量对所有线程的可见性，即当一条线程修改了这个变量的值，新值对于其他线程来说是可以立即得知的。
- 禁止指令重排序优化。普通的变量仅仅会保证在该方法的执行过程中所有依赖赋值结果的地方都能获取到正确的结果，而不能保证变量赋值操作的顺序与程序代码中的执行顺序一致。

需要注意的是，volatile变量在各个线程的工作内存中不存在一致性问题，但是Java里面的运算并非原子操作，并且volatile并不能保证原子性，导致volatile变量的运算在并发下一样是不安全的。

### 7、volatile的使用限制

由于volatile变量只能保证可见性，在不符合以下两条规则的运算场景中，我们仍然要通过加锁来保证原子性。
- 运算结果并不依赖变量的当前值，或者能够确保只有单一的线程修改变量的值。
- 变量不需要与其他的状态变量共同参与不变约束。

### 8、volatile的使用场景

- 状态标记
使用volatile来修饰状态标记量，使得状态标记量对所有线程是实时可见的，从而保证所有线程都能实时获取到最新的状态标记量，进一步决定是否进行操作。

- 双重检测机制实现单例
普通的双重检测机制在极端情况，由于指令重排序会出现问题，通过使用volatile来修饰instance，禁止指令重排序，从而可以正确的实现单例。

### 9、volatile的原理和实现机制

观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令。lock前缀指令实际上相当于一个内存屏障（也称内存栅栏），内存屏障会提供3个功能：
- 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已全部完成；
- 它会强制将对缓存的修改操作立即写入主存；
- 如果是写操作，它会导致其他CPU中对应的缓存无效。

## 三、CAS指令

CAS（Compare-and-Swap），即比较并交换，是一种实现并发算法时常用到的技术，Java并发包中很多类都使用到了CAS技术。

```
public class VolatileTest {
    public static volatile int race = 0;
    private static final int THREADS_COUNT = 20;
    private static CountDownLatch countDownLatch = new CountDownLatch(THREADS_COUNT);
    public static void increase() {
        race++;
    }
    public static void main(String[] args) throws InterruptedException {
        Thread[] threads = new Thread[THREADS_COUNT];
        for (int i = 0; i < THREADS_COUNT; i++) {
            threads[i] = new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int i = 0; i < 10000; i++) {
                        increase();
                    }
                    countDownLatch.countDown();
                }
            });
            threads[i].start();
        }
        countDownLatch.await();
        System.out.println(race);
    }
}
```

根据以上volatile关键字的学习我们知道，以上代码并不能按照我们所期望的输出结果200000，这是因为volatile关键字只能保证可见性，无法保证原子性，而自增操作并不是一个原子操作，在并发的情况下指令可能把较小的race值同步回内存中，导致我们每次都无法获得想要的结果。可以考虑以下解决方法：

- 使用synchronized来修饰increase方法
使用synchronized修饰后，increase方法变成了一个原子操作，因此是肯定能得到正确的结果。但是，每次自增都进行加锁，性能可能会稍微差了点。

- 使用Java并发包原子操作类
将race改为使用使用AtomicInteger定义，“race++”改成使用“race.getAndIncrement()”，AtomicInteger.getAndIncrement()是原子操作，因此我们可以确保每次都可以获得正确的结果，并且在性能上有不错的提升。

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/27/1566914881126-1566914881133.png)

通过阅读方法源码我们可以看到，getAndIncrement方法调用getAndAddInt方法，

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/27/1566915219728-1566915219734.png)

最后调用的是compareAndSwapInt方法，即CAS。

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/27/1566915241171-1566915241175.png)

getAndAddInt方法解析：拿到内存位置的最新值v，使用CAS尝试修将内存位置的值修改为目标值v+delta，如果修改失败，则获取该内存位置的新值v，然后继续尝试，直至修改成功。

CAS是英文单词CompareAndSwap的缩写，中文意思是：比较并替换。CAS需要有3个操作数：内存地址V，旧的预期值A，即将要更新的目标值B。CAS指令执行时，当且仅当内存地址V的值与预期值A相等时，将内存地址V的值修改为B，否则就什么都不做。整个比较并替换的操作是一个原子操作。

CAS(Compare and Swap)，用于在硬件层面上提供原子性操作。在 Intel 处理器中，比较并交换通过指令cmpxchg实现。比较是否和给定的数值一致，如果一致则修改，不一致则不修改。

