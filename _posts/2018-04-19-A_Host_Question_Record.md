---
layout: post
title: head
categories: [blog]
tags: [TCP&IP]
description: 
---

* TOC
{:toc}
### 记一次请求排障问题

#### 问题描述

域名xxx.xxx.me下的接口访问很慢，绑完host就很快。

**链路**：内网电脑-->xxx.xxx.me-->nginx（10.10.10.10）-->API服务

**说明**：xy-api.alta.elenet.me的解析后地址：139.139.139.139

  139.139.139.139是nginx地址

#### curl测试：

 内网电脑-->通过域名 请求返回7s

 内网电脑-->通过nginx（10.10.10.10） 请求返回1s

推测

推测一：以为是内网DNS解析的问题；

​	结果：139.139.139.139是公网IP

推测二：从139.139.139.139到nginx慢了；

​	结果：从nginx往139.139.139.139ping了下很快，怀疑是ping认为3层网络很好，不能反映问题。

#### 求助

正好去年排查个推网络问题找的shell帮忙，问了下shell，得知curl可以看耗时。

如下：

```
curl 'xxx.xxx.me' -w '\ntime_total: %{time_total} time_namelookup: %{time_namelookup} time_connect: %{time_connect} time_appconnect: %{time_appconnect} time_redirect: %{time_redirect}'
```

结果：

```
time_total: 7.105 time_namelookup: 0.072 time_connect: 5.137 time_appconnect: 7.069 time_redirect: 0.000%
```

TCP连接花了5.137s，SSL花了（7.069-5.137）s，基本确认问题。

#### 证据

当然我们还需要一点证据

直接建立一个TCP连接，看看多长时间。

```
import time
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s2 = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

t = time.time()
#xxx.xxx.me'=139.139.139.139
s.connect(('139.139.139.139', 443))
print(time.time()-t)
s.close()

t2 = time.time()
s2.connect(('10.10.10.10', 443))
print(time.time()-t2)
s2.close()
```

结果一个5s，一个0.002s，可以让SLB提供方处理了。



附录：

[curl -w usage](http://blog.51cto.com/h2ofly/1957171)

