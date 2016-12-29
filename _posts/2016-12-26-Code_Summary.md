---
layout: post
title: 一些代码总结
categories: [blog]
tags: [Java]
description: 
---

一些代码总结

[TOC]

## Java

### List和Set

- List的顺序是往尾部插入，Set是往头部插入，所以index有差别。
- List里面不可以插入null，Set可以插入null，并且永远占据index(0)，可以remove(null)。
- subList(i, j)注意，0<= i < j <= length。



## Redis

- 可往redis存null，但是最好不要存空对象。

eg：list.sub(0,0)，其class为java.util.RandomAccessSubList，不利于工具解析。

- 存储拍平了的对象时可以使用hash，有效节省空间。

所谓拍平了指`Map<String, String>`，而不是`Map<String, Object>`，可以用工具类拍平结构，对象转Json。

批量删除

`redis-cli -h 127.0.0.1 -p 6379 keys "cache:arena_message-NoticeCache:arena:BoardcastReviseDataSwitch:*" | xargs redis-cli -h 127.0.0.1 -p 6379 del` 将127.0.0.1替换为redis服务器地址。

## JDK

- static变量的使用

如果涉及并发修改的话直接用分布式应用，zk、etcd或redis都可以，不涉及修改，直接final好了。



