---
layout: post
title: Spark+Hive的大数据开发
categories: [blog]
tags: [Spark]
description: 
---

# Spark+Hive的大数据开发

* TOC
{:toc}

从9月初开始进行大数据平台的开发，到25号已经完成不同维度的数据服务了。写一点spark开发的总结。

其实只需要理解两个核心概念，RDD & DataFrame。

## 弹性分布式数据集（RDD）

目前两种，一种Parallelized Collections，一种Hadoop DataSets。

### Parralelized Collections

通过SparkContext的parallelize(dataCollection)创建。

```
val dataPC = sc.parallelize(Array(1,2,3),slices)
```

需要注意参数slices，在推送数据到mysql时，在数据记录超过10M是一般需要超过10的分割数才能保证在60min内完成数据推送。原因是Spark在一个slice（数据分片）上起一个task，

### Hadoop DataSets

```
val hiveRDD = sc.textFile("data.txt",slices)
```

spark为每一个slice创建一个指定大小分片，默认64MB。

## Operation

transformation指map等转换操作，actions指reduce等聚合操作并返回结果给driver。

### transformation

类似java stream：map,filter,flatMap

union(DataSet)

对于kv数据集，还有

groupByKey(), reduceByKey():    (k,v).groupByKey()==>(k, Seq[v])

 join():     (k,v).join(k,w)==>(k,(v,w))

groupWith():    (k,v).groupWith(k,w)==>(k, Seq[v], Seq[w])

### actions

reduce(func): func两个参数，返回一个值，注意函数是分治的。

collect(): 注意收集的数据是load到spark内存的，注意不要超过driver单机内存。

take(n):   收集前n个元素返回数组，由driver程序计算

saveAsTextFile(path)： iterm.toString()转为文本

saveAsSequenceFile(path)： 针对kv数据集

foreach()

### cache

dataRDD.cache()可以将计算结果保存在分布式存储里，下次用起来很快，适合类迭代算法用。

## DataFrame

利用spark进行来自hive源的读取就是个DataFrame，DF将模式(column)和Rows数据集匹配起来，本质上就是在内存里面建了ORM对应的List<Model>，所以hive表读写可以通过DF来完成。

模式相当于ORM中的Beans，如：

```scala
val unitRDD = spark.sparkContext.parallelize(Array("1 101 'wang'","1 1181 'gang","2 1931 'subway")).map(_.split(" "))
val unitSchema = StructType(List(
      StructField("organization_id", LongType, true),
      StructField("user_id", LongType, true),
      StructField("user_name", StringType, true),
    ))
val unitRDDRow = unitRDD.map(p => Row(p(0).toLong, p(1).toLong, p(2).trim()))
val unitDF = spark.createDataFrame(unitRDDRow, unitSchema)
unitDF.createOrReplaceTempView("unit_template")//注册表
sc.sql("insert into temp.unit select * from unit_template")	
```

反之，对应：

```scala
val unitDF = sc.sql(select * from temp.unit)
```

