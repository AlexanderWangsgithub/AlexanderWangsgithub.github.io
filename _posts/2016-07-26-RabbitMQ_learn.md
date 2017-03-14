---
layout: post
title: RabbitMQ的一些业务使用总结
categories: [blog]
tags: [RabbitMQ]
description: 
---

# 一些业务使用RabbitMQ的总结

* TOC
{:toc}

## Core理解

理解几个概念，Queue是根据Bingdings规则绑定到Exchange的，消息是发布到Exchange上的，服务器决定消息投递到哪个队列。

### flow

```flow
st=>start: Producer
e=>end: Consumer
opExchange=>operation: Exchange
opBlinding=>operation: Blinding
opQueue=>operation: Queue
st->opExchange->opBlinding->opQueue->e
```

basic.consumer: 订阅，消费后最近的消息后**自动接收**下一条消息。

basic.get: 获得单条。

Consumer获得消息需要返回basic.ack，一般采用auto_ack。

basic.reject允许Consumer拒绝消息。basic.reject=true?重发:删除

### connection&channel

许多channels共享一个TCP connection。单个channel的生产消费能力就十分强了，而且channel的创建代价不高，无需维护连接池，最多只需要维护connection的连接池即可。

关于connection&channel的双层连接池的设计比较没用，并且因为channel的自动重连机制而难以维护，我实现过，压测结果发现没啥卵用问题不少。

### Bindings

```
channel.queue_bind(exchange=exchange_name,
                   queue=queue_name,
                   routing_key='city.boardcast')
```



### Exchange

#### direct exchange

可以通过routingKey匹配投递到对应队列。

如果相同名称的routingKey绑定了多个队列的话就相当于fanout了。

#### fanout exchange

只能广播用

#### topic exchange

应用最为广泛的。该exchange与direct exchange基本相同，区别是可以匹配规则。

譬如说，CityProducer生产了`routing_key='city.boardcast'`，StateProducer生产了`routing_key='state.boardcast'`，如果我现在有一个queue=boardcast_queue需要接受city和state所有的消息，只需要`routing_key='*.boardcast'`

```
channel.queue_bind(exchange=exchange_name,
                   queue=boardcast_queue,
                   routing_key='*.boardcast')
```

routing_key按照`.`来分割，匹配任意字符`*`，匹配所有routing_key`#`

### VHOST

顾名思义——虚拟服务器，与RMQ服务器关系类似于OS和虚拟机。

vhost之间绝对隔离，不同的vhost里的exchange、queue不能通行。所以对于一个业务来说，建立一个vhost保证了安全性。

默认vhost是`/`。

## Consumer

消费就两种，一种推一种拉，推就是走订阅模式，mq通过长连接推到客户端；拉就是客户端不对的rpc到server去polling。

### 推示例

```java
    Connection connection = factory.newConnection();
    Channel channel = connection.createChannel();
    channel.basicQos(1);
    channel.exchangeDeclare(getExchange(), "topic", true);
    channel.queueBind(getQueueName(), getExchange(), getRoutingKey());
    QueueingConsumer consumer = new QueueingConsumer(channel);
    channel.basicConsume(getQueueName(), true, consumer);
    QueueingConsumer.Delivery delivery = null;
    try {
        delivery = consumer.nextDelivery();
    } catch (Exception ex) {
        logger.error("next delivery failed: ", ex);
        if (ex instanceof InterruptedException ||
                ex instanceof ShutdownSignalException ||
                ex instanceof ConsumerCancelledException) {
            consumer = null;
        }
        continue;
    }
    //do with delivery.getBody()
```

### 拉

```
channel.basicGet("queue_name", autoAck=true)
```

退的好处是及时消费，拉的好处是可以在客户端控制速率，避免尖峰把服务打挂。

