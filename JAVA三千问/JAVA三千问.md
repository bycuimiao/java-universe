#JAVA三千问
    Ps:答案不一定准确，随时间和经验的增加，不断完善
###1、为什么频繁切换上下文会造成极大的开销？
    答：因为保存和恢复执行上下文，丢失局部性，使得CPU时间更多的花在线程调度而不是线程运行上。
    操作系统实现线程之间的切换时需要从用户态转换到核心态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高
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
###20、单点登录是如何实现的？原理是什么？
###21、缓存击穿应对方案有哪些？
	最靠谱的就是布隆过滤器。guava有布隆过滤器的实现
	或者互斥锁排队，就是在redis读库的时候加锁，保证没有大量请求同时访问数据库	
###22、布隆过滤器原理  非常惊艳的算法
	https://www.jianshu.com/p/2104d11ee0a2
	本质上布隆过滤器是一种数据结构，比较巧妙的概率型数据结构（probabilistic data structure），特点是高效地插入和查询，可以用来告诉你 
	“某样东西一定不存在或者可能存在”。
    相比于传统的 List、Set、Map 等数据结构，它更高效、占用空间更少，但是缺点是其返回的结果是概率性的，而不是确切的。
    BitMap
    默认误判率3%
    应用
    黑名单
    	比如邮件黑名单过滤器，判断邮件地址是否在黑名单中
    排序(仅限于BitSet)
    	仔细想想，其实BitSet在set(int value)的时候，“顺便”把value也给排序了。
    网络爬虫
    	判断某个URL是否已经被爬取过
    K-V系统快速判断某个key是否存在
    	典型的例子有Hbase，Hbase的每个Region中都包含一个BloomFilter，用于在查询时快速判断某个key在该region中是否存在，如果不存在，直接返回，节省掉后续的查询。
###23、为什么HashMap的长度的2的N次幂
	http://www.importnew.com/16301.html
	因为&运算的效率高于%运算，保持N次幂是11111111
###24、什么是Bit-map？
###25、Arrays.sort(arr) 是使用的什么排序算法？（JDK8）
	根据不同类型的数据，有不同的重载实现。String[]类型是长度在7以内，使用插入排序，大于7个使用快排。对于int[]是在286长度以下用双轴快排。
###26、synchronized的实现原理是什么？
	https://www.cnblogs.com/paddix/p/5367116.html
	synchronized (this) {
	            System.out.println("Method 1 start");
	}
	同步块的情况，底层语义是montorenter和monitorenter
	public synchronized void method() {
             System.out.println("Hello World!");
    }
    方法层面的synchronized是常量池中多了ACC_SYNCHRONIZED标示符，JVM就是根据该标示符来实现方法的同步的：当方法调用时，调用指令将会检查
    方法的 ACC_SYNCHRONIZED 访问标志是否被设置，如果设置了，执行线程将先获取monitor，获取成功之后才能执行方法体，方法执行完后再释放
    monitor。在方法执行期间，其他任何线程都无法再获得同一个monitor对象。 其实本质上没有区别，只是方法的同步是一种隐式的方式来实现，无需通
    过字节码来完成。
###27、什么是偏向锁？
	https://blog.csdn.net/javazejian/article/details/72828483 
	偏向锁是Java 6之后加入的新锁，它是一种针对加锁操作的优化手段，经过研究发现，在大多数情况下，锁不仅不存在多线程竞争，而且总是由同一线程
	多次获得，因此为了减少同一线程获取锁(会涉及到一些CAS操作,耗时)的代价而引入偏向锁。偏向锁的核心思想是，如果一个线程获得了锁，那么锁就进
	入偏向模式，此时Mark Word 的结构也变为偏向锁结构，当这个线程再次请求锁时，无需再做任何同步操作，即获取锁的过程，这样就省去了大量有关锁
	申请的操作，从而也就提供程序的性能。所以，对于没有锁竞争的场合，偏向锁有很好的优化效果，毕竟极有可能连续多次是同一个线程申请相同的锁。但
	是对于锁竞争比较激烈的场合，偏向锁就失效了，因为这样场合极有可能每次申请锁的线程都是不相同的，因此这种场合下不应该使用偏向锁，否则会得不
	偿失，需要注意的是，偏向锁失败后，并不会立即膨胀为重量级锁，而是先升级为轻量级锁。
###28、什么是轻量级锁
	倘若偏向锁失败，虚拟机并不会立即升级为重量级锁，它还会尝试使用一种称为轻量级锁的优化手段(1.6之后加入的)，此时Mark Word 的结构也变为轻量级锁的结构。轻量级锁能够提升程序性能的依据是“对绝大部分的锁，在整个同步周期内都不存在竞争”，注意这是经验数据。需要了解的是，轻量级锁所适应的场景是线程交替执行同步块的场合，如果存在同一时间访问同一锁的场合，就会导致轻量级锁膨胀为重量级锁。
###29、什么是自旋锁
	轻量级锁失败后，虚拟机为了避免线程真实地在操作系统层面挂起，还会进行一项称为自旋锁的优化手段。这是基于在大多数情况下，线程持有锁的时间都不会太长，如果直接挂起操作系统层面的线程可能会得不偿失，毕竟操作系统实现线程之间的切换时需要从用户态转换到核心态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，因此自旋锁会假设在不久将来，当前的线程可以获得锁，因此虚拟机会让当前想要获取锁的线程做几个空循环(这也是称为自旋的原因)，一般不会太久，可能是50个循环或100循环，在经过若干次循环后，如果得到锁，就顺利进入临界区。如果还不能获得锁，那就会将线程在操作系统层面挂起，这就是自旋锁的优化方式，这种方式确实也是可以提升效率的。最后没办法也就只能升级为重量级锁了。
###30、什么是消除锁
	消除锁是虚拟机另外一种锁的优化，这种优化更彻底，Java虚拟机在JIT编译时(可以简单理解为当某段代码即将第一次被执行时进行编译，又称即时编译)，通过对运行上下文的扫描，去除不可能存在共享资源竞争的锁，通过这种方式消除没有必要的锁，可以节省毫无意义的请求锁时间，如下StringBuffer的append是一个同步方法，但是在add方法中的StringBuffer属于一个局部变量，并且不会被其他线程所使用，因此StringBuffer不可能存在共享资源竞争的情景，JVM会自动将其锁消除。
###31、锁分类
	https://www.cnblogs.com/qifengshi/p/6831055.html
	公平锁/非公平锁
    可重入锁			可以在一定程度上避免死锁
    独享锁/共享锁
    互斥锁/读写锁
    乐观锁/悲观锁		原子类多是CAS乐观锁
    分段锁
    偏向锁/轻量级锁/重量级锁
    自旋锁
###32、Synchronized是公平锁么？
	Synchronized是非公平锁，可重入。且不能设置为公平锁。且是独享锁。
	ReentrantLock默认是公平锁，可通过构造方法指定为非公平锁。可重入。独享锁。
	ReadWriteLock，其读锁是共享锁，其写锁是独享锁
###33、分布式事务的两个理论是什么？
	CAP - 
		一致性(Consistency) ： 客户端知道一系列的操作都会同时发生(生效)
        可用性(Availability) ： 每个操作都必须以可预期的响应结束
        分区容错性(Partition tolerance) ： 即使出现单个组件无法可用,操作依然可以完成
	BASE - 
		Basically Available（基本可用）
        Soft state（软状态）
        Eventually consistent（最终一致性）
    BASE理论是对CAP中的一致性和可用性进行一个权衡的结果，理论的核心思想就是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用
    适当的方式来使系统达到最终一致性（Eventual consistency）。
###34、分布式事务解决方案？
	https://www.cnblogs.com/savorboard/p/distributed-system-transaction-consistency.html
	两阶段提交（2PC）
		在XA协议中包含着两个角色：事务协调者和事务参与者
		存在部分问题，1.性能问题2.协调者单点故障问题3.丢失消息导致的不一致问题。
		ps：三阶段提交增加了CanCommit阶段，并且引入了超时机制
	补偿事务（TCC）
		Try 阶段主要是对业务系统做检测及资源预留
        Confirm 阶段主要是对业务系统做确认提交，Try阶段执行成功并开始执行 Confirm阶段时，默认 Confirm阶段是不会出错的。即：只要Try成功，Confirm一定成功。
        Cancel 阶段主要是在业务执行错误，需要回滚的状态下执行的业务取消，预留资源释放。
        优点： 跟2PC比起来，实现以及流程相对简单了一些，但数据的一致性比2PC也要差一些
        缺点： 缺点还是比较明显的，在2,3步中都有可能失败。TCC属于应用层的一种补偿方式，所以需要程序员在实现的时候多写很多补偿的代码，在一
        些场景中，一些业务流程可能用TCC不太好定义及处理。
    本地消息表（异步确保）
    MQ 事务消息
###35、mysql事务问题
	而事务的 ACID 是通过 InnoDB 日志和锁来保证。事务的隔离性是通过数据库锁的机制实现的，持久性通过 Redo Log（重做日志）来实现，原子性和
	一致性通过 Undo Log 来实现。Undo Log 的原理很简单，为了满足事务的原子性，在操作任何数据之前，首先将数据备份到一个地方（这个存储数据
	备份的地方称为 Undo Log）。然后进行数据的修改。如果出现了错误或者用户执行了 Rollback 语句，系统可以利用 Undo Log 中的备份将数据恢
	复到事务开始之前的状态。 和 Undo Log 相反，Redo Log 记录的是新数据的备份。在事务提交前，只要将 Redo Log 持久化即可，不需要将数据持
	久化。当系统崩溃时，虽然数据没有持久化，但是 Redo Log 已经持久化。系统可以根据 Redo Log 的内容，将所有数据恢复到最新的状态。对具体实
	现过程有兴趣的同学可以去自行搜索扩展。
###36、线程池有什么作用?
	线程复用;控制最大并发数;管理线程
###37、springMvc请求流程？
	https://www.cnblogs.com/leskang/p/6101368.html
	https://www.jianshu.com/p/6f841d81ed72
	第一步：发起请求到前端控制器(DispatcherServlet)
    第二步：前端控制器请求HandlerMapping查找 Handler （可以根据xml配置、注解进行查找）
    第三步：处理器映射器HandlerMapping向前端控制器返回Handler，HandlerMapping会把请求映射为HandlerExecutionChain对象（包含一个
    	   Handler处理器（页面控制器）对象，多个HandlerInterceptor拦截器对象），通过这种策略模式，很容易添加新的映射策略
    第四步：前端控制器调用处理器适配器去执行Handler
    第五步：处理器适配器HandlerAdapter将会根据适配的结果去执行Handler
    第六步：Handler执行完成给适配器返回ModelAndView
    第七步：处理器适配器向前端控制器返回ModelAndView （ModelAndView是springmvc框架的一个底层对象，包括 Model和view）
    第八步：前端控制器请求视图解析器去进行视图解析 （根据逻辑视图名解析成真正的视图(jsp)），通过这种策略很容易更换其他视图技术，只需要更改
    	   视图解析器即可
    第九步：视图解析器向前端控制器返回View
    第十步：前端控制器进行视图渲染 （视图渲染将模型数据(在ModelAndView对象中)填充到request域）
    第十一步：前端控制器向用户响应结果
###38、一次完整的Http请求过程
	https://blog.csdn.net/zjkC050818/article/details/78345819
	域名解析 --> 发起TCP的3次握手 --> 建立TCP连接后发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，
	并请求html代码中的资源（如js、css、图片等） --> 浏览器对页面进行渲染呈现给用户
###39、JVM由那几部分组成
	类加载器(classloader) + 执行引擎(execution engine) + 运行时数据区域(runtime data area)
###40、运行时数据区域由哪几部分组成
	由线程共享的数据区：堆、方法区
	线程隔离的数据区：虚拟机栈、本地方法栈、程序计数器
###41、程序计数器的作用是什么？
	为了切换线程后能恢复到正确的执行位置，每条线程都需要一个独立的程序计数器。如果线程执行的是一个java方法，这个计数器记录的是正在执行的虚拟机
	字节码指令的地址；如果正在执行的Native方法，则计数器值为空。此内存区域是唯一一块没有规定OOM的区域。
###42、是所有的对象实例以及数组都要再堆上分配么？
	随着JIT编译器的发展与逃逸分析技术的成熟，栈上分配、标量替换优化技术将会导致一些微妙变化，并不是所有对象都绝对的在堆上分配了
###43、方法区存储的是什么？	
	存储的是已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据
###44、永久代和方法区的区别是什么
	方法区是JVM的规范，而永久代是这个规范的实现，这个实现在JDK8中被移除，取而代之的是元区域MetaSpace，使用本地内存即物理内存
###45、运行时常量池在JVM的哪个区域？
	JDK7之前在方法区，JDK8之后在堆中。
###46、有哪些算法可以判断一个对象是否可以被回收
	引用计数法、可达性分析
###47、如何判断一个类是否是"无用的类"，在方法区可被回收
	该类所有实例已被回收，堆中无该类的实例
	加载该类的ClassLoader已经被回收
	该类的Class对象没有被引用，也没有地方通过反射访问该类的方法
###48、有哪些垃圾回收算法？
	标记-清除算法
	复制算法
	标记-整理算法
	分代收集算法
###49、IP报文里面的TTL有什么用？
	TTL协议由源主机设置，每经过一个路由节点，TTL减1，当TTL为0时，数据包被丢弃，发送ICMP报文给主机，防止主机无休止的发送报文。
###50、什么是ICMP协议
	ICMP协议是检测网络是否通畅、主机是否可达、路由是否可以用等网络运行状态的协议。
###51、简述三次握手
	A机器 --> SYN=1 , seq=x 					--> B机器
	A机器 <-- SYN=1 , ACK=1 seq=y , ack=x+1 <-- B机器
	A机器 --> ack=1 , seq=x+1 , ack=y+1		--> B机器
	ps:seq为序列化，ack为确认序列化，SYN和ACK是TCP报文FLAG位标志，SYN用做建立连接时的同步信号；ACK用于对收到的数据进行确认。
###52、什么是SOA
	SOA（Service Oriented Architecture）“面向服务的架构”:他是一种设计方法，其中包含多个服务， 服务之间通过相互依赖最终提供一系列的功能。
	一个服务 通常以独立的形式存在与操作系统进程中。各个服务之间 通过网络调用。
###53、服务治理
	服务治理领域最重要的问题就是服务发现与注册
	dubbo就是一个带有服务治理功能的RPC框架，提供了一套较为完整的服务治理方案
###54、dubbo协议
	https://blog.csdn.net/fuyuwei2015/article/details/72848310
	Dubbo缺省的dubbo协议采用单一长连接和NIO异步通讯，适合于小数据量大并发的服务调用，以及服务消费者机器数远大于服务提供者机器数的情况。
	dubbo协议的序列化协议是Hessian二进制序列化
###55、HashMap
	http://www.importnew.com/28263.html
###56、HTTPS默认端口是？
	HTTPS的默认端口号是443
###57、HTTP无状态的特性是什么意思？
	HTTP协议是一个无状态的协议，同一个客户端的这次请求和上次请求是没有对应关系。为了解决这个问题， Web程序引入了Cookie机制来维护状态.
###58、HTTP状态码
	HTTP/1.1中定义了5类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别
    1XX  提示信息 - 表示请求已被成功接收，继续处理
    2XX  成功 - 表示请求已被成功接收，理解，接受
    3XX  重定向 - 要完成请求必须进行更进一步的处理
    4XX  客户端错误 -  请求有语法错误或请求无法实现
    5XX  服务器端错误 -   服务器未能实现合法的请求
    
    304 Not Modified    代表上次的文档已经被缓存了， 还可以继续使用，
    400 Bad Request  客户端请求与语法错误，不能被服务器所理解
    403 Forbidden 服务器收到请求，但是拒绝提供服务
    404 Not Found
    500 Internal Server Error 服务器发生了不可预期的错误
    503 Server Unavailable 服务器当前不能处理客户端的请求，一段时间后可能恢复正常
	---------------
	1**(信息类)：表示接收到请求并且继续处理
    100——客户必须继续发出请求
    101——客户要求服务器根据请求转换HTTP协议版本
    ************
    2**(响应成功)：表示动作被成功接收、理解和接受
    200——表明该请求被成功地完成，所请求的资源发送回客户端
    201——提示知道新文件的URL
    202——接受和处理、但处理未完成
    203——返回信息不确定或不完整
    204——请求收到，但返回信息为空
    205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
    206——服务器已经完成了部分用户的GET请求
    ************
    3**(重定向类)：为了完成指定的动作，必须接受进一步处理
    300——请求的资源可在多处得到
    301——本网页被永久性转移到另一个URL
    302——请求的网页被转移到一个新的地址，但客户访问仍继续通过原始URL地址，重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
    303——建议客户访问其他URL或访问方式
    304——自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用
    305——请求的资源必须从服务器指定的地址得到
    306——前一版本HTTP中使用的代码，现行版本中不再使用
    307——申明请求的资源临时性删除
    ************
    4**(客户端错误类)：请求包含错误语法或不能正确执行
    400——客户端请求有语法错误，不能被服务器所理解
    401——请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
    HTTP 401.1 - 未授权：登录失败
    　　HTTP 401.2 - 未授权：服务器配置问题导致登录失败
    　　HTTP 401.3 - ACL 禁止访问资源
    　　HTTP 401.4 - 未授权：授权被筛选器拒绝
    HTTP 401.5 - 未授权：ISAPI 或 CGI 授权失败
    402——保留有效ChargeTo头响应
    403——禁止访问，服务器收到请求，但是拒绝提供服务
    HTTP 403.1 禁止访问：禁止可执行访问
    　　HTTP 403.2 - 禁止访问：禁止读访问
    　　HTTP 403.3 - 禁止访问：禁止写访问
    　　HTTP 403.4 - 禁止访问：要求 SSL
    　　HTTP 403.5 - 禁止访问：要求 SSL 128
    　　HTTP 403.6 - 禁止访问：IP 地址被拒绝
    　　HTTP 403.7 - 禁止访问：要求客户证书
    　　HTTP 403.8 - 禁止访问：禁止站点访问
    　　HTTP 403.9 - 禁止访问：连接的用户过多
    　　HTTP 403.10 - 禁止访问：配置无效
    　　HTTP 403.11 - 禁止访问：密码更改
    　　HTTP 403.12 - 禁止访问：映射器拒绝访问
    　　HTTP 403.13 - 禁止访问：客户证书已被吊销
    　　HTTP 403.15 - 禁止访问：客户访问许可过多
    　　HTTP 403.16 - 禁止访问：客户证书不可信或者无效
    HTTP 403.17 - 禁止访问：客户证书已经到期或者尚未生效
    404——一个404错误表明可连接服务器，但服务器无法取得所请求的网页，请求资源不存在。eg：输入了错误的URL
    405——用户在Request-Line字段定义的方法不允许
    406——根据用户发送的Accept拖，请求资源不可访问
    407——类似401，用户必须首先在代理服务器上得到授权
    408——客户端没有在用户指定的饿时间内完成请求
    409——对当前资源状态，请求不能完成
    410——服务器上不再有此资源且无进一步的参考地址
    411——服务器拒绝用户定义的Content-Length属性请求
    412——一个或多个请求头字段在当前请求中错误
    413——请求的资源大于服务器允许的大小
    414——请求的资源URL长于服务器允许的长度
    415——请求资源不支持请求项目格式
    416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
    417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求长。
    ************
    5**(服务端错误类)：服务器不能正确执行一个正确的请求
    HTTP 500 - 服务器遇到错误，无法完成请求
    　　HTTP 500.100 - 内部服务器错误 - ASP 错误
    　　HTTP 500-11 服务器关闭
    　　HTTP 500-12 应用程序重新启动
    　　HTTP 500-13 - 服务器太忙
    　　HTTP 500-14 - 应用程序无效
    　　HTTP 500-15 - 不允许请求 global.asa
    　　Error 501 - 未实现
    HTTP 502 - 网关错误
    HTTP 503：由于超载或停机维护，服务器目前无法使用，一段时间后可能恢复正常
###59、HTTP Request header有哪些？
	https://www.cnblogs.com/rayray/p/3729533.html  ps:通篇干货
	Cache头域
		-----------
		If-Modified-Since
		作用： 把浏览器端缓存页面的最后修改时间发送到服务器去，服务器会把这个时间与服务器上实际文件的最后修改时间进行对比。如果时间一致，那么返
		回304，客户端就直接使用本地缓存文件。如果时间不一致，就会返回200和新的文件内容。客户端接到之后，会丢弃旧文件，把新文件缓存起来，并显示
		在浏览器中.
		例如：If-Modified-Since: Thu, 09 Feb 2012 09:07:57 GMT
		-----------
		If-None-Match
		作用: If-None-Match和ETag一起工作，工作原理是在HTTP Response中添加ETag信息。 当用户再次请求该资源时，将在HTTP Request 中加入
		If-None-Match信息(ETag的值)。如果服务器验证资源的ETag没有改变（该资源没有更新），将返回一个304状态告诉客户端使用本地缓存文件。否则
		将返回200状态和新的资源和Etag.  使用这样的机制将提高网站的性能
		例如: If-None-Match: "03f2b33c0bfcc1:0"
		-----------
		Pragma
		作用： 防止页面被缓存， 在HTTP/1.1版本中，它和Cache-Control:no-cache作用一模一样
		Pargma只有一个用法， 例如： Pragma: no-cache
		-----------
		Cache-Control
        作用: 这个是非常重要的规则。 这个用来指定Response-Request遵循的缓存机制。各个指令含义如下
        Cache-Control:Public   可以被任何缓存所缓存（）
        Cache-Control:Private     内容只缓存到私有缓存中
        Cache-Control:no-cache  所有内容都不会被缓存
    ---------------
	Client头域
		-----------
		Accept
        作用： 浏览器端可以接受的媒体类型,
        例如：  Accept: text/html  代表浏览器可以接受服务器回发的类型为 text/html  也就是我们常说的html文档,
        如果服务器无法返回text/html类型的数据,服务器应该返回一个406错误(non acceptable)
        通配符 * 代表任意类型
        例如  Accept: */*  代表浏览器可以处理所有类型,(一般浏览器发给服务器都是发这个)
        -----------
        Accept-Encoding：
        作用： 浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法（gzip，deflate），（注意：这不是只字符编码）;
        例如： Accept-Encoding: gzip, deflate
        -----------
        Accept-Language
        作用： 浏览器申明自己接收的语言。 
        语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等；
        例如： Accept-Language: en-us
        -----------
        User-Agent
        作用：告诉HTTP服务器， 客户端使用的操作系统和浏览器的名称和版本.
        我们上网登陆论坛的时候，往往会看到一些欢迎信息，其中列出了你的操作系统的名称和版本，你所使用的浏览器的名称和版本，这往往让很多人感到
        很神奇，实际上，服务器应用程序就是从User-Agent这个请求报头域中获取到这些信息User-Agent请求报头域允许客户端将它的操作系统、浏览器和
        其它属性告诉服务器。
        例如： User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; CIBA; .NET CLR 2.0.50727; .NET 
        CLR 3.0.4506.2152; .NET CLR 3.5.30729; .NET4.0C; InfoPath.2; .NET4.0E)
        -----------
        Accept-Charset
        作用：浏览器申明自己接收的字符集，这就是本文前面介绍的各种字符集和字符编码，如gb2312，utf-8（通常我们说Charset包括了相应的字符编码方案）；
    ---------------
    Cookie/Login 头域
    	Cookie:
        作用： 最重要的header, 将cookie的值发送给HTTP 服务器
    ---------------
    Entity头域
    	-----------
    	Content-Length
        作用：发送给HTTP服务器数据的长度。
        例如： Content-Length: 38
        -----------
        Content-Type
        作用：
        例如：Content-Type: application/x-www-form-urlencoded
    ---------------
    Miscellaneous 头域
    	Referer:
        作用： 提供了Request的上下文信息的服务器，告诉服务器我是从哪个链接过来的，比如从我主页上链接到一个朋友那里，他的服务器就能够从
        HTTP Referer中统计出每天有多少用户点击我主页上的链接访问他的网站。
        例如: Referer:http://translate.google.cn/?hl=zh-cn&tab=wT
    ---------------
    Transport 头域 
    	-----------
    	Connection
        例如：　Connection: keep-alive   当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问
        这个服务器上的网页，会继续使用这一条已经建立的连接
        例如：  Connection: close  代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭， 当客户端再次发送Request，
        需要重新建立TCP连接。
        -----------
        Host（发送请求时，该报头域是必需的）
        作用: 请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的
        例如: 我们在浏览器中输入：http://www.guet.edu.cn/index.html
        浏览器发送的请求消息中，就会包含Host请求报头域，如下：
        Host：http://www.guet.edu.cn
        此处使用缺省端口号80，若指定了端口号，则变成：Host：指定端口号
###60、HTTP Response header有哪些
	Cache头域
		-----------
		Date
        作用:  生成消息的具体时间和日期
        例如：　Date: Sat, 11 Feb 2012 11:35:14 GMT 
        -----------
    	Expires    
    	作用: 浏览器会在指定过期时间内使用本地缓存
        例如: Expires: Tue, 08 Feb 2022 11:35:14 GMT
        -----------
        Vary
        作用：
        例如: Vary: Accept-Encoding
    Cookie/Login 头域
    	-----------
    	P3P
        作用: 用于跨域设置Cookie, 这样可以解决iframe跨域访问cookie的问题
        例如: P3P: CP=CURa ADMa DEVa PSAo PSDo OUR BUS UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR    
        -----------
        Set-Cookie
        作用： 非常重要的header, 用于把cookie 发送到客户端浏览器， 每一个写入cookie都会生成一个Set-Cookie.
        例如: Set-Cookie: sc=4c31523a; path=/; domain=.acookie.taobao.com
    Entity头域
    	-----------
    	ETag
        作用:  和If-None-Match 配合使用。（实例请看上节中If-None-Match的实例）
        例如: ETag: "03f2b33c0bfcc1:0"
        -----------
        Last-Modified
        作用： 用于指示资源的最后修改日期和时间。（实例请看上节的If-Modified-Since的实例）
        例如: Last-Modified: Wed, 21 Dec 2011 09:09:10 GMT
        -----------
        Content-Type
        作用：WEB服务器告诉浏览器自己响应的对象的类型和字符集,
        例如:
        Content-Type: text/html; charset=utf-8
        Content-Type: image/jpeg
        -----------
        Content-Length
        指明实体正文的长度，以字节方式存储的十进制数字来表示。在数据下行的过程中，Content-Length的方式要预先在服务器中缓存所有数据，然后所有数据再一股脑儿地发给客户端。
        例如: Content-Length: 19847
        -----------
        Content-Encoding
        WEB服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。
        例如：Content-Encoding：gzip
        -----------
        Content-Language
        作用： WEB服务器告诉浏览器自己响应的对象的语言者
        例如： Content-Language:da
    Miscellaneous 头域
    	Server:
        作用：指明HTTP服务器的软件信息
        例如:Server: Microsoft-IIS/7.5
        X-Powered-By:
        作用：表示网站是用什么技术开发的
        例如： X-Powered-By: ASP.NET
	Transport头域
		Connection
        例如：　Connection: keep-alive   当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问
        这个服务器上的网页，会继续使用这一条已经建立的连接
        例如：  Connection: close  代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭， 当客户端再次发送Request，
        需要重新建立TCP连接。
    Location头域
    	Location
        作用： 用于重定向一个新的位置, 包含新的URL地址
        实例请看304状态实例
###61、HTTP协议是无状态的和Connection: keep-alive的区别
	无状态是指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。从另一方面讲，打开一个服务器上的网页和你之前打开这个服务器上的网页
	之间没有任何联系
    HTTP是一个无状态的面向连接的协议，无状态不代表HTTP不能保持TCP连接，更不能代表HTTP使用的是UDP协议（无连接）
    从HTTP/1.1起，默认都开启了Keep-Alive，保持连接特性，简单地说，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会
    关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接
    Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间
###62、keepalive是如何保持的
	keepalive是tcp连接中的会话保持的一种机制，链接建立之后，如果应用程序或者上层协议一直不发送数据，或者隔很长时间才发送一次数据，当链接很
	久没有数据报文传输时如何去确定对方还在线，到底是掉线了还是确实没有数据传输，链接还需不需要保持，这种情况在TCP协议设计中是需要考虑到的。
	TCP协议通过一种巧妙的方式去解决这个问题，当超过一段时间之后，TCP自动发送一个数据为空的报文给对方，如果对方回应了这个报文，说明对方还在线，
	链接可以继续保持，如果对方没有报文返回，并且重试了多次之后则认为链接丢失，没有必要保持链接。
###63、HTTP/1.1协议中共定义了八种方法（有时也叫“动作”）
	OPTIONS - 返回服务器针对特定资源所支持的HTTP请求方法。也可以利用向Web服务器发送'*'的请求来测试服务器的功能性。
    HEAD- 向服务器索要与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以在不必传输整个响应内容的情况下，就可以获取包含在响应消息
    头中的元信息。该方法常用于测试超链接的有效性，是否可以访问，以及最近是否更新。
    GET - 向特定的资源发出请求。注意：GET方法不应当被用于产生“副作用”的操作中，例如在web app.中。其中一个原因是GET可能会被网络蜘蛛等随意访问。
    POST - 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
    PUT - 向指定资源位置上传其最新内容。
    DELETE - 请求服务器删除Request-URI所标识的资源。
    TRACE- 回显服务器收到的请求，主要用于测试或诊断。
    CONNECT - HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
    PATCH - 用来将局部修改应用于某一资源，添加于规范RFC5789。
###64、什么是HTTPS？
	https://blog.csdn.net/xiaoming100001/article/details/81109617
	HTTPS是身披SSL外壳的HTTP。HTTPS是一种通过计算机网络进行安全通信的传输协议，经由HTTP进行通信，利用SSL/TLS建立全信道，加密数据包。
	HTTPS使用的主要目的是提供对网站服务器的身份认证，同时保护交换数据的隐私与完整性。
	基于HTTP协议，通过SSL或TLS提供加密处理数据、验证对方身份以及数据完整性保护
	https://blog.51cto.com/11883699/2160032
	信息通信采用http是不安全的，存在信息劫持、篡改的风险，https是加密传输，是安全的通信，对于https加密的过程，我们首先介绍的对称加密，采用
	对称加密进行通信存在秘钥协商过程的不安全性，因此我们采用了非对称加密算法解决了对协商过程的加密，因此https是集对称加密和非对称加密为一体
	的加密过程
	如何安全的获取公钥，并确保公钥的获取是安全的， 那就需要用到终极武器了：SSL 证书（需要购买）和CA机构
###65、Linux ps命令
	ps -l 
		将目前属于您自己这次登入的 PID 与相关信息列示出来
	ps aux
		列出目前所有的正在内存当中的程序
###15、怎么对命令进行取别名？
	alias ll='ls -l'
###15、mysql索引是B+tree(B加树)，和B-tree(B树)有什么区别
	https://blog.csdn.net/pangchengyong0724/article/details/76168576
###15、什么是微服务的无状态化？以及如何实现
	https://blog.csdn.net/zysgdhf4253/article/details/80421701
	把状态放在中间件中，保证业务模块的无状态化
###15、nginx是几层负载均衡？
	https://www.cnblogs.com/kevingrace/p/6137881.html
	nginx可以做4层负载，也可以做7层负载，stream形式是4层负载。
	七层负载均衡基本都是基于http协议的，适用于web服务器的负载均衡。（nginx）
    四层负载均衡主要是基于tcp协议报文，可以做任何基于tcp/ip协议的软件的负载均衡。(haproxy、LVS)
###15、公钥私钥数据签名理解
	https://www.cnblogs.com/shenkebky/p/8461841.html
	公钥加密，私钥解密。
    私钥数字签名，公钥验证。
###15、RSA公钥加密算法原理？
	RSA算法基于一个十分简单的数论事实：将两个大素数相乘十分容易，但那时想要对其乘积进行因式分解却极其困难，因此可以将乘积公开作为加密密钥。
###15、kafka和rabbitmq对比与消息队列选型？
	https://blog.csdn.net/u013256816/article/details/79838428
	kafka支持消息回溯，多用于日志系统
	rabbitmq功能性更好
###15、RabbitMQ的Broker组成？
	Broker主要由Exchange和Queue组成
	Exchange负责接收消息、转发消息到绑定的队列；Queue存储消息，提供持久化、队列等功能。AMQP客户端通过Channel与Broker通信，Channel是多
	路复用连接中的一条独立的双向数据流通道。
###15、一致性Hash原理，以及用途
	https://www.cnblogs.com/lpfuture/p/5796398.html
	解决分布式缓存问题
	环形解决分布式缓存核心问题(节点变动)；虚拟节点，解决数据倾斜问题
###15、HTTP中Get与Post的区别
	根据HTTP规范，GET用于信息获取，而且应该是安全的和幂等的
###15、有哪些防止哈希冲突的办法
###15、快排最坏情况的数据特征是什么样子
###15、ABA问题
###15、泛型相关
    Java里的泛型分3种：
    1.类级别的泛型
    2. 方法级别的泛型
    3.属性或变量级别的泛型
###设计原则有哪些
    单一职责原则
    开闭原则
    里氏替换原则
    依赖倒置原则
    接口隔离原则
    迪米特法则
###15、Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎

###15、zk的选举过程
###15、spring的事务传播机制

###16、KMP算法
    概要：http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html
    深入：http://wiki.jikexueyuan.com/project/kmp-algorithm/define.html
    "部分匹配"的实质是，有时候，字符串头部和尾部会有重复。比如，"ABCDAB"之中有两个"AB"，那么它的"部分匹配值"就是2（"AB"的长度）。
    搜索词移动的时候，第一个"AB"向后移动4位（字符串长度-部分匹配值），就可以来到第二个"AB"的位置。
###16、AQS默认是独享模型还是共享模型
    This class supports either or both a default <em>exclusive</em> mode and a <em>shared</em> mode.
    默认是独享
    Usually, implementation subclasses support only
     * one of these modes, but both can come into play for example in a
     * {@link ReadWriteLock}. Subclasses that support only exclusive or
     * only shared modes need not define the methods supporting the unused mode.
    但ReadWriteLock是唯一支持两种模型的子类
###16、线程的同步时可以使一个线程阻塞而等待一个信号，同时放弃锁使其他线程可以能竞争到锁。在synchronized中我们可以使用Object的wait()和notify方法实现这种等待和唤醒，但是在Lock中怎么实现这种wait和notify呢
    实现Condition接口，AQS里面的内部类ConditionObject就是做这个的
###16、什么是CLH锁？
    CLH(Craig, Landin, and Hagersten  locks): 是一个自旋锁，能确保无饥饿性，提供先来先服务的公平性。
    CLH锁也是一种基于链表的可扩展、高性能、公平的自旋锁，申请线程只在本地变量上自旋，它不断轮询前驱的状态，如果发现前驱释放了锁就结束自旋。
###16、为什么ArrayList是可序列化的，但elementData字段却被transient修饰？
      writeObject 和 readObject方法
      序列化时需要使用 ObjectOutputStream 的 writeObject() 将对象转换为字节流并输出。而 writeObject() 方法在传入的对象存在 writeObject() 的时候会去反射调用该对象的 writeObject() 来实现序列化。反序列化使用的是 ObjectInputStream 的 readObject() 方法，原理类似。
      是为了保证只序列化实际存储的那些元素，而不是整个数组，从而节省空间和时间。
      为了性能考虑，只序列化里面的元素，空余的槽位不需要序列化。
###16、什么情况下double类型的数据运算会有精度丢失？
    https://tool.oschina.net/hexconvert/  
    https://blog.csdn.net/weixin_44259720/article/details/87010388  
    https://blog.csdn.net/marco__/article/details/102515668  
    https://blog.csdn.net/Ibelievesunshine/article/details/100076965  
    https://jingyan.baidu.com/article/425e69e6e93ca9be15fc1626.html  
    这个问题我们先要知道二进制小数是如何转换十进制的。
    然后会发现二进制转换十进制的时候会产生循环，导致二进制无法停止，而double只有64位bit，后面多出的部分只能舍弃，造成精度丢失。
    运算中，所有十进制小数都刚刚可以转换成64位以内的二进制的数，则精度不会丢失
```java
public class Temp08 {
    public static void main(String[] args) {
        //不会有误差，因为4.75在转换二进制的时候不会有精度损失
        System.out.println(4.75 + 4.75 + 4.75);
        //会有误差，因为4.2在转换二进制的时候会有精度损失
        System.out.println(4.2 + 4.2 + 4.2);
    }
}
```    

###17、既然浮点型存在精度损失，那么他存在的意义是什么呢？
    有些场景不关心精度损失，更关心计算速度。比如显卡