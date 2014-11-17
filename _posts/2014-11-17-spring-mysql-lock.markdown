---
layout: post
title:  "在mysql下的并发锁"
date:   2014-11-17 17:00:00
categories: Code
---

1. 悲观锁
	在Service层所对应函数里加@Transactional注解, 并在mybatis的SQL语句改为

	`select .... from ... where ... for update`.

	这里有个细节, 行级锁都是基于索引的，如果一条SQL语句用不到索引是不会使用行级锁的，会使用表级锁。行级锁的缺点是：由于需要请求大量的锁资源，所以速度慢，内存消耗大。

2. 乐观锁
	在表中添加一个字段version, select时将version获取, 在update的时候, 通过判断version值是否和表中的相等来判断是否发生冲突: 

	`Select * from .....`

	`Update .... Set .... version = version + 1 Where .... version = #version`


