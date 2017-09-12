---
layout: post
title: Scala Snippet
categories: [blog]
tags: [Scala]
description: 
---

# Scala Snippet对比Java&Js

* TOC
{:toc}
### package object

package object是Scala2.8后的特性，package object完成了一个package软链接，同时package object本身与object并无其他区别。所以很明显，这是个为接口兼容诞生的特性。

所以在组织project时common function时，我们可以将同类型的function写在一个package下，而对于存在与不用package下同类型的对象软链到package里。

### 单例对象

即伴生对象，常用于工厂方法。

```
class A(foo: String)

object A {
  def apply(foo: String) = new A(foo)
}
```

apply类似于js的apply，支持将参数数组作为函数的参数，经典的apply([0,1,2,3])进行sum。

### 函数对象

Scala的函数式与Java本质上并无不同，一个类的方法是不能作为lambda函数调用的，只有继承了Function类才可以被作为lambda函数用，不过Scala提供了语法糖。

```
class SumOneV1 extends Function1[Int, Int] {
  def apply(i: Int): Int = i + 1
}
```

语法糖：

```
class SumOneV1 extends Function1[Int, Int] {
  def apply(i: Int): Int = i + 1
}

class SumOneV2 extends (Int => Int) {
  def apply(i: Int): Int = i + 1
}

object test {
  def main(args: Array[String]) {
    val sumOne1 = new SumOneV1()
    val sumOne2 = new SumOneV2()
    val sumOne3 = new Function1[Int, Int] {
      def apply(x: Int): Int = x + 1
    }
    println("sumOne1(3) = " + sumOne1(3))
    println("sumOne2(3) = " + sumOne2(3))
    println("sumOne3(3) = " + sumOne3(3))
  }
}
```

### 柯里化curry

```
def add(x:Int,y:Int)=x+y
```

转为

```
def add(x:Int)(y:Int) = x + y
```

本质：接收一个x为参数，返回一个匿名函数，该匿名函数的定义是：接收一个Int型参数y，函数体为x+y

```
//js版更清晰
var add = function(x) {
  return function(y) {
    return x + y;
  };
};
//scala
var add = new Function1[Int, Int] {
      def apply(x: Int): Int = x + y
    }
```

好处：可以传入部分参数而得到一个新函数

```
val x = 1000
def addX = add(x)//产生了新的函数
val result = addX(1)//使用新函数进行计算
```