---
layout: post
title: 
categories: [blog]
tags: [Architecture]
description: 
---

CQRS and EventSourcing

[TOC]

## CQRS

QRS（Command Query Responsibility Segration）说白了就是职责分离。其实现就是一个异步状态机，将写操作变成异步事件。而这部分异步事件的设计使用EventSourcing实现。

### 好处是：

#### 减少并发带来的阻塞

多线程同时要执行写操作的时候因为锁的争用导致线程等待。对于MySQL来说，InnoDB引擎受到争用。而通过事件，将多线程的写请求放到事件队列里面，对于InnoDB来说就是串行的，不会有争用，同时带来了数据库写请求的稳定的QPS。

#### 批量commit事件

多线程的写请求过来，进入事件队列后可以合并多个事件一次提交。





[实现的例子](http://www.codeproject.com/Articles/555855/Introduction-to-CQRS)



## EventSourcing

事件溯源，指不直接修改数据而记录事件，再通过逻辑和事件来获得数据在某个时刻的状态。

好处：

不修改数据，没有并发及事务问题。

坏处：

代码逻辑不能变，否则没法溯源。



[DDD领域驱动设计]({% post_url 2016-11-16-DDD_thinking %})