---
layout: post
title: 关于个推推送GC异常分析
categories: [blog]
tags: [Java]
description: 
---

关于个推推送GC异常分析

* TOC
{:toc}

## Dump

### 生成dump

`jmap -dump:format=b,file=01-03_dump.bin pid`

### 看堆栈

`jstack pid`

#### waiting for monitor entry

如果有大量线程bolcked，一般来说，可能是有全局锁

Java Thread State：

`java.lang.Thread.State: BLOCKED (on object monitor)`:等待进入临界区

#### waiting on condition

大多是资源request没有得到response，譬如第三方网络资源。

`"Elog-Appender-ASYNC" #20 prio=5 os_prio=0 tid=0x00007f8678431800 nid=0x2487 waiting on condition [0x00007f8626dd9000]`

`java.lang.Thread.State: WAITING (parking)`：等到条件发生

`java.lang.Thread.State: TIMED_WAITING (parking或sleeping)`：定时唤醒自己。

#### in Object.wait()

`"JVM-Monitor" #34 daemon prio=5 os_prio=0 tid=0x00007f861402a800 nid=0x2496 in Object.wait() [0x00007f8624fbe000]`

这个一般都跟RMI、GC相关

### scp

```
scp username@host:01-03_dump.bin ~/Desktop/01-03_dump.bin
```

## Analyzer

MAT导入file

### 1.看Top Consumers

 Print the most expensive objects grouped by class and by package.

可以看大对象以及数量占比，根据包来分类

### 2.Dominator Tree

看大对象,可以双指看属性跟下去。

### 3.Path to GC Roots

with all references

### btw

Histogram也经常用的，在分析一些基础对象把内存撑爆了的时候，最快了。



## Result

sun.security.ssl.SSLSocketImpl实例太多，导致频繁GC，一路追下去发现是Apns的问题，第三方jar，socket使用的姿势有问题，导致在高QPS情况下socket对象释放不掉。