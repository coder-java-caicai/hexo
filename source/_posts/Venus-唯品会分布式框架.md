---
title: Venus----唯品会分布式框架
date: 2016-04-28 17:25:23
tags: soa
---
http://wiki.hexnova.com/display/Venus/HOME

 HOME

附件：1
被Struct添加，被Struct最后更新于2015-Aug-26  (查看更改)
Venus（New Version：3.2.14） 是什么？

它是由(Venus service framework)+服务路由产品（Venus-Bus）+服务注册中心(Venus-Registry) 组合而成，提供远程服务。它着 开发简单、高性能、高并发能力 的服务端框架。
客户端与服务端之间的通讯对开发人员完全透明
他跟目前我们经常用到的框架：axis、CXF、Hessian WebService、Jboss Remoting等框架类似。
系统功能目标：
提供高性能的服务通讯框架
具备性能监控（可以清晰的看到每个服务执行的时间、过长可以通过监控告警出去）
具备流量控制（每个服务每个时刻的调用次数、每天的峰值情况、）
访问控制（服务的授权控制）
提供可选择性服务数据缓存（cache支持，key采用表达式，由框架提供缓存支持，而不需要编写任何cache相关的代码）
提供框架进行再次研发能力，提供interceptor、validator等接口。
提供高性能的服务总线（Venus-Bus），能够轻易接入高性能的服务总线。（Venus-Bus项目支持，针对该venus的协议，以后接入服务总线轻而易举）
开发方面：
服务接口定义清晰(接口、参数、校验、以及服务鉴权)
自动生成接口文档，以方便阅读接口声明
客户端框架快速开发、提供多种语言版本的客户端
提供3种服务调用方式（同步、异步、回调）
服务端框架提供多种协议提供服务，而不需要做额外的开发 
	语言支持情况
目前客户端SDK暂时只有java、PHP语言版本
服务端面向java语言
java 语言开发例子
演讲稿下载

Venus演讲稿.pdf
开发人员的关注点

1. 如何服务化

采用接口与实现分离，服务接口是一种契约，他与我们开发web Service类似。
java开发语言：采用对程序员友好的接口申明形式，开发人员不需要关心客户端与服务端之间的传输协议。
其他语言：可以通过该框架提供自定义协议或者Http协议（http协议即将在2.1.0版本release出来）进行交互
2. 服务接口定制

定义服务接口
接口参数命名
定义参数校验规则
Java语言服务接口尽量不要依赖其他项目. 接口层面只需要接口相关的参数对象类与服务类
异常定义
3. 接口参数校验

4. 提供3种交互方式

 请求应答模式：普通的request、response，一般用于接口有返回值
异步请求模式：通常用于接口无返回值，客户端并不关心服务器的处理结果，也不用关心服务器处理多少时间
异步回调模式：接口无返回值，处理通常消化大量时间，需要服务端通知处理结果的业务接口
源代码：

demo 的svn 地址：svn://svn.hexnova.com/venus/venus-helloworld/trunk
框架svn地址： svn://svn.hexnova.com/venus/venus-framework/trunk
svn的用户名：guest
svn密码：guest
Maven Repository

<repository><id>hexnova-open</id><url>http://maven.hexnova.com/nexus/content/groups/hexnova-open</url><releases><enabled>true</enabled><updatePolicy>never</updatePolicy></releases><snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots></repository>
目前版本情况：

trunk是最新开发版本，会不停有新东西进入
稳定版本: 3.2.14
最新版本:3.2.14
版本发布Blog地址： http://wiki.hexnova.com/pages/viewrecentblogposts.action?key=Venus
开发人员与blog

日志

日志: Venus 3.2.12 Release 创建：
Struct
2014-Sep-05
Venus
日志: Venus 3.2.10 Release 创建：
Struct
2014-Jun-25
Venus
日志: Venus 3.2.3 Released 创建：
Struct
2014-Feb-19
Venus
日志: Venus 3.0.9 Released 创建：
Struct
2013-Dec-25
Venus
日志: Venus 3.0.6 Released 创建：
Struct
2013-Nov-22
Venus
日志: Venus 3.0.4 Released 创建：
Struct
2013-Nov-14
Venus
日志: Venus 3.0.3 Released 创建：
Struct
2013-Oct-25
Venus
日志: Venus 3.0.2 Released 创建：
Struct
2013-Oct-21
Venus
日志: Venus 3.0.1 Released 创建：
Struct
2013-Oct-14
Venus
日志: Venus 2.3.0 Released 创建：
Struct
2012-Oct-08
Venus
日志: Venus 2.2.7 Released 创建：
Struct
2012-Sep-24
Venus
日志: Venus 2.2.6 Released 创建：
Struct
2012-Jun-28
Venus
日志: Venus 2.2.3 Released 创建：
Struct
2012-May-21
Venus
日志: Venus 2.0.4 Released 创建：
Struct
2012-Mar-31
Venus
日志: Venus 2.0.3 Released 创建：
Struct
2012-Mar-23
Venus
日志: Venus 2.0.1 Released 创建：
Struct
2012-Jan-02
Venus
日志: Venus 1.3.0 Released 创建：
Struct
2011-Dec-07
Venus
日志: Venus 1.2.0 Released 创建：
Struct
2011-Dec-01
Venus
日志: Venus 1.1.0 Released 创建：
Struct
2011-Nov-28
Venus

研发人员列表

昵称	角色	职责	开源社区	目前供职于
Struct	Member	负责架构设计、通信框架研发、服务框架研发	Hexnova	上海汽车工业集团
Daisy	Member	对象数据序列化、服务接口数据校验、服务框架研发	Hexnova	

Sunng	Member	服务框架研发	Hexnova	

Yuanjian Yi	Member 
PHP客户端开发	Hexnova 
上海由你网络科技有限公司 
huawei	Member 
服务注册中心	Hexnova 


样例：

简单的接口例子：HelloService.java
HelloService接口例子
package com.meidusa.venus.hello.api;

import com.meidusa.venus.annotations.Endpoint;
import com.meidusa.venus.annotations.Param;
import com.meidusa.venus.annotations.Service;
import com.meidusa.venus.notify.InvocationListener;

/**
 * Service framework的 HelloService 接口例子.</p>
 * 支持3种调用方式：</p>
 * <li> 请求应答模式：普通的request、response，一般用于接口有返回值</li>
 * <li> 异步请求模式：通常用于接口无返回值，客户端并不关心服务器的处理结果，也不用关心服务器处理多少时间</li>
 * <li> 异步回调模式：接口无返回值，处理通常消化大量时间，需要服务端通知处理结果的业务接口</li>
 *
 * @author Struct
 *
 */
@Service(name="HelloService",version=1)
publicinterface HelloService {

	/**
	 * 无返回结果的服务调用，支持回调方式，该服务在通讯层面上为异步调用
	 * @param name
	 * @param invocationListener 客户端的回调接口
	 */
	@Endpoint(name="sayHelloWithCallbak")
	publicabstract void sayHello(@Param(name="name") String name,
				      @Param(name="callback") InvocationListener<Hello> invocationListener);
	/**
	 * 无返回结果的服务调用，支持同步或者异步调用,
	 * 该接口申明：同步，并且接口申明异常
	 * @param name
	 */
	@Endpoint(name="sayHello",async=false)
	publicabstract void sayHello(@Param(name="name") String name) throws HelloNotFoundException;

	/**
	 * 无返回结果的服务调用，支持同步或者异步调用，无异常申明
	 * @param name
	 */
	@Endpoint(name="sayAsyncHello",async=true)
	publicabstract void sayAsyncHello(@Param(name="name") String name);


	/**
	 * 有返回结果的服务调用，该接口只能支持同步调用
	 * @param name
	 * @return
	 */
	@Endpoint(name="getHello",timeWait=10000)
	publicabstract Hello getHello(@Param(name="name") String name);
}
客户端TestCase编写
客户端TestCase
package com.meidusa.venus.hello.client;

import java.util.concurrent.CountDownLatch;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import com.meidusa.venus.exception.CodedException;
import com.meidusa.venus.hello.api.Hello;
import com.meidusa.venus.hello.api.HelloNotFoundException;
import com.meidusa.venus.hello.api.HelloService;
import com.meidusa.venus.notify.InvocationListener;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:/applicationContext-helloworld-client.xml")
public class TestHelloService {

	@Autowired
	private HelloService helloService;

	@Test
	public void saySync(){
		System.out.println(helloService.getHello("jack"));
	}

	@Test
	public void testSyncWithException(){
		try {
			helloService.sayHello("jack");
		} catch (HelloNotFoundException e) {
			System.out.println("throw an user defined HelloNotFoundException");
		}
	}

	@Test
	public void testAsync(){
		helloService.sayAsyncHello("jack");
	}

	@Test
	public void testCallback() throws Exception{
		//为了让回调完成，采用countDownLatch计数器方式，避免testcase主线程运行完成而回调未结束的问题
final CountDownLatch latch = new CountDownLatch(1);

                  //在正常的使用的代码中这个类需要单实例，避免过多的callback listener导致内存问题
                      InvocationListener<Hello> listener = new InvocationListener<Hello>() {
			public void callback(Hello myobject) {
				System.out.println(" async call back result="+myobject);
				latch.countDown();
			}

			@Override
			public void onException(Exception e) {
				if(e instanceof CodedException){
				    CodedException exception = (CodedException) e;
				    System.out.println(" async call back error:"+exception.getErrorCode()+",message="+exception.getMessage());
				}else{
				    System.out.println(" async call back message="+e.getMessage());
				}
				latch.countDown();

			}
		};

		helloService.sayHello("jack",listener);
		latch.await();
	}

}
服务端的实现
服务端对HelloService的简单实现
package com.meidusa.venus.hello.impl;

import java.math.BigDecimal;
import java.util.HashMap;
import java.util.Map;

import com.meidusa.venus.hello.api.Hello;
import com.meidusa.venus.hello.api.HelloNotFoundException;
import com.meidusa.venus.hello.api.HelloService;
import com.meidusa.venus.notify.InvocationListener;

public class DefaultHelloService implements HelloService {
	privateString greeting;
	publicString getGreeting() {
		return greeting;
	}

	public void setGreeting(String greeting) {
		this.greeting = greeting;
	}
	public Hello getHello(String name) {
		Hello hello = new Hello();
		hello.setName(name);
		hello.setGreeting(greeting);
		Map<String,Object> map = new HashMap<String,Object>();
		hello.setMap(map);
		map.put("1", 1);
		map.put("2", newLong(2));
		map.put("3", newInteger(3));
		hello.setBigDecimal(new BigDecimal("1.341241233412"));
		return hello;
	}

	public void sayHello(String name)  throws HelloNotFoundException {
		thrownew HelloNotFoundException(name +" not found");
	}

	@Override
	public void sayAsyncHello(String name) {
		System.out.println("method sayAsyncHello invoked");
	}

	public void sayHello(String name,
			InvocationListener<Hello> invocationListener) {
		Hello hello = new Hello();
		hello.setName(name);
		hello.setGreeting(greeting);
		Map<String,Object> map = new HashMap<String,Object>();
		hello.setMap(map);
		map.put("1", 1);
		map.put("2", newLong(2));
		map.put("3", newInteger(3));

		if(invocationListener != null){
			invocationListener.callback(hello);
		}

	}
}
最近的更新

Go语言客户端 go-venus-plugin
commented by deephex
Jan 22
Go语言客户端 go-venus-plugin
updated by Struct
(view change)
Jan 15
1. 各种客户端简单使用
updated by Struct
(view change)
Jan 15
go-venus-plugin.zip
attached by Struct
Jan 15
go-venus-plugin.zip
attached by Struct
Jan 15
JAVA语言客户端
commented by 匿名用户
2015-Oct-14
HOME
commented by wangzhenjun
2015-Oct-08
HOME
updated by Struct
(view change)
2015-Aug-26
JAVA语言客户端
commented by 匿名用户
2015-Aug-10
后端处理线程设置
updated by Struct
(view change)
2015-Jul-29
. Venus Http协议
updated by Struct
(view change)
2015-Jun-29
venus-http-adaptor-3.2.13-distribution.zip
attached by Struct
2015-Jun-29
JAVA语言客户端
updated by Struct
(view change)
2015-Jun-11
HOME
commented by zhuo
2015-Feb-14
Venus 3.2.12 Release
commented by sunstar_s
2015-Jan-14
Venus Eclipse Plugin发布
commented by 匿名用户
2015-Jan-10
Venus Eclipse Plugin发布
commented by 匿名用户
2015-Jan-05
HOME
commented by 匿名用户
2014-Nov-06
HOME
commented by 匿名用户
2014-Nov-05
Venus 3.2.12 Release
created by Struct
2014-Sep-05
More 

Navigate space

 
1. 各种客户端简单使用
2. 高级使用指南
3. Architecture
4. 协议以及交互序列图介绍
5. 性能测试工具--Service-benchmark
6. 性能测试(单客户端)
7. 极限测试(多客户端)
FAQ
Venus Eclipse Plugin发布
二、其他语言客户端
子页面 (10) 

  隐藏子页面  |  页面重排
页面: 1. 各种客户端简单使用 
页面: 2. 高级使用指南 
页面: 3. Architecture 
页面: 4. 协议以及交互序列图介绍 
页面: 5. 性能测试工具--Service-benchmark 
页面: 6. 性能测试(单客户端) 
页面: 7. 极限测试(多客户端) 
页面: FAQ 
页面: Venus Eclipse Plugin发布 
页面: 二、其他语言客户端 