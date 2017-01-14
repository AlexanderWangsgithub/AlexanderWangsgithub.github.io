---
layout: post
title: Python开发环境
categories: [blog]
tags: [Python] 
description:
---

# Python开发环境

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

virtualenv为project建立虚拟环境，必备。

```
pip install virtualenv
cd project_path
virtualenv venv
virtualenv -p /usr/local/bin/python3 venv #指定解释器
source venv/bin/activate #激活环境，激活后可以pip安装各种包了
deactivate
```

#### 保存环境

```
pip freeze > requirements.txt
```

#### 重新安装环境

```
pip install -r requirements.txt
```

