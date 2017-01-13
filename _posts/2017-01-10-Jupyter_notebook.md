---
layout: post
title: Jupyter Notebook
categories: [blog]
tags: [Python]
description: 
---



# Jupyter Notebook

[TOC]



## 

## 升级pip3

避免依赖问题

```shell
pip3 install --upgrade pip
```

```shell
➜  workspace git:(master) ✗ pip3 install jupyter

Could not find an activated virtualenv (required).
```

 `subl ~/.zshrc`将`export PIP_REQUIRE_VIRTUALENV=false`改为false。

## zlib

#### Question

```shell
➜  workspace git:(master) ✗ brew install zlib
Updating Homebrew...
Error: No available formula with the name "zlib"
==> Searching for similarly named formulae...
This similarly named formula was found:
lzlib
To install it, run:
  brew install lzlib
==> Searching taps...
This formula was found in a tap:
homebrew/dupes/zlib
To install it, run:
  brew install homebrew/dupes/zlib
```

#### fixed way

```
brew tap homebrew/dupes
```

#### fixed brew

```
Run brew update twice.
Run brew doctor and fix all the warnings (outdated Xcode/CLT and unbrewed dylibs are very likely to cause problems).
```

[Solution From](https://github.com/Homebrew/brew/blob/master/docs/Troubleshooting.md)

#### then

```
ln -s /usr/local/Cellar/zlib/1.2.8/include/* /usr/local/include 
ln -s /usr/local/Cellar/zlib/1.2.8/lib/* /usr/local/lib
```

## 安装

```
pip3 install jupyter
```

## 使用

在笔记本目录下

```
jupyter notebook
ipython notebook #old version
```

## 分享

提交GitHub后，可利用https://nbviewer.jupyter.org/可视化分享