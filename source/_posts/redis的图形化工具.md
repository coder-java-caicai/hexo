---
title: redis的图形化工具
date: 2016-04-28 17:15:35
tags: redis
---
redis的图形化工具

2015-10-29 Rails365 运维帮
1. 介绍

本篇会介绍几个关于redis的图形化的监控工具和管理工具。

2. redis-stat

redis-stat（https://github.com/junegunn/redis-stat）提供终端和web端的监控页面，它安装和使用起来很简单。

安装只需要一条指令。

$ gem install redis-stat

运行更简单。

$ redis-stat

效果图如下：



也可以以server的形式运行，默认情况下是监听在63790端口。

$ redis-stat --server

还可以以后台进程的形式开启服务。

redis-stat --server --daemon

效果图如下：




3. redis-browser

redis-browser（https://github.com/monterail/redis-browser）是redis的web端的图形化管理工具。利用它可以查看和管理redis的数据，界面很简洁，安装和使用这个工具也比较简单，而且它还能和rails应用结合在一起。

安装。

$ gem install redis-browser

要使用也只是需要一条命令。

$ redis-browser

效果图如下:



4. redmon

redmon也是一个监控redis运行情况的页面监控工具。

安装。

$ gem install redmon

使用。

$ redmon

效果图如下：



完结。

