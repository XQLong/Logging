对于Java中的两类容器Collection和Map：

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/06/1565056754385-1565056754391.png)

- LinkedList 和 ArrayList 都是不同步的，线程不安全；
- Vector 和 Stack 都是同步的，线程安全；
- Set是线程不安全的；

- Hashtable的方法是同步的，线程安全；
- HashMap的方法不是同步的，线程不安全；

- LinkedBlockingQueue是一个基于节点链接的可选是否有界的阻塞队列，不允许null值。是一个线程安全的阻塞队列，实现了先进先出等特性。
- PriorityQueue是一个无界队列，不允许null值，入队和出队的时间复杂度是O（log(n)）。每次从队列中取出的是具有最高优先权的元素。
- ConcurrentLinkedQueue是一个基于链接节点的无界线程安全队列，该队列的元素遵循FIFO原则。