oyo面试题
准备：首先准备两段自己认为比较拿的出手的两段代码，面试时，面试官有可能让你写一小段熟悉的代码，十行左右。
https://blog.csdn.net/u012817635/article/details/79917674
```
public static <T> Predicate<T> distinctByKey(Function<? super T, Object> keyExtractor) {
    Map<Object, Boolean> seen = new ConcurrentHashMap<>();
    return t -> seen.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;
  }
```
###1、HashMap和ConcurrentHashMap区别和实现
    线程安全
    ConcurrentHashMap 1.7分段锁，1.8CAS
###2、线程池种类
    5种
    FixedThreadPool，特点：固定池子中线程的个数。使用静态方法newFixedThreadPool()创建线程池的时候指定线程池个数
    CachedThreadPool（弹性缓存线程池），特点：用newCachedThreadPool()方法创建该线程池对象，创建之初里面一个线程都没有，当execute方法
    或submit方法向线程池提交任务时，会自动新建线程；如果线程池中有空余线程，则不会新建；这种线程池一般最多情况可以容纳几万个线程，里面的线
    程空余60s会被回收。
    SingleThreadPool（单线程线程池），特点：池中只有一个线程，如果扔5个任务进来，那么有4个任务将排队；作用是保证任务的顺序执行
    ScheduledThreadpool（定时器线程池）
    WorkStealingPool since1.8 获取当前可用的线程数量进行创建作为并行级别,使用ForkJoinPool
###3、Hash一致性算法
    https://www.cnblogs.com/lpfuture/p/5796398.html
    环，虚拟节点 
###4、如何保证分布式事务一致性
    CAP定理
    BASE理论：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性
    两阶段提交（2PC）
    补偿事务（TCC）
    本地消息表（异步确保）
    MQ 事务消息
    Sagas 事务模型
###5、分布式锁实现方式
    zk顺序节点
    redis zest锁
6、String转换int的方法
    
7、JVM存储模型，垃圾回收机制

###8、MySql的聚集索引和非聚集索引的问题以及存储方式
    聚集索引叶子节点中存的是数据
    非聚集索引叶子节点村的是聚集索引
###9、session和cookie区别
    一个存在服务器，一个存在本地 
10一个m行，n列的表格，左上角到右下角的所有路径
```
int sum(int m, int n)//递归
{
	if (m == 1 || n == 1)
		return 1;
	int total = sum(m - 1, n) + sum(m, n - 1);
	return total;
}
```
11.手写消费者生产者  ps:闹呢？不会！给我个电脑我还能写写

12.String转int是自己实现，不是用java 提供的实现

###13.HashMap源码中put方法做的事情
    在java8中put方法实际调用的是putVal方法
    putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict)
    通过hash值计算出的下标如果没有node，则new一个node放入元素
    如果有，则判断哈希值是否相同，相同则覆盖；
    否则判断该node是链表还是红黑树，并使用相对应的方式put值，
    若是链表，则找到next为null的位置，并set值，并检测判断长度是否达到了8，达到，则转换为红黑树
    最后自增modCount，并检测长度是否大于阈值，若大于，则重新散列
###14.HashMap为什么在多线程操作时会造成死循环
    准确来说是1.7的HashMap会在resize的时候造成死循环，在1.8中已经修复了这个问题 
    主要原因是1.7的HashMap在rehash的时候会倒置链表元素，1.8中使用head和tail保证了链表的顺序和rehash之前的一致
###15.公平锁和非公平锁的有什么差异
    ReentrantLock、ReadWriteLock默认都是非公平锁
    https://blog.csdn.net/zhilinboke/article/details/83104597
    1、若在释放锁的时候总是没有新的兔子来打扰，则非公平锁等于公平锁；
    2、若释放锁的时候，正好一个兔子来喝水，而此时位于队列头的兔子还没有被唤醒（因为线程上下文切换是需要不少开销的），此时后来的兔子则优先获
    得锁，成功打破公平，成为非公平锁；
###16.synchronized和lock的区别
    https://www.cnblogs.com/iyyy/p/7993788.html
17.StringBuffer和StringBuilder的区别

18.聚集索引和非聚集索引的区别

19.单向链表反转（可能让手写下）

20.字符串中查找只有一个的字符（可能让书写下）

21.随意输入字符串，写程序反着输出来

22.分布式锁怎么实现

###23.aio bio nio 区别
    异步阻塞aio
    同步阻塞bio
    同步非阻塞nio
24.画一个分布式锁架构模型图，分布式锁 redis实现的原理。

25.java原子操作类，常用的数据结构类型，Tree和BTree。

26.单例模式代码 hash数据结构实现

27.Concurrenthash分段锁实现

28.两个空瓶子可以兑换一瓶饮料，输入N个空瓶子，返回可以喝到的饮料数据（可能让手写）

29.查一个链表的倒数第三个元素(手写)

30.链表反转（手写）

31.查一个字符串的第一个非重复字母（手写）

32.查数组里边重复的第一个数字（手写）

33.随意写一个超过10行的代码

34.怎么造成oom

35.内存泄漏和垃圾回收

36.手写一个算法：线程通信交互的流程

###37.性别字段适不适合建索引
    不适合
###38.Tomcat的默认线程数
    minProcessors：最小空闲连接线程数，用于提高系统处理性能，默认值为10
     maxProcessors：最大连接线程数，即：并发处理的最大请求数，默认值为75
     acceptCount：允许的最大连接数，应大于等于maxProcessors，默认值为100
39.string转换int
40.java多线程锁，独占锁，重入锁。
41.二叉树的查找算法，红黑树原理左旋右旋等
42.程序的stackoverflow堆栈溢出及解决
43.hash表的底层结构
44.手写阻塞队列
45.http缓冲 http协议 缓存
46.空间复杂度与时间复杂度

50.数据库中，男女字段适合加索引么？mysql,oracle对此问题可能有区别
###51.二叉树时间复杂度，和hash表的时间复杂度区别
    二叉树O(logn)
    哈希表O(1) 
    各种数据结构的时间复杂度：
    https://blog.csdn.net/ted_cs/article/details/82881831
52.CAS是什么
53.java1.7和java1.8中hashmap有什么区别
54.b+树状索引
55.画一个b+tree和 b-tree的结构，他们的区别是什么
56.volatile原理
57.分布式锁redis的实现
