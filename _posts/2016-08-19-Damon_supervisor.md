---
layout: post
title: 守护进程之supervisor
categories: [blog]
tags: [SOA]
description: 
---

# 守护进程之supervisor

​   SOA服务需要守护进程保证服务不会挂掉，可以在Container启动的时候或者使用发布系统启动Damon，SOA中间件同时接收到supervisor对上一个Git Head的INT信号，实现新的git commit的发布，这里面涉及到一个注册中心通知服务的时间。
​	这意味着，可以在发布系统接入supervisor实现更好的管控。

[TOC]

## 介绍

> Supervisor is a client/server system that allows its users to control a number of processes on UNIX-like operating systems.

## 安装supervisor

```shell
   brew install supervisor
```

```shell
   sudo apt-get install supervisor
```

## 修改配置文件

   ```
   sudo vim /etc/supervisord.d/arena.api.melody.ini
   ```

   跳到最后一行，插入以下内容：

   ```
   $ cat service.ini
   [program:arena.api.test]
   command=sh script/start_service.sh //要后台运行的命令[program:程序名]
   autorestart=true  //程序随supervisor启动
   process_name=arena.api.test
   numprocs=1
   stopsignal=INT
   directory=/data/arena.api.test
   user=usertest
   redirect_stderr=true
   stdout_logfile=/data/log/supervisor/arena.api.test.out  //指定标准日志输出文件
   stdout_logfile_maxbytes=100MB
   stdout_logfile_backups=10
   stopasgroup=true
   killasgroup=true
   ```

   可使用模版生成配置文件

   ```
   echo_supervisord_conf > /etc/supervisord.d/servername.conf
   ```

## 启动supervisor和server

   ```
   sudo service supervisor start
   ps -ef | grep servername
   ```

   

## 客户端控制
作为C/S架构，supervisord是服务端，可以通过客户端supervisorctl控制服务端的运行。 

   ```
   sudo supervisorctl start servername
   sudo supervisorctl stop servername
   sudo supervisorctl restart servername
   sudo supervisorctl reload
   sudo supervisorctl start all
   ```
