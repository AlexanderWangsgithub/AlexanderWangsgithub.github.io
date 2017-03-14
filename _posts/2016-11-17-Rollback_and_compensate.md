---
layout: post
title: 分布式事务——回退与补偿
categories: [blog]
tags: [Architecture]
description: 
---

# 分布式事务——回退与补偿

* TOC
{:toc}

## 回退

为了保证多个operation在一个事务里执行，可以通过记录起始状态实现。

事务D包含{a, b, c},其中a、b、c分别属于不同服务的写操作。

伪代码：

```java
do(){ 
	a_before_data = get(a);
	a_after_data = operate(a);

	b_before_data = get(b);
	try{
		b_after_data = operate(b);
	}catch(Execption){
		rollback(a_before_data);
	}

	try{
		c_after_data = operate(c);
	}catch(Execption){
		rollback(a_before_data);
		rollback(b_before_data);
	}
}
```

这种需要服务方提供update接口，然后update回退，一般来说这是相对容易的。



## 补偿

事实上我们这种也用的多，失败量是少数的，不回退只记录，然后再补偿。

事务D包含{a, b, c},其中a、b、c分别属于不同服务的写操作。

伪代码：

```
do(){ 
	try{
		a_before_data = get(a);
		a_after_data = operate(a);
		b_before_data = get(b);
		b_after_data = operate(b);
		c_before_data = get(c);
		c_after_data = operate(c);
	}catch(Execption){
		record(a_before_data, a_after_data, b_before_data,
			b_after_data, c_before_data, c_after_data);
	}
}

monitor(){
	while(record().getFirst().exist()){
		doCompensate(record().getFirst());
	}
}
```

