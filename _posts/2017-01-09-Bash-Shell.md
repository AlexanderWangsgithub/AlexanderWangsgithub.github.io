---
layout: post
title: Java or Python程序员的shell快速教程
categories: [blog]
tags: [Efficiency]
description: 
---

Java or Python程序员的shell快速教程

[TOC]

## 介绍

```
#!/bin/bash
#usage ./initbook.sh GitbookGuide
echo Your gitbook name is $1
mkdir $1
mkdir "${1}_pages"

cd $1
echo ".DS_Store" > .gitignore
```

看上面的示例，`#!/bin/bash`表示解释器，`#`表示注释，`$1`是第一个输入参数。

将脚本保存为`initbook.sh`，执行的时候，先`chmod +x initbook.sh`赋给执行权限，然后`./initbook.sh GitbookGuide`即可运行脚本。

对于Shell编程，一般用来做简单的脚本，方便使用linux下awk，sed，grep，crontab等非常方便的命令，以完成服务器任务。

除了各种命令行工具外，理解了变量的使用基本流利写shell了，逻辑语句都大差不差的。

## 变量

```shell
language="shell"
echo $language
echo ${language}
```

花括号用来区分边界，相当于`()`

```
for language in Java Python Erlang Elixir; do
    echo "I write ${language}'s program"
done
```

变量名和等号之间不能有空格



### String

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
- 单引号字串中不能出现单引号（对单引号使用转义符后也不行）

#### 获取字符串长度

```
string="hello"
echo ${#string}
```

#### 提取子字符串

```
string="hello world"
echo ${string:1:4} 
```

输出：elllo

#### 查找子字符串

```
string="hello world"
echo `expr index "$string" o` 
```

输出：8，这个语句的意思是：找出单词is在这名话中的位置

[more](http://tldp.org/LDP/abs/html/string-manipulation.html)

## 逻辑语句

### for

```
for file in `ls $dir1`
do cp -r "$dir1$file" $dir2
done
```

### if

```
if condition1
then
    command1
elif condition2
    command2
else
    commandN
fi
```

跟Python差不多。

示例：

```shell
#!/bin/bash
export KEEPER_DIR="ature/hell"
successFunc(){
	echo $KEEPER_DIR" is current success";
}
failFunc(){
	echo $KEEPER_DIR" is current error";
}
#注意空格
if [[ $KEEPER_DIR == master || $KEEPER_DIR == release || $KEEPER_DIR == develop || $KEEPER_DIR =~ feature.* ]];then successFunc; else failFunc; fi
```



### while

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```



## 命令行工具

陆续写成[GitBook](https://alexanderwangsgithub.github.io/Linux_Command_Tutorial)

地址：https://alexanderwangsgithub.github.io/Linux_Command_Tutorial/

## 注意点

### copy dir

cp目录时`cp -r`不好使，使用for遍历

```shell
pathAbs=`pwd`
dir1="$pathAbs/$projectName/_book/"
dir2="${projectName}_pages"

for file in `ls $dir1`
do cp -r "$dir1$file" $dir2
done
```



## 附录

- [一些Sample](http://tldp.org/LDP/abs/html/)

- [Unix Shell Programming](http://www.tutorialspoint.com/unix/unix-shell.htm)
- [Linux Shell Scripting Tutorial - A Beginner's handbook](http://bash.cyberciti.biz/guide/Main_Page)

