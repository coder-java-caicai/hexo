layout: tomcat并发优化--tomcat
title: Tomcat的四种基于HTTP协议的Connector性能比较
date: 2016-04-28 17:23:26
tags: tomcat
---
Tomcat 6 支持 NIO -- Tomcat的四种基于HTTP协议的Connector性能比较

Tomcat从5.5版本开始，支持以下四种Connector的配置分别为：


<!--基于NIO的http协议的Connector 性能最好--->
<Connector port="8081" protocol="org.apache.coyote.http11.Http11NioProtocol"  connectionTimeout="20000" redirectPort="8443"/>

<!--tomcat默认http协议的Connector 性能最差-->
<Connector port="8081" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443"/>

<!--http连接池协议的Connector   性能差- ->   
<Connector executor="tomcatThreadPool" port="8081" protocol="HTTP/1.1"connectionTimeout="20000"redirectPort="8443" />

<!--基于NIO链接池协议的Connector 性能较好-->
<Connector executor="tomcatThreadPool" port="8081" protocol="org.apache.coyote.http11.Http11NioProtocol"connectionTimeout="20000"redirectPort="8443" />

我们姑且把上面四种Connector按照顺序命名为 NIO, HTTP, POOL, NIOP

为了不让其他因素影响测试结果，我们只对一个很简单的jsp页面进行测试，这个页面仅仅是输出一个Hello World。假设地址是 http://tomcat1/test.jsp

我们依次对四种Connector进行测试，测试的客户端在另外一台机器上用ab命令来完成，测试命令为： ab -c 900 -n 2000http://tomcat1/test.jsp，最终的测试结果如下表所示(单位:平均每秒处理的请求数)：

NIO    HTTP    POOL    NIOP
281     65     208     365
666     66     110     398
692     65     66     263
256     63     94     459
440     67     145     363

由这五组数据不难看出，HTTP的性能是很稳定，但是也是最差的，而这种方式就是Tomcat的默认配置。NIO方式波动很大，但没有低于280 的，NIOP是在NIO的基础上加入线程池，可能是程序处理更复杂了，因此性能不见得比NIO强；而POOL方式则波动很大，测试期间和HTTP方式一样，不时有停滞。

由于linux的内核默认限制了最大打开文件数目是1024，因此此次并发数控制在900。

尽管这一个结果在实际的网站中因为各方面因素导致，可能差别没这么大，例如受限于数据库的性能等等的问题。但对我们在部署网站应用时还是具有参考价值的。



这个可以利用apache server的 ab测试
C:\Program Files (x86)\Apache Software Foundation\Apache2.2\bin>ab -n2000 -c100 http://localhost:8080/

或者

C:\Program Files (x86)\Apache Software Foundation\Apache2.2\bin>ab -n2000 -c100 -w http://localhost:8080/ >c:/a.html


-n 代表请求数
-c 代表并发数
-w 输出到
