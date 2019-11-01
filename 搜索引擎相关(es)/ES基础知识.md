###1、elasticsearch基于Lucene的分布式搜索引擎
###2、es基础架构
    es存储数据的单位是索引(index)
    index -> type -> mapping -> document -> field
    一个index有多条数据
    es的分布式：index存在shard里，一个index的存储数据被分布在多个shard，多个shard可能被分布在不同的机器上
    es的高可用：shard有多个备份，每个shard都有一个primary shard，和多个replica shard
    写数据是写到primary shard，primary shard会把数据同步到replica shard
    es集群有多个节点，会自动选举一个节点为master节点，负责维护索引元数据，出现故障的时候切换 primary shard和replica shard
    es集群读的时候可以从primary shard去读，也可以从replica shard去读
    一个es节点可以承载多个shard
###3、es写数据详细分析
    1、客户端发起写请求
    2、先找选中一个es节点作为协调节点
    3、协调节点根据数据进行hash，确认该数据要路由分发到哪个primary shard(可能需要分发到的shard不在该es节点上，需要发送到其他es节点的shard上)
    4、数据被写入指定的primary shard后，primary shard会将数据同步到其他replica shard上
    5、当其他replica shard都同步完数据后，由协调节点返回写成功应答给客户端
    数据写入primary shard细节
    1、数据同时写入内存buffer和translog日志
    ps:如果数据刚刚写入buffer和translog日志的时候有客户端来查询该数据，是查不到的。
    2、默认每隔1s 做一次refresh操作，刷一次buffer到os cache(操作系统的缓存)中，并清空buffer
    ps:数据被refresh到os cache中，就可以被查询到了。
    所以es是准实时的。NRT，near real-time。数据写入1s后才能看到
    3、每隔5s，os cache做一次fsync操作，将数据写入到segment file磁盘文件
    ps:buffer每次refresh就会产生一个segment file
    ps:数据写入segment file之后同时建立好倒排索引
    4、随着不断写入，translog会不断变大，当达到一定的阈值之后，会触发flush操作
    将buffer现有数据强行写入os cache，并清空buffer
    将一个commit point写入磁盘文件，这个commit point对应所有的segment file
    强行将os cache所有的数据都fsync到segment file磁盘文件中
    将translog清空，再次创建一个translog。默认30分钟执行一次，这个过程叫flush
    ps:写translog的时候不是直接把日志写到translog文件，而是先写入os cache，每隔5s中会把os cache的数据持久化到translog文件中
    所以es所在机器宕机的时候可能会丢失5s在os cache的数据。可以设置每次写入直接写入磁盘，但会把性能和吞吐量下降一个量级
###4、es删除文件
    删除数据实际上是把数据写入.del文件中的，标识该数据已经被删除。
    一旦某条数据被删除了，客户端搜索该数据的时候，在.del文件中发现该数据，就不会把该数据搜索出来
    那么什么时候做物理删除呢？
    由于buffer每次refresh就会产生一个segment file，所以file会越来越多，出发到阈值后会执行merge
    会把多个segment file和.del文件合成一个segment file，此时物理删除数据。
###5、es写流程4个底层核心概念：refresh、flush、translog、merge
###6、es查询和搜索数据
    查询：GET操作，获取某一条数据。
    写入某个document，这个document会自动分配(也可以指定)给你一个全局唯一的ID，doc id，同时也是根据doc id进行hash路由到对应的primary shard
    GET操作就是通过doc id获取document
    GET操作的时候，通过协调节点进行hash路由，然后根据负载均衡选取一个shard，并从中读取document
    搜索：根据关键字 全文检索
    通过协调节点，根据关键字查询各个shard，并将doc id汇总在协调节点，再通过doc id查询到所有的document，在协调节点再次筛选后返回最匹配的document
    
    
    
