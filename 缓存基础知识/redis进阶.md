1
redis数据结构
string
底层结构有两种int，sds
	int
	sds
	sds分为两种存储方式
		raw 存储方式 	连续内存
		embstr 字符串 长度小于等于REDIS_ENCODING_EMBSTR_SIZE_LIMIT（默认44）时使用embstr，否则是raw  不连续内存
	为什么转换，是因为内存分配翻倍，连续内存成本高。但连续内存查询快。
	1M之前，扩容翻倍。1M之后，只增加1M

hash
list
set
zset

扩展LRU是什么？
redis、mysql、java对LRU的实现的不同的区别？