查看慢查询是否开启:

    mysql> show variables like '%slow%';
    +---------------------+---------------------------------+
    | Variable_name       | Value                           |
    +---------------------+---------------------------------+
    | log_slow_queries    | OFF                             |
    | slow_launch_time    | 2                               |
    | slow_query_log      | OFF                             |
    | slow_query_log_file | /var/run/mysqld/mysqld-slow.log |
    +---------------------+---------------------------------+
    
配置my.cnf文件

    long_query_time = 1
    log-slow-queries = /var/run/mysqld/slowquery.log


    

/var/lib/mysql/slowquery.log

    mysql > set global slow_query_log=1;
    mysql > set global long_query_time=1;
    mysql > set global slow_query_log_file='mysql-slow.log';
    

#####必须配置：
    set global log_slow_queries = 1;



slow_query_log=1
slow_query_log_file = /var/log/mysql/slowquery.log
long_query_time = 0.5
log-queries-not-using-indexes


long_query_time = 0.5
log-slow-queries = /var/lib/mysql/slowquery.log


set global slow_query_log_file='/var/lib/mysql/slowquery.log';
 set global long_query_time=0.5;
 
slow_query_log_file = /var/log/mysql/slowquery.log


SET GLOBAL LOG_SLOW_TIME = 1;
SET GLOBAL LOG_QUERIES_NOT_USING_INDEXES = ON;