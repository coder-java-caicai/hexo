---
title: memcache安装及配置
date: 2016-04-28 17:16:27
tags: memcached
---
CentOS6.4安装Memcached与使用例子

1、安装libevent
    wget http://monkey.org/~provos/libevent-1.4.14b-stable.tar.gz
    tar zxvf libevent-1.4.14b-stable.tar.gz
    cd libevent-1.4.14b-stable
    ./configure --prefix=/usr/local/libevent/
    make && make install
    ln -s /usr/local/libevent/lib/libevent-1.4.so.2 /lib/libevent-1.4.so.2
   /**
      1) 如果共享库文件安装到了/lib或/usr/lib目录下, 那么需执行一下ldconfig命令

ldconfig命令的用途, 主要是在默认搜寻目录(/lib和/usr/lib)以及动态库配置文件/etc/ld.so.conf内所列的目录下, 搜索出可共享的动态链接库(格式如lib*.so*), 进而创建出动态装入程序(ld.so)所需的连接和缓存文件. 缓存文件默认为/etc/ld.so.cache, 此文件保存已排好序的动态链接库名字列表. 

2) 如果共享库文件安装到了/usr/local/lib(很多开源的共享库都会安装到该目录下)或其它"非/lib或/usr/lib"目录下, 那么在执行ldconfig命令前, 还要把新共享库目录加入到共享库配置文件/etc/ld.so.conf中, 如下:

# cat /etc/ld.so.conf
include ld.so.conf.d/*.conf
# echo "/usr/local/lib" >> /etc/ld.so.conf
# ldconfig

3) 如果共享库文件安装到了其它"非/lib或/usr/lib" 目录下,  但是又不想在/etc/ld.so.conf中加路径(或者是没有权限加路径). 那可以export一个全局变量LD_LIBRARY_PATH, 然后运行程序的时候就会去这个目录中找共享库. 

LD_LIBRARY_PATH的意思是告诉loader在哪些目录中可以找到共享库. 可以设置多个搜索目录, 这些目录之间用冒号分隔开. 比如安装了一个mysql到/usr/local/mysql目录下, 其中有一大堆库文件在/usr/local/mysql/lib下面, 则可以在.bashrc或.bash_profile或shell里加入以下语句即可:

export LD_LIBRARY_PATH=/usr/local/mysql/lib:$LD_LIBRARY_PATH    

一般来讲这只是一种临时的解决方案, 在没有权限或临时需要的时候使用.

4）如果程序需要的库文件比系统目前存在的村文件版本低，可以做一个链接
比如：
error while loading shared libraries: libncurses.so.4: cannot open shared
object file: No such file or directory

ls /usr/lib/libncu*
/usr/lib/libncurses.a   /usr/lib/libncurses.so.5
/usr/lib/libncurses.so  /usr/lib/libncurses.so.5.3

可见虽然没有libncurses.so.4，但有libncurses.so.5，是可以向下兼容的
建一个链接就好了
ln -s  /usr/lib/libncurses.so.5.3  /usr/lib/libncurses.so.4

**/

    
2、安装Memcached
    wget http://www.danga.com/memcached/dist/memcached-1.2.0.tar.gz
    tar zxvf memcached-1.4.15.tar.gz
    cd memcached-1.4.15
    ./configure --prefix=/usr/local/memcached/ --with-libevent=/usr/local/libevent/
    make && make install
3、启动Memcached
     /usr/local/memcached/bin/memcached -d -m 64 -u root -l 127.0.0.100 -p 11211 -c 128 -P /tmp/memcached.pid
4、为了方便管理，写个SHELL脚本吧。
  
 vi /etc/rc.d/init.d/memcached
    #!/bin/sh
    #
    # memcached: MemCached Daemon
    #
    # chkconfig: - 90 25
    # description: MemCached Daemon
    #
    # Source function library.
    . /etc/rc.d/init.d/functions
    . /etc/sysconfig/network
    #[ ${NETWORKING} = "no" ] && exit 0
    #[ -r /etc/sysconfig/dund ] || exit 0
    #. /etc/sysconfig/dund
    #[ -z "$DUNDARGS" ] && exit 0
    start()
    {
    echo -n $"Starting memcached: "
    daemon $MEMCACHED -u daemon -d -m 64 -l 127.0.0.100 -p 11211 -c 128 -P /tmp/memcached.pid
    echo
    }
    stop()
    {
    echo -n $"Shutting down memcached: "
    killproc memcached
    echo
    }
    MEMCACHED="/usr/local/memcached/bin/memcached"
    [ -f $MEMCACHED ] || exit 1
    # See how we were called.
    case "$1" in
    start)
    start
    ;;
    stop)
    stop
    ;;
    restart)
    stop
    sleep 3
    start
    ;;
    *)
    echo $"Usage: $0 {start|stop|restart}"
    exit 1
    esac
    exit 0
5、添加Memcached开机启动
    cd /etc/rc.d/init.d/
    chmod 777 memcached
    chkconfig --add memcached
    chkconfig --level 235 memcached on
    chkconfig --list | grep memcached
6、Memcached使用
    [root@www.111cn.net] service memcached start
    [root@www.111cn.net] service memcached stop
    [root@www.111cn.net] service memcached restart