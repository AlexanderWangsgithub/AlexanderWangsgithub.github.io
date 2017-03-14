---
layout: post
title: SOA架构的考虑
categories: [blog]
tags: [SOA,Architecture]
description: 
---

微服务SOA架构的考虑

* TOC
{:toc}

## 说明

SOA是面向服务架构的意思，和微服务不是一个概念，譬如我上家公司的SOA就是通过ESB的形式实现服务注册和通信，这样的问题是随着业务变得复杂化不易维护。微服务本质是SOA，但是拆分了业务。

## 微服务适用情况

1. 业务复杂，并会越来越复杂；
2. 多种开发语言
3. 频繁发布
4. 监控（包括不同粒度和调用链）
5. 熔断

## 一些问题

### 分布式事务

#### 实现

1. 通过消息队列实现，通过消息调用其他服务。即[CORS_and_EventSourcing]({% post_url 2016-11-16-CORS_and_EventSourcing %})
2. [补偿措施]({% post_url 2016-11-17-Rollback_and_compensate %})



## 具体实现

一个微服务SOA中间件应该有以下部分:

​	Container、RPC框架、Register、Daemon、Heartbeat、LoadBalance、Monitor。

核心是RPC和Register。

[TOC]

# RPC

​	对于RPC（远程调用），最少我们得有Web API调用方式，一般为RESTful风格，通过http＋约定来实现远程调用，如http+json/xml，对于B/S来说，采用这种方式对B端更加友好。然而，这种方式毕竟效率不够高，对于MicroServer之间的RPC，显然直接使用二进制+TCP，通过C/S来约定更为高效，如ProtocolBuffers、thrift。


## Web API

### RPC通信协议

​	可以基于http+json作为协议，使用Netty来处理外部http请求。

​	如何设计？

```
struts{
	basic:Head,Exception;
	Request extends Head{Exception};
	Response extends Head{Exception};
}
```

### RPC协议传输

http协议。

ps：[RESTful](http://www.cnblogs.com/artech/p/3506553.html)

### RPC请求接收

​	http长连接请求接收基于Netty来实现异步阻塞，封一个HttpJsonServer，启动的时候加载一些ChannelHandler：JsonRequestDeserializer，异步处理的handler。我们可以在SOA Container里启动该server。

具体可以看：[异步处理模型]({% post_url 2016-06-13-Async_queue_program_model %})

### RPC请求解析

​	请求解析的实现是反序列化后通过解析json，得到类和接口，在通过动态代理实现调用。可以在这里做些trace。

其实就是把request和response都封到Task里，(再使用异步队列存任务，)Executor执行。



​	即Container—>HttpJsonServer—>HttpJsonTask{request—>pre—>filter—>run—>response}



## ClientToService

### RPC通信协议

基于TCP协议，使用二进制的数据包，可以使用Socket通信。

### RPC协议传输

### RPC请求解析



# 提纲

#### 问题：

有返回和无返回
请求是否幂等
超时，在s收到了请求；c请求了
异常处理：是否收到

#### 通信：

两将军问题，如何确认是对方收到消息，重复ack。

#### 数据:

##### 跨机器查询

方法：数据分区、数据镜像

##### 如何高可用—》写多份

数据一致性：弱（异步）、最终一致性（异步）、强（同步）
事务

##### Master-slave

lease机制
Master-Master merge conflict 

mysql 半同步
（阿里是半同步，城市之间）

##### 两阶段提交

1. try confirm or cancel?
2. confirm

##### 三阶段提交

状态机

##### paxos算法

##### etcd、zookeeper

##### cap系统

##### google分布式 2008 

##### 数据一致性

##### brw模型，dynameDB

有空再写。






