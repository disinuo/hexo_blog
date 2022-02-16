---
title: 分布式系统介绍
date: 2021-3-9
tags:
---

漫谈分布式系统
https://zhuanlan.zhihu.com/p/141775358

我有个习惯，学习一个新知识的时候，除了搞懂 What -- 这个东西是什么怎么用，还会特别关注甚至是最先关注 Why -- 为什么会有这个东西，为什么我要知道它甚至用它。然后如果有需要，再去关注 How -- 它是怎么实现的，有什么值得借鉴的地方。

为什么会有分布式？
两点：算得慢（算力不足），存不下（数据分散）。

无论是线程、进程，还是协程，本质上，目的都是为了计算的并行化，解决的是算的慢的问题。
因为一个CPU算不过来，而单机扩容CPU数量成本极高（有空间这个刚性限制），所以需要水平扩容，多搞几台机器。

计算的分布式化：基于分治思想，出现了分布式框架。
存储的分布式化：分布式存储引擎，主要解决数据如何拆分、数据如何冗余、数据如何定位的问题。

分布式文件系统
按照Block拆分文件，然后通过一个管理员来管理Block到对应及其的映射关系。

新的问题：
如果存储block的机器挂了怎么办？ 如果管理员挂了怎么办？

高效存储：

1. 很长时间没有人访问的老数据，怎么办？
2. 是不是减少副本的同时，还能还原出数据？
3. 数据压缩，平成好压缩效果与压缩/解压速度
4. 数据的分层存储：根据数据的冷热来选择不同的存储介质（局部性原理），内存、硬盘（SSD、SATA)
5. 存储与计算分离（底层基础：网络IO不再是瓶颈，而CPU性能、磁盘IO性能跟不上）

MapReduce: 分布式计算的总体思想。

分布式计算框架更多尝试对资源的调度做优化。ResourceManager, yarn, k8s;
（多租户，隔离，软隔离，租借，抢占等）

parititioning 方式，即数据在逻辑上怎么切分

localization 方式，即数据在物理上怎么分布

rebalance 方式，即在节点数变化后，数据在物理上怎么重新分布.

一个系统要想拥有高可用性，有且只有一个办法 -- 副本（replication）。

这篇文章，我们大致看了分布式系统面临的另一个核心问题 -- 可用性。

高可用的唯一出路 -- 副本（replication），因为物理故障是无法避免的，只能花钱做备份。

主从和时效性是 replication 要考虑的两个重要问题。

主从主要有单主、多主和无主三种方式。

时效性主要有同步、异步、半同步三种方式。