---
title: mysql跨数据库操作类似与oracle的dblink
date: 2016-04-28 17:21:31
tags: oracle
---
MySQL FEDERATED 存储引擎

              MySQL中针对不同的功能需求提供了不同的存储引擎。所谓的存储引擎也就是MySQL下特定接口的具体实现。
              FEDERATED是其中一个专门针对远程数据库的实现。一般情况下在本地数据库中建表会在数据库目录中生成相应的表定义文件，并同时生成相应的数据文件。
但通过FEDERATED引擎创建的表只是在本地有表定义文件，数据文件则存在于远程数据库中(这一点很重要)。
             通过这个引擎可以实现类似Oracle 下DBLINK的远程数据访问功能。
             使用show engines 命令查看数据库是否已支持FEDERATED引擎：
              
             Support 的值有以下几个：
             
YES	支持并开启
DEFAULT	支持并开启, 并且为默认引擎
NO	不支持
DISABLED	支持,但未开启

可以看出MyISAM为当前默认的引擎。
                使用FEDERATED建表语句如下：
                CREATE TABLE (......) ENGINE =FEDERATED CONNECTION='mysql://[name]:[pass]@[location]:[port]/[db-name]/[table-name]'
               创建成功后就可直接在本地查询相应的远程表了。
需要注意的几点：
              1. 本地的表结构必须与远程的完全一样。
              2.远程数据库目前仅限MySQL
              3.不支持事务
              4.不支持表结构修改
