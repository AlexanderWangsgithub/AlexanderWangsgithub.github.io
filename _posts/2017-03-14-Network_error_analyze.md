---
layout: post
title: 推送引发的网络故障分析
categories: [blog]
tags: [TCP&IP]
description: 
---

推送引发的网络故障分析

* TOC
{:toc}


## 故障

从去年12月份开始，发现到某第三方push服务在每天某个时刻固定的抖一下。查了几天，查出服务本身存在的一些问题，但也都知道不是root cause，然后突然又正常了，最后查不下去了，不了了之。最近两天又开始了，这会有了详细的日志和运维的配合，可以一下追下去了。

首先我们发现有线程组handle住了，通过日志分析出到某推的大量长时间连接。某推说它们那个点业务正常，拍胸脯没问题，于是我们就双方抓个包看看，然后发现大量的数据包重复发送。

## Error

### TCP Out-Of-Order

TCP Out-Of-Order就是字面意思，乱序了。可能原因有丢包导致重传，网络两端间有不止一个通道进行负载均衡导致。

这个错误非常多。

### Tcp dup ack

tcp重复应答，这个也很多。

### Tcp retransmission

重传。



## Test

发一些TCP请求进行测试。

### hping3

```
-c：发送数据包的个数
-d：每个数据包的大小
-S：发送SYN数据包
-w：TCP window大小
-p：目标端口，你可以指定任意端口
–flood：尽可能快的发送数据包
–rand-source：使用随机的IP地址，目标机器看到一堆ip，不能定位你的实际IP；也可以使用-a或–spoof隐藏主机名
```

如`hping3 -S --flood -rand--source -p 80 192.168.1.199`

[详细文档](http://man.linuxde.net/hping3)



