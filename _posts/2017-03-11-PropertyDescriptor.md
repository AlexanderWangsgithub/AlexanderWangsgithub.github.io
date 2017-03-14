---
layout: post
title: Java Bean的处理
categories: [blog]
tags: [Java]
description: 
---

Java Bean

* TOC
{:toc}

### PropertyDescriptor

针对field的读写操作

```java
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    public static class ObjA{
        int fieldA;
        String fieldB;
    }
    @Test
    public void PropertyDescriptorTest() throws IntrospectionException, InvocationTargetException, IllegalAccessException {
        ObjA objA = new ObjA(11, "before");
        PropertyDescriptor descriptor = new PropertyDescriptor("fieldB", objA.getClass());
        Method getter = descriptor.getReadMethod();//获得某个field的get方法
        Method setter = descriptor.getWriteMethod();//获得某个field的set方法
        assertEquals("before", getter.invoke(objA));
        setter.invoke(objA, "after");
        assertEquals("after", objA.getFieldB());
        assertEquals(descriptor.getPropertyType(),objA.getFieldB().getClass());//获得某个field的class
    }
```







