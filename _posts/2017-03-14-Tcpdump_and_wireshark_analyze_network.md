---
layout: post
title: Tcpdump+Wireshark分析网络组合
categories: [blog]
tags: [TCP&IP]
description: 
---

# Tcpdump+Wireshark分析网络组合

{:toc}

## TCPDUMP

### programer

#### base

```
(1)tcp: ip icmp arp rarp 和 tcp、udp、icmp这些选项等都要放到第一个参数的位置，用来过滤数据报的类型
(2)-i eth1 : 只抓经过接口eth1的包
(3)-t : 不显示时间戳
(4)-s 0 : 抓取数据包时默认抓取长度为68字节。加上-S 0 后可以抓到完整的数据包
(5)-c 100 : 只抓取100个数据包
(6)dst port ! 22 : 不抓取目标端口是22的数据包
(7)src net 192.168.1.0/24 : 数据包的源网络地址为192.168.1.0/24
(8)-w ./target.cap : 保存成cap文件，方便用wireshark分析
```

#### logic

```
非 : ! or "not" (去掉双引号)  
且 : && or "and"  
或 : || or "or"
```

#### example

```
源端口为64841，目的host为aliyun-broker-01.vm.xxx.me的所有TCP包，并存到文件
tcpdump -i en0 '((tcp) and (src port 64841) and (dst host adct-eless-1.vm.elenet.me))' -w Desktop/mac_0314.cap
```

[更多了解](http://www.cnblogs.com/ggjucheng/archive/2012/01/14/2322659.html)



## Wireshark

Windows&Mac上的TCP分析神器，颜色标记状态已经follow stream、flow graph简直太方便。

我不用它抓包，只用它分析。

打开之前保存dump.cap。

1. 绿色表示网络没啥问题。

2. 对有问题条目，右击——>Follow——>TCP Stream

   ![alias](/img/post/2017-03-14/tcp_stream.png)

3. 菜单栏：statistics——>Flow Graph

![alias](/img/post/2017-03-14/flow_graph.png)