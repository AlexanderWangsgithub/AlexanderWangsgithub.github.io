---
layout: post
title: 关于发布
categories: [blog]
tags: [DevOps]
description: 
---

# 关于发布

[TOC]

事实上，我踩过很多坑。我很庆幸有个技术强大&Oncall的leader能帮我兜住底。

该做一点总结了

#### 1.发alpha环境

##### 	a.验证业务正常。

##### 	b.业务曲线以及execption、transaction是否正常。

#### 2.发布prod环境

##### 	a.灰度发布

##### 	b.对于关键路径一定要灰度并观察曲线