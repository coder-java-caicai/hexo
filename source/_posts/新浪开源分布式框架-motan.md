---
title: 新浪开源分布式框架--motan
date: 2016-04-28 17:24:27
tags: soa
---
开源地址 https://github.com/weibocom/motan/wiki/zh_quickstart

zh_quickstart

axb edited this page 2 days ago · 2 revisions
 Pages 8

Documents

Overview
Quick Start
User Guide(Preparing)
Develop Guide(Preparing)
FAQ
中文文档

概述
快速入门
用户指南
开发指南(准备中)
常见问题
Clone this wiki locally

 Clone in Desktop
快速入门
简单调用示例
集群调用示例
使用Consul作为注册中心
使用ZooKeeper作为注册中心
快速入门中会给出一些基本使用场景下的配置方式，更详细的使用文档请参考用户指南.
如果要执行快速入门介绍中的例子，你需要:
JDK 1.7或更高版本。
java依赖管理工具，如Maven或Gradle。
简单调用示例

在pom中增加依赖
<dependency>
    <groupId>com.weibo</groupId>
    <artifactId>motan-core</artifactId>
    <version>0.0.1</version>
</dependency>
<dependency>
    <groupId>com.weibo</groupId>
    <artifactId>motan-transport-netty</artifactId>
    <version>0.0.1</version>
</dependency>

<!-- only needed for spring-based features -->
<dependency>
    <groupId>com.weibo</groupId>
    <artifactId>motan-springsupport</artifactId>
    <version>0.0.1</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.2.4.RELEASE</version>
</dependency>
为调用方和服务方创建接口。
src/main/java/quickstart/FooService.java
package quickstart;

publicinterfaceFooService {
    public String hello(String name);
}
实现服务方逻辑。
src/main/java/quickstart/FooServiceImpl.java
package quickstart;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

publicclassFooServiceImplimplementsFooService {

    public String hello(String name) {
        System.out.println(name +" invoked rpc service");
        return"hello "+ name;
    }

    publicstaticvoidmain(String[] args) throws InterruptedException {
        ApplicationContext applicationContext =new ClassPathXmlApplicationContext("classpath:motan_server.xml");
        System.out.println("server start...");
    }
}
src/main/resources/motan_server.xml
<?xml version="1.0" encoding="UTF-8"?>
<beansxmlns="http://www.springframework.org/schema/beans"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xmlns:motan="http://api.weibo.com/schema/motan"xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd   http://api.weibo.com/schema/motan http://api.weibo.com/schema/motan.xsd">

    <!-- service implemention bean -->
    <beanid="serviceImpl"class="quickstart.FooServiceImpl" />
    <!-- exporting service by motan -->
    <motan:serviceinterface="quickstart.FooService"ref="serviceImpl"export="8002" />
</beans>
执行FooServiceImpl类中的main函数将会启动motan服务，并监听8002端口.
实现服务调用方。
src/main/resources/motan_client.xml
<?xml version="1.0" encoding="UTF-8"?>
<beansxmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:motan="http://api.weibo.com/schema/motan"
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd   http://api.weibo.com/schema/motan http://api.weibo.com/schema/motan.xsd">

    <!-- reference to the remote service -->
    <motan:refererid="remoteService"interface="quickstart.FooService"directUrl="localhost:8002"/>
</beans>
src/main/java/quickstart/Client.java
package quickstart;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


publicclassClient {

    publicstaticvoidmain(String[] args) throws InterruptedException {
        ApplicationContext ctx =new ClassPathXmlApplicationContext("classpath:motan_client.xml");
        FooService service = (FooService) ctx.getBean("remoteService");
        System.out.println(service.hello("motan"));
    }
}
执行Client类中的main函数将执行一次远程调用，并输出结果。
集群调用示例

在集群环境下使用motan需要依赖外部服务发现组件，目前支持consul或zookeeper。
使用Consul作为注册中心

Consul安装与启动

安装（官方文档）

# 这里以linux为例
wget https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip
unzip consul_0.6.4_linux_amd64.zip
sudo mv consul /bin
启动（官方文档）

测试环境启动：
consul agent -dev
ui后台 http://localhost:8500/ui
motan-Consul配置

在server和client中添加motan-registry-consul依赖
<dependency>
    <groupId>com.weibo</groupId>
    <artifactId>motan-registry-consul</artifactId>
    <version>0.0.1</version>
</dependency>
在server和client的配置文件中分别增加consul registry定义。
<motan:registryregProtocol="consul"name="my_consul"address="127.0.0.1:8500"/>
在motan client及server配置改为通过registry服务发现。
client
    <motan:refererid="remoteService"interface="quickstart.FooService"registry="my_consul"/>
server
    <motan:serviceinterface="quickstart.FooService"ref="serviceImpl"registry="my_consul"export="8002" />
server程序启动后，需要显式调用心跳开关，注册到consul。
MotanSwitcherUtil.setSwitcher(ConsulConstants.NAMING_PROCESS_HEARTBEAT_SWITCHER, true)
进入ui后台查看服务是否正常提供调用
启动client，调用服务
使用ZooKeeper作为注册中心

ZooKeeper安装与启动(官方文档)

单机版安装与启动
wget http://mirrors.cnnic.cn/apache/zookeeper/zookeeper-3.4.8/zookeeper-3.4.8.tar.gz
tar zxvf zookeeper-3.4.8.tar.gz

cd zookeeper-3.4.8/conf/
cp zoo_sample.cfg zoo.cfg

cd ../
sh bin/zkServer.sh start
motan-ZooKeeper配置

在server和client中添加motan-registry-zookeeper依赖
<dependency>
    <groupId>com.weibo</groupId>
    <artifactId>motan-registry-zookeeper</artifactId>
    <version>0.0.1</version>
</dependency>
在server和client的配置文件中分别增加zookeeper registry定义。
zookeeper为单节点
<motan:registryregProtocol="zookeeper"name="my_zookeeper"address="127.0.0.1:2181"/>
zookeeper多节点集群
<motan:registryregProtocol="zookeeper"name="my_zookeeper"address="127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183"/>
在motan client及server配置改为通过registry服务发现。
client
<motan:refererid="remoteService"interface="quickstart.FooService"registry="my_zookeeper"/>
server
<motan:serviceinterface="quickstart.FooService"ref="serviceImpl"registry="my_zookeeper"export="8002" />
启动client，调用服务