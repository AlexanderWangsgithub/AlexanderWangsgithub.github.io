---
layout: post
title: 推送系统问题定位——打点
categories: [blog]
tags: [RabbitMQ]
description: 
---

推送系统问题定位——打点

* TOC
{:toc}

## 生产消息
可以是订单也可以是通知等。
往MQ发。
1. PushData里加时间戳。
2. 打点，监控入队情况。

## 消费消息
从MQ中读。
1. 打点，监控MQ消费情况。
2. 打点MQ里面等待时间。
3. 加时间戳。
4. 分发，dispatch到各个推送平台的WorkThreadPool。
5. Executor里打点，监控WorkThreadPool队列情况。
6. 加时间戳
7. 调用apns等返回后打点。

充分的打点，在任何一个环节出了问题都可以迅速定位。