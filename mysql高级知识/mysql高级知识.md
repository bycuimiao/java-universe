1、查看最大线程数  
show variables like '%max_connections%';  
show global status like 'Max_used_connections';  
show status like 'Threads%';  
show full processlist;  