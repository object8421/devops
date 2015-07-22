# ActiveMQ 简明教程

ActiveMQ 是最流行的，能力强劲的开源消息总线。ActiveMQ 是一个完全支持JMS1.1和J2EE 1.4规范的 JMS Provider实现，尽管JMS规范出台已经是很久的事情了，但是JMS在当今的J2EE应用中间仍然扮演着特殊的地位。

## 参考资料

* [Active MQ 官网](http://activemq.apache.org/)
* [使用文档](http://activemq.apache.org/using-activemq-5.html)

## Features

* （**支持多种语言和协议**）Supports a variety of Cross Language Clients and Protocols from Java, C, C++, C#, Ruby, Perl, Python, PHP。

* （**完全支持企业集成模式的JMS客户端和Message Broker**）full support for the Enterprise Integration Patterns both in the JMS client and the Message Broker。

* (**支持许多高级功能,如消息组、虚拟目的地,通配符和复合目的地**) Supports many advanced features such as Message Groups, Virtual Destinations, Wildcards and Composite Destinations. 

* (**完全支持JMS 1.1和J2EE 1.4规范，如瞬态、持久性、事务和XA消息**) Fully supports JMS 1.1 and J2EE 1.4 with support for transient, persistent, transactional and XA messaging. 

* (**对Spring的支持**) Spring Support so that ActiveMQ can be easily embedded into Spring applications and configured using Spring's XML configuration mechanism.

* (**通过了常见服务器的测试，ActiveMQ可自动部署到任何支持J2EE 1.4的服务器上**) Tested inside popular J2EE servers such as TomEE, Geronimo, JBoss, GlassFish and WebLogic Includes JCA 1.5 resource adaptors for inbound & outbound messaging so that ActiveMQ should auto-deploy in any J2EE 1.4 compliant server. 

* (**支持多种传送协议**) Supports pluggable transport protocols such as in-VM, TCP, SSL, NIO, UDP, multicast, JGroups and JXTA transports. 

* (**支持通过JDBC和journal提供高速的消息持久化**) Supports very fast persistence using JDBC along with a high performance journal. 

* (**从设计上保证了高性能的集群，客户端-服务器，点对点**) Designed for high performance clustering, client-server, peer based communication. 

* (**REST API 的提供技术基于Web API的消息传递**) REST API to provide technology agnostic and language neutral web based API to messaging. 

* (**对Ajax的支持**) Ajax to support web streaming support to web browsers using pure DHTML, allowing web browsers to be part of the messaging fabric. 

* (**CXF 和 Axis 的支持**) CXF and Axis Support so that ActiveMQ can be easily dropped into either of these web service stacks to provide reliable messaging. 

* (**可以很容易得调用内嵌JMS provider，进行测试**) Can be used as an in memory JMS provider, ideal for unit testing JMS.


## 安装和使用

* Windows 只需下载activemq-xxx.zip解压即可完成安装。

* 启动ActiveMQ

```
  cd [activemq_install_dir]
  bin\activemq
```
启动用可访问：http://localhost:8161/

* 测试安装
访问 http://localhost:8161/ 或 http://localhost:8161/admin 可进入管理页面。
```
  Login/Password: admin / admin
  Navigate to "Queues"
  Add a queue name and click create
  Send test message by klicking on "Send to"
```

* 查看日志
```
 [activemq_install_dir]/data/activemq.log
```

* 停止服务
```
cd [activemq_install_dir]/bin
./activemq stop
```

* 配置

[参考配置链接](http://activemq.apache.org/getting-started.html#GettingStarted-ConfiguringActiveMQ)
  