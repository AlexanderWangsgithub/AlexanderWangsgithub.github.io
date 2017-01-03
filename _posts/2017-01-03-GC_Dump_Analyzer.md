---
layout: post
title: 关于个推推送GC异常分析
categories: [blog]
tags: [Java]
description: 
---

关于个推推送GC异常分析

[TOC]

## Dump

生成dump，同时看看堆栈

`jmap -dump:format=b,file=01-03_dump.bin pid`&`jstack pid`

scp

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