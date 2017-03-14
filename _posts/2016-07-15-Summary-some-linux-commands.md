---
layout: post
title: Summary some linux commands 
categories: [blog]
tags: [Efficiency]
description: 
---

summary some linux commands 

* TOC
{:toc}

## awk

```
awk 'BEGIN{FS="/";OFS="\0"}{print "https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-",$5,".jpg"}' test
```
```
jps|awk '{print $1}' |xargs -t kill -9 {}
```

FS 输入字段分隔符（缺省为:space:）

OFS输出字段分隔符（缺省为:space:）

RS输出字段分隔符（缺省为:\n）

ORS输出字段分隔符（缺省为:\n）

##### 模式

杀死占用进程

`lsof -i :8090|awk '$2 !~ /PID/ {print $2}'|xargs kill -9`

~是模式匹配，!~是反模式

[陈皓教程](http://coolshell.cn/articles/9070.html)

## vi

### 命令模式：

G跳到文件尾

dw删除当前字符到单词尾包括空格的所有字符

yw复制当前光标所在位置到单词尾字符的内容到vi缓存区，相当于复制一个单词
y$复制光标所在位置到行尾内容到缓存区
y^复制光标所在位置到行首内容到缓存区

### 末行模式：

替换
　　:s/old/new用new替换行中首次出现的old
　　: s/old/new/g 用new替换行中所有出现的old
　　:#,# s/old/new/g用new替换从第＃行到第＃行中出现的old
　　：% s/old/new/g用new替换整篇中出现的old

## sed

stream editor

末尾加g表示全部替换

```
cat sample-http-server|sed 's/21_1/24/g'
sed -i 's/21_1/24/g' sample-http-server直接修改文件，慎重
```

批量替换文件名

```shell
for file in 0.*.mp3;do echo "$file";mv -f "$file" `echo "$file"|sed 's/^0\.\ //'|sed 's/\ /_/g'`;done
```



[manual](http://coolshell.cn/articles/9104.html)

## tcpdump

`sudo tcpdump -X -i en0 udp port 8125`

-i any:监听所有的介面。这样你就知道是不是有流量产生。

-n:不要解决主机名，以IP数字形式显示主机。

-nn:不要解析主机名或端口名字。

-X:同时以十六进制和ASCII字符显示包的数据。

-XX: 同-X,但也会显示Ethernet头部。

-v, -vv, -vvv:详细，更详细，再详细些！ 冗余输出得到的包信息。

-c: 抓取 x个包后就停下。

-S: 打印绝对序号。

-e:同时得到Ethernet头部。

-q: 显示少一点协议信息。

-E: 用提供的密钥解密IPSEC 流量。

-s:设置显示前多少个字节的包内容（snaplength）。

表达式：

```
# 源主机是192.168.1.34, 并且目标端口是80
tcpdump -i eth0 src 192.168.1.34 and dst port 80
# 所有tcp协议并且端口不是22
tcpdump -i eth0 tcp and not port 22
有时候表达式太复杂了, 我们要进行分组, 即用()把一些表达式组合在一起.
# 源主机是192.168.1.34并且目标端口是22或者3389
tcpdump -i eth0 'src 192.168.1.34 and (dst port 22 or 3389)'
我们还可以通过分析数据包其中的部分数据计算过滤:
# 抓取带有SYN标志的数据, tcp[13]的意思是从tcp数据包头中取第14个byte数据, 详细参考tcp协议头
sudo tcpdump -i wlan0 'host www.baidu.com and tcp[13]&2!=0'
```

## 小命令

### lsof -i

查看端口占用

`lsof -i :8090`

杀死占用进程

`lsof -i :8090|awk '$2 !~ /PID/ {print $2}'|xargs kill -9`

~是模式匹配，!~是反模式



### ps -ef



### pkill

`pkill java`

### curl

```shell
curl -o filename url
```

### splite
压缩分割文件
```
tar zcvf - lantern-installer-beta.dmg |split -b 1m - lantern_install.tar.gz
```
解压
```
cat lantern_install.tar.gza* |tar zx
```



## redis批量删除

```
➜  ~ redis-cli -h 192.168.112.94 -p 6379 keys "cache:arena_message-NoticeCache:*" | xargs redis-cli -h 192.168.112.94 -p 6379 del
```

登录：`redis-cli -h 192.168.112.94 -p 6379`

列举所有key：`redis-cli keys "cache:arena_message-NoticeCache:*" `