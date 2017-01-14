---
layout: post
title: ThreadPoolExecutor备忘录
categories: [blog]
tags: [Java]
description: 
---

# ThreadPoolExecutor备忘录



ThreadPoolExecutor之前在做一些异步处理的时候会用到，最近的业务代码又需要使用，发现有些概念忘记了，所以打算记下来，方便自己记忆。

[TOC]



写在前面，下面引用部分，来自[链接](http://www.imooc.com/article/5887)

# 一些参数-引用

## 参数

```
corePoolSize：核心线程数
        * 核心线程会一直存活，及时没有任务需要执行
        * 当线程数小于核心线程数时，即使有线程空闲，线程池也会优先创建新线程处理
        * 设置allowCoreThreadTimeout=true（默认false）时，核心线程会超时关闭

    2、queueCapacity：任务队列容量（阻塞队列）
        * 当核心线程数达到最大时，新任务会放在队列中排队等待执行

    3、maxPoolSize：最大线程数
        * 当线程数>=corePoolSize，且任务队列已满时。线程池会创建新线程来处理任务
        * 当线程数=maxPoolSize，且任务队列已满时，线程池会拒绝处理任务而抛出异常

    4、 keepAliveTime：线程空闲时间
        * 当线程空闲时间达到keepAliveTime时，线程会退出，直到线程数量=corePoolSize
        * 如果allowCoreThreadTimeout=true，则会直到线程数量=0

    5、allowCoreThreadTimeout：允许核心线程超时
```



## 自带移除策略

两种情况会拒绝处理任务：

- 当线程数已经达到maxPoolSize，切队列已满，会拒绝新任务
- 当线程池被调用shutdown()后，会等待线程池里的任务执行完毕，再shutdown。如果在调用shutdown()和线程池真正shutdown之间提交任务，会拒绝新任务。线程池会调用rejectedExecutionHandler来处理这个任务。

| 策略                  | 说明                                       |
| ------------------- | ---------------------------------------- |
| AbortPolicy         | 当任务添加到线程池中被拒绝时，它将抛出 RejectedExecutionException 异常 |
| CallerRunsPolicy    | 当任务添加到线程池中被拒绝时，会在线程池当前正在运行的Thread线程池中处理被拒绝的任务 |
| DiscardOldestPolicy | 当任务添加到线程池中被拒绝时，线程池会放弃等待队列中最旧的未处理任务，然后将被拒绝的任务添加到等待队列中 |
| DiscardPolicy       | 当任务添加到线程池中被拒绝时，线程池将丢弃被拒绝的任务              |

线程池默认的处理策略是**AbortPolicy**

## 自定义移除策略

实现RejectedExecutionHandler接口

```java
public class PushRejectedPolicy implements RejectedExecutionHandler {
    private static Log logger = LogFactory.getLog(PushRejectedPolicy.class);

    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        if (!executor.isShutdown()) {
            Runnable thread = executor.getQueue().poll();
            logger.warn("PushRejectedPolicy_%s", JsonUtil.writeAsString(thread));
            executor.execute(r);
        }
    }
}
```

## 自定义ThreadPoolFactory

如下，将poolName加上，方便定位问题。

```java
public class PushThreadFactory implements ThreadFactory {
    private static final AtomicInteger poolNumber = new AtomicInteger(1);
    private final ThreadGroup group;
    private final AtomicInteger threadNumber = new AtomicInteger(1);
    private final String namePrefix;

    public PushThreadFactory(String poolName) {
        SecurityManager s = System.getSecurityManager();
        group = (s != null) ? s.getThreadGroup() :
                Thread.currentThread().getThreadGroup();
        namePrefix = poolName + "-" +
                poolNumber.getAndIncrement() +
                "-thread-";
    }

    public Thread newThread(Runnable r) {
        Thread t = new Thread(group, r,
                namePrefix + threadNumber.getAndIncrement(),
                0);
        if (t.isDaemon())
            t.setDaemon(false);
        if (t.getPriority() != Thread.NORM_PRIORITY)
            t.setPriority(Thread.NORM_PRIORITY);
        return t;
    }
}
```



## 执行顺序

```
        线程池按以下行为执行任务
    1. 当线程数小于核心线程数时，创建线程。
    2. 当线程数大于等于核心线程数，且任务队列未满时，将任务放入任务队列。
    3. 当线程数大于等于核心线程数，且任务队列已满
        - 若线程数小于最大线程数，创建线程
        - 若线程数等于最大线程数，抛出异常，拒绝任务
```

## 如何设置参数

```
    1、默认值
        * corePoolSize=1
        * queueCapacity=Integer.MAX_VALUE
        * maxPoolSize=Integer.MAX_VALUE
        * keepAliveTime=60s
        * allowCoreThreadTimeout=false
        * rejectedExecutionHandler=AbortPolicy()

    2、如何来设置
        * 需要根据几个值来决定
            - tasks ：每秒的任务数，假设为500~1000
            - taskcost：每个任务花费时间，假设为0.1s
            - responsetime：系统允许容忍的最大响应时间，假设为1s
        * 做几个计算
            - corePoolSize = 每秒需要多少个线程处理？ 
                * threadcount = tasks/(1/taskcost) =tasks*taskcout =  (500~1000)*0.1 = 50~100 个线程。corePoolSize设置应该大于50
                * 根据8020原则，如果80%的每秒任务数小于800，那么corePoolSize设置为80即可
            - queueCapacity = (coreSizePool/taskcost)*responsetime
                * 计算可得 queueCapacity = 80/0.1*1 = 80。意思是队列里的线程可以等待1s，超过了的需要新开线程来执行
                * 切记不能设置为Integer.MAX_VALUE，这样队列会很大，线程数只会保持在corePoolSize大小，当任务陡增时，不能新开线程来执行，响应时间会随之陡增。
            - maxPoolSize = (max(tasks)- queueCapacity)/(1/taskcost)
                * 计算可得 maxPoolSize = (1000-80)/10 = 92
                * （最大任务数-队列容量）/每个线程每秒处理能力 = 最大线程数
            - rejectedExecutionHandler：根据具体情况来决定，任务不重要可丢弃，任务重要则要利用一些缓冲机制来处理
            - keepAliveTime和allowCoreThreadTimeout采用默认通常能满足

    3、 以上都是理想值，实际情况下要根据机器性能来决定。如果在未达到最大线程数的情况机器cpu load已经满了，则需要通过升级硬件（呵呵）和优化代码，降低taskcost来处理。
```

# Feature

## 任务前后

afterExecute() ：任务执行之前

beforeExecute() ：任务执行之前

terminated() : 整个线程池停止之后执行





# Understanding

任务Worker是个Runnable，用AbstractQueuedSynchronizer实现了

互斥锁（非重入）。



关于AQS

[*The java*.*util*.*concurrent Synchronizer Framework*](http://gee.cs.oswego.edu/dl/papers/aqs.pdf)

[深入浅出 Java Concurrency (7): 锁机制 part 2 AQS](http://www.blogjava.net/xylz/archive/2010/07/06/325390.html)

