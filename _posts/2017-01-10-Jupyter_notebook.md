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



## pyenv

pyenv可以用来管理和切换不同的python版本

.zshrc中添加

```shell
xport PYENV_ROOT=/usr/local/opt/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
```

usage:

```
pyenv versions
pyenv install --list
pyenv global 3.6.0
pyenv rehash #使用 pip 安装模块后，更新数据库；
pyenv uninstall #卸载某个版本
pyenv update #更新 pyenv 及其插件
```

## virtualenv

```
pip install virtualenv
cd project_path
virtualenv venv
virtualenv -p /usr/local/bin/python3 venv #指定解释器
deactivate
```

## 使用

在笔记本目录下

```
jupyter notebook
ipython notebook #old version
```

## 分享

提交GitHub后，可利用https://nbviewer.jupyter.org/可视化分享