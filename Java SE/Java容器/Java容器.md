# Java容器

容器可以管理对象的生命周期、对象与对象之间的依赖关系，可以使用一个配置文件（通常是XML），在上面定义好对象的名称、如何产生、哪个对象产生之后必须设定为某个对象的属性等，在启动容器之后，所有对象可以直接取用，不用编写任何一行程序代码来产生对象，或是建立对象和对象之间的依赖关系。

- 执行Java的应用程序：

容器是Java所编写的程序，容器来自动管理对象关系，常用的如WebSphere、WebLogic、Tomcat等。

- Java内部的容器

主要包括Collection和Map两种，Collection存储着对象的集合，而Map存储着键值对（两个对象）映射表。

Collection：

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/08/03/1564804991136-1564804991152.png)

Map：

![title](https://raw.githubusercontent.com/XQLong/Logging/master/img/2019/08/03/1564805101849-1564805101849.png)

## Collection

存储着对象集合。

### 1、Set

- TreeSet:基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如HashSet。HashSet的查找效率为O(1),TreeSet则为O（LogN）。
-HashSet:基于Hash表实现，支持快速查找，但不支持有序性查找。并且失去了元素的插入顺序信息，也就是说使用Iterator遍历HashSet得到的结果是不确定的。
- LinkedHashSet：具有HashSet的查找效率，且内部使用双向链表维护元素的插入顺序。

### 2、List

- ArrayList：基于动态数组实现，支持随机访问；
- Vector：和ArrayList类似，但它是线程安全的；
- LinkedList：基于双向链表实现，只能顺序访问，但可以在链表中快速地插入和删除元素。不仅如此，LinkedList还可以作栈，队列和双向队列。

### 3、Queue

- LinkedList：可以用来实现双向队列；
- PriorityQueue：基于堆结构实现，可以用它来实现优先队列。

## Map

### 1、TreeMap
基于红黑树实现

### 2、HashMap
基于哈希表实现。HashMap无contains方法。

### 3、HashTable
- 和HashMap类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入HashTable并不会导致数据不一致。（它是遗留类，应该尽量避免使用它，可以使用ConcurrentHashMap来支持线程安全）。
- HashTable中Key和Value都不允许出现null值。在HashMap中，null可以作为键，且这样的键只有一个；可以有一个或多个键所对应的值为null。当get()方法返回null值时，既可以表示HashMap中没有该键，也可以表示该键对应的值为null。因此在HashMap中不能由get()方法来判断HashMap中是否存在某个键，二应该用containKey（）方法来判断。



