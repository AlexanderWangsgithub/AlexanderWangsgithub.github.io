---
layout: post
title: Jupyter使用的教程
categories: [blog]
tags: [Python]
description: 
---

# Jupyter使用的教程

[TOC]

## 如何本地运行本教程
1. 安装Jupyter到本地，详见[Jupyter Notebook](https://alexanderwangsgithub.github.io/blog/Jupyter_notebook.html)
2. 安装git后，执行git clone 
3. 到笔记本目录下，执行`jupyter notebook`

## 菜单栏
- File
- Edit
- View
- Insert
- Cell
- Kernel
- Help

### File
##### New Notebook-->Python3 
打开一个新笔记
##### Open...
##### Make a Copy
##### Rename
##### Save and Checkpoint
Checkpoint是一个记录点
##### Revert to Checkpoint
返回到某一个记录点
##### Print View
##### Download as
- ipynb
- py
  会将`%%`（Jupyter命令）和`!`（Unix命令）转换为Python，挺有用的
- md
  直接转md，发博客方便
- html
- pdf

##### Close and Halt

### Edit
都是对Cell操作的，和ToolBar基本一致。
包括复制、粘贴、删除、合并、移动

### View
#### Toggle Header
是否显示头部信息，包括文件名和举例上次保存时间
#### Toggle ToolBar
是否显示工具栏
#### Cell ToolBar
- None
  啥也不展示
- Edit MetaData
  Cell的Json属性
```json
{
  "slideshow": {
    "slide_type": "subslide"
  }
}
```
- Raw Cell format
  应该是格式化
- Slideshow
  最有用的！用于制作Slides

### Insert
#### Insert Cell Above
#### Insert Cell Below

### Cell
ToolBar里基本都有

### Kernel
Restart和Reconnect相关
Change Kernel：换Python环境

### Help
一些教程链接
- Python
- IPython
- NumPy（数值数组矩阵运算）
- SciPy（微分积分）
- MatPlotlib（绘图）
- SymPy（数学符号）
- pandas（数据分析，画图）

## 如何使用Cell？

### 类型
- Code 
  可执行的代码，Jupyter命令、Unix命令、各种脚本语言代码
- Markdown
  可书写markdown
- Raw NBconvert
  应该是默认格式（不确定）
- Heading
  标题级别，相当于html里面的h1、h2……

### Jupyter命令
#### 示例
使用`%%`前缀或`%`，如下写入文件：


```python
%%writefile printName.py
print "Alexander Wang"
```

    Overwriting printName.py


打开刚才写入的文件


```python
%pycat printName.py
```

#### magics list
- `$$  $$` 
  用LaTex写公式，`$$ P(A \mid B) = \frac{P(B \mid A) , P(A)}{P(B)} $$`
- `%%bash`,`%%HTML`,`%%python2`,`%%ruby`
  指定解释器
- `%load`
  `%load ./printName.py`载入外部脚本

- `%env` 
  设置环境变量
- `%%writefile` 
  可以保存cell下面内容到文件
- `%pycat` 
  弹窗打开文件
- `%pdb` 
  调试程序
  -`%prun` 
  每个函数消耗的时间
- `%%time` 
  cell内代码的单次运行时间信息
- `%who` 
  列出所有的全局变量
  ……

### 快捷键
工具有个键盘图标可以看所有快捷键

`Esc + F` 在代码中查找、替换
`Esc + O` 在cell和输出结果间切换。
`Shift + J` 或 `Shift + Down` 选择下一个cell。
`Shift + K` 或 `Shift + Up` 选择上一个cell。
`Shift + M` 合并cell

### 使用Unix命令
加前缀`!`


```python
!pwd
```

    /Users/wanggang/workspace/learn-python


### 各种脚本语言
- `%%bash`,`%%HTML`,`%%python2`,`%%ruby`
  指定解释器，默认是启动Jupyter的python解释器


```python
print("hello")
```

    hello



```python
%%bash
echo "hello"
```

    hello


保存后，还可以生成slides，和ppt一样的。
[使用Jupyter制作slides](https://alexanderwangsgithub.github.io/blog/Jupyter-slides.html)
