---
layout: post
title: 使用Jupyter制作slides
categories: [blog]
tags: [Python]
description: 
---

# 使用Jupyter制作slides

Jupyter note非常适合写教程，帮助别人节省时间，如果需要进行一场分享会的话，可以生成slides作为PPT使用。

[TOC]

## Env

- iPython

Jupyter就是iPython Notebook的新名字。

- nbconvert

用于将notebook转换成html

> 关于环境，请看[Jupyter Notebook](https://alexanderwangsgithub.github.io/blog/Jupyter_notebook.html)

## Usage

### start

```shell
cd $notebookDir
jupyter notebook
```

### labels

菜单栏选择View—>Toggle Toolbar—>打开

菜单栏选择View—>Cell Toolbar—>Slidesshow—>选择

- Slide 

单个view，左右滑动切换

- Sub-Slide

Cell的sub-cell，上下滑动切换

- Fragment

这个是Slide或Sub-Slide的属性，可以按次序展示，单击一次出现一条

- Skip

跳过，注释非演示代码用的

- Notes

在页面按s就可以跳出来的注释



## Reveal

### themes

[Sky](http://www.slideviper.oquanta.info/tutorial/slideshow_tutorial_slides.html?theme=sky#/11), [Beige](http://www.slideviper.oquanta.info/tutorial/slideshow_tutorial_slides.html?theme=beige#/11), [Serif](http://www.slideviper.oquanta.info/tutorial/slideshow_tutorial_slides.html?theme=serif#/11), etc.

### transitions

[Cube](http://www.slideviper.oquanta.info/tutorial/slideshow_tutorial_slides.html?transition=cube#/11), [Zoom](http://www.slideviper.oquanta.info/tutorial/slideshow_tutorial_slides.html?transition=zoom#/11), [None](http://www.slideviper.oquanta.info/tutorial/slideshow_tutorial_slides.html?transition=none#/11), etc.

## gen

```shell
jupyter-nbconvert --to slides Python_Share.ipynb --reveal-prefix '//cdn.bootcss.com/reveal.js/3.2.0' --output Python_Share
```

## server

```shell
python -m SimpleHTTPServer 8000
```

