---
layout: post
title: 函数式编程
categories: [blog]
tags: [Java]
description: 
---

* TOC
{:toc}

## 理解

lambda的本质是一个匿名方法。譬如(x,y)->{return x+y}

## 注意

Lambda好用但是要注意：

- filter判空
- 对于争用资源，并行带来的压力

## 使用

### 判空filter

Optional

```java
Optional.ofNullable(ids).orElseGet(Collections::emptyList).stream()
        .distinct()
        .map(p -> getPosition(positionParams, p, function))
        .filter(p -> p != null)
        .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
```

要对每一个元素对象（p）进行判空。

```java
noticeSet.stream()
  .filter(p -> p != null)
  .filter(p -> gravityMap.get(String.valueOf(p)) != null)            
  .filter(p -> gravityMap.get(String.valueOf(p)) == gravity.getValue()).collect(Collectors.toSet());
```



### 排序sorted&比较Comparator

```java
        Comparator<SimpleNotice> byBeginDate = (s1, s2) -> s2.getBeginDate().compareTo(s1.getBeginDate());//根据begin倒序

            return allSimpleNotice.stream().filter(p -> p != null).sorted(byBeginDate).collect(Collectors.toList());

```



#### 新老写法比较

```java
Collections.sort(bpmBdEvaluates, new Comparator<BpmBdEvaluate>() {
    @Override
    public int compare(BpmBdEvaluate o1, BpmBdEvaluate o2) {
        return o1.getUpdated_at().compareTo(o2.getUpdated_at());
    }
});
return bpmBdEvaluates.get(bpmBdEvaluates.size() - 1);
```
使用Lambda
```java
Collections.sort(bpmBdEvaluates, (o1, o2) -> o1.getUpdated_at().compareTo(o2.getUpdated_at()));
```
### map

```java
oldNotice.stream().map(p -> new SimpleNotice(p.getId(), NoticeType.PERMESSAGE.getValue(),
                    p.getTitle(), p.getTitle(), p.getBeginDate(), (p.isRead() ? 1 : 0))).collect(Collectors.toSet());
```



### groupBy

```java
        List<Province> provinceList = provinceNameMap.entrySet().stream().map(p -> new Province(p.getKey(), p.getValue(), citysMap.get(p.getKey()))).collect(Collectors.toList());
```

### collector

```java
positionList.stream().collect(Collectors.toMap(DataPosition::getId, x -> x.getPosition().intValue())); positionList.stream().collect(Collectors.toMap(DataPosition::getId, x -> x.getPosition().intValue(), (key1, key2) -> {
                return key2;//如果存在重复value
```

