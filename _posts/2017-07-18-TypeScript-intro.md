---
layout: post
title: TypeScript学习
categories: [blog]
tags: [Fe]
description: 
---

* TOC
{:toc}

## TypeScript

### Installation

1. `apm install atom-typescript` (`apm` needs `git` in your path)
2. Fire up atom. Open a TypeScript file. Potentially wait for further installs (just `apm install linter` if its not there already).

### tsconfig.json
"compileOnSave": true
### compiler
use the TypeScript compiler that  your node_modules installed.
### Format Code
Shortcut : ctrl+alt+l or cmd+alt+l. Will format just the selection if you have something selected otherwise it will format the entire file.


## NgModule

NgModule是一个装饰器函数，它接收一个用来描述模块属性的元数据对象。其中最重要的属性是：

```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

declarations - 声明本模块中拥有的视图类。Angular 有三种视图类：组件、指令和管道。
exports - declarations 的子集，可用于其它模块的组件模板。
imports - 本模块声明的组件模板需要的类所在的其它模块。
providers - 服务的创建者，并加入到全局服务列表中，可用于应用任何部分。
bootstrap - 指定应用的主视图（称为根组件），它是所有其它视图的宿主。只有根模块才能设置bootstrap属性。

### template

##### ngFor
```
  <ul>
    <li *ngFor="let code of defaultAction.codes">
      {{ code }}
    </li>
  </ul>
```