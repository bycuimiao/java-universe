1、查看最大线程数  
show variables like '%max_connections%';  
show global status like 'Max_used_connections';  
show status like 'Threads%';  
show full processlist;  


https://www.cnblogs.com/f-ck-need-u/p/9001061.html  
1.错误日志(error log)：记录mysql服务的启停时正确和错误的信息，还记录启动、停止、运行过程中的错误信息。  
2.查询日志(general log)：记录建立的客户端连接和执行的语句。  
3.二进制日志(bin log)：记录所有更改数据的语句，可用于数据复制。  
4.慢查询日志(slow log)：记录所有执行时间超过long_query_time的所有查询或不使用索引的查询。  
5.中继日志(relay log)：主从复制时使用的日志。  


查询超出变量 long_query_time 指定时间值的为慢查询。但是查询获取锁(包括锁等待)的时间不计入查询时间内。  
mysql记录慢查询日志是在查询执行完毕且已经完全释放锁之后才记录的，因此慢查询日志记录的顺序和执行的SQL查询语句顺序可能会不一致(例如语句1先执行，查询速度慢，语句2后执行，但查询速度快，则语句2先记录)。  
默认超时时长为10秒

二进制日志
对于事务表的操作，二进制日志只在事务提交的时候一次性写入(基于事务的innodb二进制日志)，提交前的每个二进制日志记录都先cache，提交时写入。对于非事务表的操作，每次执行完语句就直接写入。

查看binlog是否开启
show variables like 'log_%'; 