---
layout: post
title: 大数据服务接口——语义化
categories: [blog]
tags: [Java]
description: 
---

# 大数据服务接口——语义化

* TOC
{:toc}
自8月开始的中后台数据服务平台的开发，发现在接口服务方面，为了应对复杂的查询筛选，我将接口以语义的形式进行抽象。什么意思呢，我并没有在接口中定义一套DSL反而利用Java的静态特性去将枚举、成员文档化，使得接口使用方可以清晰的组织语义进行查询。

### 场景一：

#### 产品：

获取某个组织下，近7天的S档且为新店的店铺列表，按照店铺综合得分降序排序。

#### 结构为：

[某组织] + [近7天] + [S档] + [新店]   & sort by  [店铺综合得分] + [是否降序]

#### 抽象为：

[节点] ：节点类型+节点ID

[筛选条件] ：近七天 + S档 + 新店 +……   等筛选条件的交集

[排序方法]： number排序字段 + 是否降序

#### 接口：

```java
    List<ShopAnalysis> getSortShopAnalysis(Unit unit, FilterQuery filterQuery, ShopSortCondition shopSortCondition, int offset, int limit);
```



### 场景二：

#### 产品：

获取某个组织下，近7天的SAB档且为独家新店且订单数介于[1000,1200)之间的店铺数。

#### 结构为：

[某组织] + [近7天] + [SAB档] + [独家] + [新店] + [指定订单数]

#### 抽象为：

[节点] ：节点类型+节点ID

[筛选条件] ：[某组织] + [近7天] + [SAB档] + [独家] + [新店] + [指定订单数]  等筛选条件的交集

### 接口：

在语言层面，将筛选字段分为3种类型：boolean、number、enum(string)三种类型，这样针对不同类型进行包装即可。

```java
    int getShopAnalysisCount(Unit unit, ShopCountCondition shopCountCondition) throws ServiceException;
```

```java
/**店铺维度count聚合条件*/
public class ShopCountCondition {
    /**店铺boolean类型标签列表，取列表所有元素条件的交集，有则传无则不传*/
    List<ShopBooleanColumn> shopBooleanColumns;

    /**店铺Number类型标签列表，取列表所有元素条件的交集，有则传无则不传*/
    List<ShopNumberColumn> shopNumberColumns;

    /**枚举类型——店铺档次，档次列表元素之间取并集，与其他成员之间取交集*/
    List<ShopLevelType> shopLevelTypes;
    
    /**布尔类型店铺维度标签条件*/
    public static class ShopBooleanColumn{
        /**布尔类型店铺维度标签*/
        ShopBooleanColumnType column;

        /**店铺维度标签的值，默认false*/
        boolean value;
    }

    /**number类型店铺维度标签条件*/
    public static class ShopNumberColumn{
        /**number类型店铺维度标签*/
        ShopNumberColumnType column;

        /**店铺维度标签的最小值，如果没有则必须为null*/
        Number min;

        /**店铺维度标签的最大值，如果没有则必须为null*/
        Number max;
    }
}
```



## 语义化好处

语义化后接口提供更强的业务支撑能力，同时无需频繁修改代码，加查询字段也不需要改接口。

