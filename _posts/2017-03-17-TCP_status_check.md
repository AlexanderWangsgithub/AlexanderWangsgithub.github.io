---
layout: post
title: TCP状态排查
categories: [blog]
tags: [TCP&IP]
description: 
---

# TCP状态排查

* TCO
{:toc}


## 场景

短暂时间的超时，handle死了java线程组。

## 分析

可能原因：

1. TCP连接问题
  a. RST报文丢失
  b. FIN报文丢失
  c. 内核错误
2. 对方服务问题

## Fixed Way

抓一段出错时间TCP包，同时ping对方服务。

1. 先确定是否是网络问题，检查Out-Of-Order、retransmission。
2. 一条条TCP stream检查，是否有握手未完成，wait时间。
3. 确定是网络障碍还是服务问题。

实际排障情况：
一眼看过去乱序、重传低于5%，而我们有200个线程，故不是这个原因。
检查TCP stream，发现有ack等待时间超过6s的，说明网络没问题应该是对方服务问题，除非我们看到的那段正好网络断了，概率极低。
