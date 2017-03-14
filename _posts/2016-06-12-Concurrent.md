---
layout: post
title: 并发(Concurrent)
categories: [blog]
tags: [Java]
description: 
---

# 并发(Concurrent)

* TOC
{:toc}

## 概念
### 并发与并行
并发在于“争”，多个线程争一个资源，譬如争用、死锁等问题；
并行在于“分享”，将计算问题拆分，交给多个线程去计算，譬如Map-Reduce。

### 乐观锁与悲观锁
乐观锁不锁住竞争资源，而是假设无锁，不断的去尝试访问，直到访问成功。
悲观锁是锁住竞争资源，只有获得锁的线程可以访问，其他的都挂起。
乐观锁有CAS算法，譬如Atomic包下的并发，通过调用C最后通过CPU实现CAS，指令级的消耗小；悲观锁有Synchronized，jvm会将没有获得锁的线程挂起，context切换大；轻度中度使用中建议使用非阻塞算法CAS。
### 死锁（ Deadlock）、饥饿（ Starvation）和活锁（ Livelock）
死锁循环占用，永远循环下去。
饥饿是一直等着，可能因为优先级低或者某个资源一直被其它线程占用着，可能有机会执行。
活锁是两个线程碰到了互相避让，sleep后又撞上了。



## 使用技巧

使用多线程的原因：一个CPU单位时间只能被一个线程使用，对于多个事件开启的线程，在事件空闲时期让其他事件的线程访问CPU资源。

java：多线程。

python：多进程，一个进程对应一个线程。协程。



JDK的线程优先级不用管。



start()

suspend(), resume(), stop() 不要用

循环判断boolean代替stop()

等待／通知机制替代resume()

Thread.join()等待线程结束



python协程变量

`from gevent local import local`