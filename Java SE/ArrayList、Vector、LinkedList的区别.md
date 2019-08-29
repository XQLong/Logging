- ArrayList与Vector底层都是数组, 访问快, 添加/删除慢


1. ArrayList与Vector底层都是数组
2. ArrayList与Vector默认初始化容量:10
3. ArrayList扩容: 1.5倍,   Vector扩容:2倍
4. Vector提供的方法都 使用了synchronized修饰 ,是线程安全的；而ArrayList是线程不安全的。