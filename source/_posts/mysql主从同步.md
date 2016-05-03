---
title: mysql主从同步
date: 2016-04-28 17:18:13
tags: mysql
---
前提条件两个数据库并且局域网能够访问并且两个数据库的版本号要一致（msyql -V 查看mysql版本号）。
否则容易出现莫名错误不好处理。
1:主数据库141（10.10.39.101）下面称作master
2:从数据库142（10.10.49.90）下面称作slave

配置如下
1:在主服务器上,设置一个从数据库的账户,使用REPLICATION SLAVE赋予权限,如:
mysql> GRANT REPLICATION SLAVE ON *.* TO 'slave001'@'10.10.49.90' IDENTIFIED BY '1qazZZR@WSX';

2:修改主数据库的配置文件my.cnf,开启BINLOG，并设置server-id的值，修改之后必须重启Mysql服务
[mysqld]
log-bin = mysql-bin
server-id=141

3:之后可以得到主服务器当前二进制日志名和偏移量，这个操作的目的是为了在从数据库启动后，从这个
点开始进行数据的恢复
mysql> show master status\G;
*************************** 1. row ***************************
File: mysql-bin.000003
Position: 243
Binlog_Do_DB:
Binlog_Ignore_DB:
1 row in set (0.00 sec)
4:停止对master数据库的更新然后导出需要同步的数据库，
mysql> flush tables with read lock;
mysqldump -h127.0.0.1 -p3306 -uroot -p zzr > /home/zzr.sql
mysql> unlock tables;
5：将刚才主数据备份的zzr.sql复制到从数据库，进行导入  
6：接着修改从数据库的my.cnf,增加server-id参数,指定复制使用的用户,主数据库服务器的ip,端口以及开始执
行复制日志的文件和位置
[mysqld]
server-id=142
log_bin = mysql-bin
master-host =10.10.39.101
master-user=slave001
master-pass=1qazZZR@WSX
master-port =20010
master-connect-retry=60
replicate-do-db =zzr,open
7:在从服务器上,启动slave进程	
mysql> start slave;
    如果启动失败：在slave上强制执行
    mysql> change master to master_host='master_host', master_user='you_user_name', master_password='you_master_user_password',master_log_file='mysqld-bin.000001',master_port=3306, master_log_pos=98;
   （master_log_file和master_log_pos）的值你可以在服务器上运行 show master status; 来得到。
    然后 mysql> start slave;启动slave进程
8:在从服务器进行show salve status验证	
mysql> SHOW SLAVE STATUS\G
9：检测一下。看看是否主从同步了。 