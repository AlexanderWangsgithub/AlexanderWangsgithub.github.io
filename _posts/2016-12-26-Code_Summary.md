---
layout: post
title: 一些代码总结
categories: [blog]
tags: [Java]
description: 
---

一些代码总结

* TOC
{:toc}

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

## 发布

多人协作需规范

master用于回退，release用于线上，develop用于多人协作alpha。

feature需合develop再发布、发版，以避免其他人feature丢失。

develop发snapshot

release发release

