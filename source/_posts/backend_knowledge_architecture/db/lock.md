---
title: 数据库必知必会-锁篇
date: 2022-02-16 23:20:03
updated:
mathjax: true
categories:
tags: 
---

数据库中涉及到锁的一些经常考察的知识点

 1. 悲观锁 VS 乐观锁

首先悲观锁（Pessimistic Lock）、乐观锁并不是两种锁，而是两种思想，两种用于实现并发控制的思想。

其中，悲观锁指的是对数据被外界修改持悲观态度，认为数据大概率会被他人修改，所以在我准备修改数据时，我会对该数据加锁以避免其他人对该数据进行并发访问。

而乐观锁指的是对外部修改数据持乐观态度，认为数据不会修改，所以我会直接对数据进行修改，在修改的以后再进行冲突的检查。如果修改失败了，我再决定如何去做。

悲观锁适合**写多读少**的场景，而乐观锁适合**读多写少**的场景。悲观锁一般通过数据库锁来实现，而乐观锁一般是通过CAS来实现。

 2. 意向锁是什么？ 为什么需要意向锁？

   意向锁是实现多粒度锁的一种方式，是表锁，分为意向排他锁、意向共享锁；意向锁之间是兼容的；意向锁与表级别的共享锁、拍他锁是可能互斥的；意向锁与行级的互斥锁、共享锁是兼容的；实现意向锁的目的有两个：第一，让多粒度锁共存；第二，加快是否可以加锁的判定效率。

|   | 意向共享锁  | 意向互斥锁|
|  ----  | ----  | ---- |
| 意向共享锁  | 兼容 | 兼容|
| 意向互斥锁 | 兼容|兼容|

|   | 意向共享锁  | 意向互斥锁|
|  ----  | ----  | ---- |
| 表级共享锁 | 兼容|互斥|
| 表级互斥锁 | 互斥|互斥|

|   | 意向共享锁  | 意向互斥锁|
|  ----  | ----  | ---- |
| 行级共享锁 | 兼容|兼容|
| 行级互斥锁 | 兼容|兼容|

考虑一种场景，事务A尝试修改一条数据，此时事务B需要加表锁。 如果没有意向锁，数据库系统需要怎么做？ 

 3. 共享锁、排他锁、意向共享锁、意向排他锁
 4. 行锁 VS 表锁
 5. Record lock、gap lock、next-key lock

    - record lock, 行锁， 锁直接加在索引记录上面，锁住的是key。
    - gap lock, 间隙锁，用来解决幻读问题
    - next-key lock， gap lock + reocrd lock

    关于next-key lock的两个原则、两个优化、一个bug:

    - 原则1: 加锁的基本单位是next-key lock, 前开后闭区间 
    - 原则2: 查找过程中遇到的对象才会加锁
    - 优化1: 索引上的等值查询，给唯一索引加锁的时候，next-key lock退化为行锁
    - 优化2: 索引上的等值查询，向右遍历时且最后一个值不满足等值条件时，next-key lock退化为gap lock
    - bug: 唯一索引的范围查找会访问到不满足条件的第一个值为止。

    https://blog.csdn.net/zwx900102/article/details/106544634?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control

![数据库锁](https://images.gitbook.cn/c347e4d0-a1b6-11ea-97df-0d0e3bd6b465)
