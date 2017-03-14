---
layout: post
title: 教妹子搭博客
categories: [blog]
tags: [Efficiency]
description: 
---

# 教妹子搭博客

* TOC
{:toc}

## 注册GitHub账号

[地址](https://github.com/)

用户名为yourname，下面会一直用到。

## 创建repository

repository的名字是yourname.github.io

```
git clone https://github.com/AlexanderWangsgithub/blog_template.git
mv blog_template yourname.github.io
```

## 修改config

打开项目目录，修改_config.yml，把YourName替换成你的用户名yourname，还有邮箱之类的也替换掉。

## Push到GitHub

```
git remote add origin https://github.com/yourname/yourname.github.io.git
git add .
git commit -m "init"
git push origin master
```

输入用户名密码

## Done！

打开链接：https://yourname.github.io/

## SSH

每次都输用户名密码比较麻烦，可以使用ssh的方式。

[教程](https://alexanderwangsgithub.github.io/blog/GitLab_and_GitHub_store_SSHKey.html)

## Markdown

[在线编辑&教程](https://maxiang.io/)

[编辑器Typora](https://typora.io/)，好用到想交钱。

写博客

在目录`_post`目录下血博客，按照日期-名字命名，开头指定下样式，照里面的例子模范吧。写完Push到GitHub。



