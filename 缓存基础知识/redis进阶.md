#1、redis数据结构
##1、string
    底层结构有两种int，sds
	    int
	    sds
	    sds分为两种存储方式
		    raw 存储方式 	连续内存
		    embstr 不连续内存 长度小于等于REDIS_ENCODING_EMBSTR_SIZE_LIMIT（默认44）时使用embstr，否则是raw  不连续内存
	优势分析
	   1.embstr 编码将创建字符串对象所需的内存分配次数从 raw 编码的两次降低为一次
       2.释放 embstr 编码的字符串只需要调用一次内存释放函数，而释放 raw 编码需要调用两次
       3.因为 embstr 编码的字符串对象所有数据都保存在一块连续的内存中，所以比起 raw 编码的字符串4对象能更好地利用缓存带来的优势
	为什么转换?
	1M之前，扩容翻倍。1M之后，只增加1M
	
	数值整形用int保存，带小数点的用sds保存
##2、hash
    底层数据结构dict(字典)+链表(Redis 为了节约内存空间使用，zset 和 hash 容器对象在元素个数较少的时候，采用压缩列表 (ziplist) 进行存储。)
    redis所有key-value形式的数据都是用的dict，包括zset的value-score
    dict的渐进式rehash是特色
    dict初始化的时候有两个hash表，一个是空的，用于渐进式rehash
    hash攻击
    扩容条件
    正常情况下，当 hash 表中元素的个数等于第一维数组的长度时，就会开始扩容，扩容的新数组是原数组大小的 2 倍。不过如果 Redis 正在做 bgsave，为了减少内存页的过多分离 (Copy On Write)，Redis 尽量不去扩容 (dict_can_resize)，但是如果 hash 表已经非常满了，元素的个数已经达到了第一维数组长度的 5 倍 (dict_force_resize_ratio)，说明 hash 表已经过于拥挤了，这个时候就会强制扩容。
    搬迁操作埋伏在当前字典的后续指令中(来自客户端的hset/hdel指令等)，但是有可能客户端闲下来了，没有了后续指令来触发这个搬迁，那么Redis就置之不理了么？当然不会，优雅的Redis怎么可能设计的这样潦草。Redis还会在定时任务中对字典进行主动搬迁。
    前提是配置了activerehashing，允许服务器在周期函数中辅助进行渐进式rehash，该参数默认值是1(开启)
    服务器目前没有执行 BGSAVE 命令或者 BGREWRITEAOF 命令，并且负载因子大于等于1。
    服务器目前正在执行 BGSAVE 命令或者 BGREWRITEAOF 命令，并且负载因子大于等于5。
    扩容细节
    每次新增、修改、删除都会进行一步rehash，并记录rehash到第几个链表
    http://redisbook.com/preview/dict/incremental_rehashing.html
    sizemask 属性的值总是等于 size - 1 ， 这个属性和哈希值一起决定一个键应该被放到 table 数组的哪个索引上面。
##3、list
    ziplist
        连续内存
        适合存少量数据，修改可能会导致级联修改，即修改删除某一个值的时候，会导致所有数据都修改。
    quicklist
        quicklist 默认的压缩深度是 0，也就是不压缩。压缩的实际深度由配置参数list-compress-depth决定。为了支持快速的 push/pop 操作，quicklist 的首尾两个 ziplist 不压缩，此时深度就是 1。如果深度为 2，就表示 quicklist 的首尾第一个 ziplist 以及首尾第二个 ziplist 都不压缩。
        ----
   
##4、set
    intset
    dict
##5、zset
    dict 存储key-value key-score
    skiplist 存储key-score List
    当数据较少时，sorted set是由一个ziplist来实现的。
    当数据多的时候，sorted set是由一个dict + 一个skiplist来实现的。简单来讲，dict用来查询数据到分数的对应关系，而skiplist用来根据分数查询数据（可能是范围查找）。
    https://blog.csdn.net/yellowriver007/article/details/79021103
    
    有序集合对象的编码可以是ziplist或者skiplist。同时满足以下条件时使用ziplist编码：
    元素数量小于128个
    所有member的长度都小于64字节
    以上两个条件的上限值可通过zset-max-ziplist-entries和zset-max-ziplist-value来修改。
    ziplist编码的有序集合使用紧挨在一起的压缩列表节点来保存，第一个节点保存member，第二个保存score。ziplist内的集合元素按score从小到大排序，score较小的排在表头位置。
    skiplist编码的有序集合底层是一个命名为zset的结构体，而一个zset结构同时包含一个字典和一个跳跃表。跳跃表按score从小到大保存所有集合元素。而字典则保存着从member到score的映射，这样就可以用O(1)的复杂度来查找member对应的score值。虽然同时使用两种结构，但它们会通过指针来共享相同元素的member和score，因此不会浪费额外的内存。
    
    https://www.jianshu.com/p/cc379427ef9d
    
    很重要的博客！！
    https://www.jianshu.com/p/fb7547369655
    skiplist作为zset的存储结构，整体存储结构如下图，核心点主要是包括一个dict对象和一个skiplist对象。dict保存key/value，key为元素，value为分值；skiplist保存的有序的元素列表，每个元素包括元素和分值。两种数据结构下的元素指向相同的位置。
    
##6、stream
    Redis 5.0 又引入了一个新的数据结构 listpack
    listpack 的设计彻底消灭了 ziplist 存在的级联更新行为，元素与元素之间完全独立，不会因为一个元素的长度变长就导致后续的元素内容会受到影响。
    
    扩展LRU是什么？
    redis、mysql、java对LRU的实现的不同的区别？