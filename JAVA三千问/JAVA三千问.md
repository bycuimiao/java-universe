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
###7、
###7、
###7、
###7、
###7、
###7、
###7、
