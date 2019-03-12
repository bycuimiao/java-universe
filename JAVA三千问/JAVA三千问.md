#JAVA三千问
    Ps:答案不一定准确，随时间和经验的增加，不断完善
###1、为什么频繁切换上下文会造成极大的开销？
    答：因为保存和恢复执行上下文，丢失局部性，使得CPU时间更多的花在线程调度而不是线程运行上。
###2、Servlet是线程安全的么？
    答：同一Servlet可能会同时被多个线程访问，所以Servlet需要是线程安全的。
###3、无状态的对象一定是线程安全的么？
    答：一定是。无状态的对象一定是线程安全的。保证一个对象线程安全最简单的办法就是保证该对象是无状态的。这种技术被称为线程封闭。
###4、volatile字段是否会被缓存在寄存器。
    答：volatile字段不会被缓存在寄存器或者其他处理器不可见的地方，因此读取volatile字段时总是获取到最新值。
###5、加锁机制可以保证可见性么？volatile变量呢？
    答：加锁机制既可以保证可见性，又可以保证原子性，而volatile变量只能保证可见性。
###6、redis是多线程还是单线程
    答：单线程。Redis是单进程单线程的网络模型，用的是epoll,poll,select网络模型，这些网络模型都是单线程处理网络请求
###7、redis支持的数据类型
    答：String、Hash、List、Set、zset
###7、redis的内部数据类型有哪些
    答：
    dict、sds、ziplist、quicklist、skiplist
    dict        http://zhangtielei.com/posts/blog-redis-dict.html
        dict也是一个基于哈希表的算法，但为了快速响应，他有两个hash表，在rehash的时候是增量式的rehash，每次只对一小部分key进行rehash。
    sds         http://zhangtielei.com/posts/blog-redis-sds.html
        redis使用sds（simple dynamic string）简单动态字符串来表示最基本的字符串数据，该结构记录了用于保存字符串的字节数组char buf[]、已使用长度int len和未使用长度int free。有点类似于java中的String对象。
        此sds利用c字符串作为字面量，并遵循以空字符'\0'作为字符串末尾的C风格，使得其可以直接重用C字符串函数库的部分函数，但相比较于C字符串有以下优点：
        直接保存字符串长度而不是像C那样需要遍历才能获取长度；
        通过空间预分配及惰性空间释放来减少由于修改字符串带来的内存重分配。空间预分配是指：当需要扩展字符数组容量时，如果分配后的长度将小于1MB，那么会预分配与当前len长度一样的字节量，如果超过1MB，则会分配1MB。惰性空间释放是指：当缩短sds字符串时，多余出来的字节数组并不回收，而是通过增长free记录起来，这样下次当需要增长到时候如果free本身就够用了，就不需要申请内存了。当然，也有API可调用来主动释放。
        使用二进制方式处理buf数组，保持二进制数据，因此可以保存除文本数据外的其他格式，如图片，音视频，压缩文件等；
        可动态扩展内存。sds表示的字符串其内容可以修改，也可以追加。在很多语言中字符串会分为mutable和immutable两种，显然sds属于mutable类型的。
        二进制安全（Binary Safe）。sds能存储任意二进制数据，而不仅仅是可打印字符。
        与传统的C语言字符串类型兼容。
    robj        http://zhangtielei.com/posts/blog-redis-robj.html
        为多种数据类型提供一种统一的表示方式。
        允许同一类型的数据采用不同的内部表示，从而在某些情况下尽量节省内存。
        支持对象共享和引用计数。当对象被共享的时候，只占用一份内存拷贝，进一步节省内存。
    ziplist     http://zhangtielei.com/posts/blog-redis-ziplist.html
    quicklist   http://zhangtielei.com/posts/blog-redis-quicklist.html
    skiplist    http://zhangtielei.com/posts/blog-redis-skiplist.html
    intset      http://zhangtielei.com/posts/blog-redis-intset.html
###8、Java线程池有哪些核心参数？都是什么含义？
	public ThreadPoolExecutor(int corePoolSize,
                                  int maximumPoolSize,
                                  long keepAliveTime,
                                  TimeUnit unit,
                                  BlockingQueue<Runnable> workQueue,
                                  ThreadFactory threadFactory,
                                  RejectedExecutionHandler handler)
	corePoolSize、maximumPoolSize、keepAliveTime、unit、workQueue、threadFactory、handler
	corePoolSize：核心线程数
		* 核心线程会一直存活，及时没有任务需要执行
		* 当线程数小于核心线程数时，即使有线程空闲，线程池也会优先创建新线程处理
		* 设置allowCoreThreadTimeout=true（默认false）时，核心线程会超时关闭
		在创建了线程池后，默认情况下，线程池中并没有任何线程，而是等待有任务到来才创建线程去执行任务，除非调用了prestartAllCoreThreads()
		或者prestartCoreThread()方法，从这2个方法的名字就可以看出，是预创建线程的意思，即在没有任务到来之前就创建corePoolSize个线程或者
		一个线程。默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到
		corePoolSize后，就会把到达的任务放到缓存队列当中
	maximumPoolSize：最大线程数
		* 当线程数>=corePoolSize，且任务队列已满时。线程池会创建新线程来处理任务
		* 当线程数=maxPoolSize，且任务队列已满时，线程池会拒绝处理任务而抛出异常	
	keepAliveTime：线程空闲时间
		* 当线程空闲时间达到keepAliveTime时，线程会退出，直到线程数量=corePoolSize
		* 如果allowCoreThreadTimeout=true，则会直到线程数量=0
	unit：TimeUnit枚举类型的值，代表keepAliveTime时间单位
	allowCoreThreadTimeout(这个参数不是在构造方法中设置，可以单独通过allowCoreThreadTimeOut(boolean value)设置)：允许核心线程超时
	workQueue：阻塞队列，用来存储等待执行的任务，决定了线程池的排队策略，有以下几种实现
		ArrayBlockingQueue;
	　　LinkedBlockingQueue;
	　　SynchronousQueue;
	threadFactory：线程工厂，用来创建线程
		创建线程时使用该工厂方法
	handler：任务拒绝处理器
		* 两种情况会拒绝处理任务：
			- 当线程数已经达到maxPoolSize，切队列已满，会拒绝新任务
			- 当线程池被调用shutdown()后，会等待线程池里的任务执行完毕，再shutdown。如果在调用shutdown()和线程池真正shutdown之间提交任务，会拒绝新任务
		* 线程池会调用rejectedExecutionHandler来处理这个任务。如果没有设置默认是AbortPolicy，会抛出异常
		* ThreadPoolExecutor类有几个内部实现类来处理这类情况：
			- AbortPolicy 丢弃任务，抛运行时异常
			- CallerRunsPolicy 执行任务
			- DiscardPolicy 忽视，什么都不会发生
			- DiscardOldestPolicy 从队列中踢出最先进入队列（最后一个执行）的任务
		* 实现RejectedExecutionHandler接口，可自定义处理器
	除了这些构建线程池需要的参数外，还有一些比较重要的成员变量
	private int largestPoolSize;   //用来记录线程池中曾经出现过的最大线程数
	private long completedTaskCount;   //用来记录已经执行完毕的任务个数
	private volatile int   poolSize;       //线程池中当前的线程数 ps:这个成员变量在1.8中已经移除，但依然可以通过getPoolSize()方法获取当前线程池线程数
###9、常用的线程池有哪些？
	newCachedThreadPool
    newFixedThreadPool 
    newScheduledThreadPool 
    newSingleThreadExecutor 
###10、事务的基本要素是什么？
	ACID
	原子性（Atomicity）：
		事务开始后所有操作，要么全部做完，要么全部不做，不可能停滞在中间环节。事务执行过程中出错，会回滚到事务开始前的状态，
		所有的操作就像没有发生一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位。
    一致性（Consistency）：
    	事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到。
    隔离性（Isolation）：
    	同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。
    持久性（Durability）：
    	事务完成后，事务对数据库的所有更新将被保存到数据库，不能回滚。
###11、mysql有哪些事务隔离级别
	http://www.cnblogs.com/huanongying/p/7021555.html
	脏读：
		事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据
	不可重复读：
		事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果 不一致。
	幻读：
		系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，
		当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读。
	小结：
		不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读侧重于新增或删除。解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表
	事务隔离级别					脏读		不可重复读	幻读
    读未提交（read-uncommitted）	是		是			是
    不可重复读（read-committed）	否		是			是
    可重复读（repeatable-read）	否		否			是  标准的rr是无法解决幻读的，但Innodb使用gap(间隙锁)解决了这个问题。next-key locks
    串行化（serializable）		否		否			否
    mysql默认的事务隔离级别为repeatable-read
###12、JVM的内部体系结构分为几部分
	三部分：类装载器（ClassLoader）子系统，运行时数据区，和执行引擎。
###13、Zookeeper有几种znode类型
	PERSISTENT - 持久化目录节点 
    	客户端与zookeeper断开连接后，该节点依旧存在 
    PERSISTENT_SEQUENTIAL - 持久化顺序编号目录节点 
    	客户端与zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号 
    EPHEMERAL - 临时目录节点 
    	客户端与zookeeper断开连接后，该节点被删除 
    EPHEMERAL_SEQUENTIAL - 临时顺序编号目录节点 
    	客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号 
###14、Zookeeper可以做什么？
	1.命名服务   2.配置管理   3.集群管理   4.分布式锁  5.队列管理
	https://www.cnblogs.com/felixzh/p/5869212.html
###15、zk是基于CAP的哪两个原则设计的？
	答：CP原则
###15、什么是MVCC  
	ps：mysql经典好文 	https://blog.csdn.net/u012817635/article/details/80585901
						https://www.cnblogs.com/likui360/p/9632641.html
	MVCC，Multi-Version Concurrency Control，多版本并发控制。MVCC 是一种并发控制的方法，一般在数据库管理系统中，实现对数据库的并发访问；
	在编程语言中实现事务内存。
	如果有人从数据库中读数据的同时，有另外的人写入数据，有可能读数据的人会看到『半写』或者不一致的数据。有很多种方法来解决这个问题，叫做并发
	控制方法。最简单的方法，通过加锁，让所有的读者等待写者工作完成，但是这样效率会很差。MVCC 使用了一种不同的手段，每个连接到数据库的读者，
	在某个瞬间看到的是数据库的一个快照，写者写操作造成的变化在写操作完成之前（或者数据库事务提交之前）对于其他的读者来说是不可见的。
	(注：与MVCC相对的，是基于锁的并发控制，Lock-Based Concurrency Control)
	MVCC最大的好处，相信也是耳熟能详：读不加锁，读写不冲突。
###16、mysql默认搜索引擎，RR(可重复读)事务隔离级别会不会出现幻读？
	不会。innodb使用gap(间隙锁)机制next-key locks解决了幻读问题。
###17、mysql的默认事务隔离级别是什么？
	mysql的事务隔离级别是可重复读
###18、innodb默认锁是什么？
	是Next-Key Locks，就是Record lock和gap lock的结合，即除了锁住记录本身，还要再锁住索引之间的间隙。
	Record lock
		单条索引记录上加锁，record lock锁住的永远是索引，而非记录本身，即使该表上没有任何索引，那么innodb会在后台创建一个隐藏的聚集主键
	索引，那么锁住的就是这个隐藏的聚集主键索引。所以说当一条sql没有走任何索引时，那么将会在每一条聚集索引后面加X锁，这个类似于表锁，但原理
	上和表锁应该是完全不同的。
    Gap lock
    	在索引记录之间的间隙中加锁，或者是在某一条索引记录之前或者之后加锁，并不包括该索引记录本身。gap lock的机制主要是解决可重复读模式下
    的幻读问题
###19、注意！mvcc是读取的，gap是你插入的时候，有可能只有record lock，有可能是record lock + gap
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
###15、
	
