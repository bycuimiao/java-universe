oyo面试题
以下面试题，是朋友去面试的一点经验，仅提供您参考，必要的时候，面试官可能会提供纸笔，写一下代码逻辑。不同的面试官风格不太一样，具体情况以现场
面试的情况为准。切忌下面的题不能泄漏，每一道题都是为了帮您通过面试，要认真准备，有点题不尽只要会思路，必要时手写也要能写出来才行。
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
    FixedThreadPool，特点：固定池子中线程的个数。使用静态方法newFixedThreadPool()创建线程池的时候指定线程池个数
    CachedThreadPool（弹性缓存线程池），特点：用newCachedThreadPool()方法创建该线程池对象，创建之初里面一个线程都没有，当execute方法
    或submit方法向线程池提交任务时，会自动新建线程；如果线程池中有空余线程，则不会新建；这种线程池一般最多情况可以容纳几万个线程，里面的线
    程空余60s会被回收。
    SingleThreadPool（单线程线程池），特点：池中只有一个线程，如果扔5个任务进来，那么有4个任务将排队；作用是保证任务的顺序执行
    ScheduledThreadpool（定时器线程池）
    WorkStealingPool
    ForkJoinPool
3、Hash一致性算法

4、如何保证分布式事务一致性

5、分布式锁实现方式

6、String转换int的方法

7、JVM存储模型，垃圾回收机制

8、MySql的聚集索引和非聚集索引的问题以及存储方式
9、session和cookie区别

10一个m行，n列的表格，左上角到右下角的所有路径

11.手写消费者生产者

12.String转int是自己实现，不是用java 提供的实现

13.HashMap源码中put方法做的事情

14.HashMap为什么在多线程操作时会造成死循环

15.公平锁和非公平锁的有什么差异

16.synchronized和lock的区别

17.StringBuffer和StringBuilder的区别

18.聚集索引和非聚集索引的区别

19.单向链表反转（可能让手写下）

20.字符串中查找只有一个的字符（可能让书写下）

21.随意输入字符串，写程序反着输出来

22.分布式锁怎么实现

23.aio bio nio 区别

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

37.性别字段适不适合建索引
38.Tomcat的默认线程数
39.string转换int
40.java多线程锁，独占锁，重入锁。
41.二叉树的查找算法，红黑树原理左旋右旋等
42.程序的stackoverflow堆栈溢出及解决
43.hash表的底层结构
44.手写阻塞队列
45.http缓冲 http协议 缓存
46.空间复杂度与时间复杂度