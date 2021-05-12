---
title: 互联网面试题解析|头条面试题汇总
date: 2021-1-11 11:47:03
mathjax: true
categories:
tags: 
---

## 操作系统

* 操作系统为什么要分内核态和用户态

* 进程内存空间布局如何

* 为什么要有page cache，操作系统怎么设计的page cache

### 进程与线程

* 进程线程的区别
  （这个问得比较细，不只是简单的内存共享之类的，还包括操作系统的进程描述，写时复制，操作系统启动等问题，推荐《深入理解计算机系统》）

* 进程间通信方式

* 线程的状态
  
* 介绍死锁和如何避免

### API实现原理

* malloc函数，你所知道的一切。（只记得一点点了）

### Linux系统命令

* Linux命令：查看端口的的命令（此处忘记耍帅，<(‵^′)>），查看cpu/內存, 文件权限管理(chmod)

## C++语言

* C++11新特性，用到哪些C++的新特性

* malloc和new的区别

## STL基础

STL里resize和reserve的区别

STL内存分配

## 计算机网络

* 网络模型以及各层协议

* 长链接 VS 短链接

长连接：
长连接多用于操作频繁，点对点的通讯，而且连接数不能太多的情况。
每个TCP连接的建立都需要三次握手，每个TCP连接的断开要四次握手。
如果每次操作都要建立连接然后再操作的话处理速度会降低，所以每次操作后，下次操作时直接发送数据就可以了，不用再建立TCP连接。例如：数据库的连接用长连接，如果用短连接频繁的通信会造成socket错误，频繁的socket创建也是对资源的浪费。

短连接：
web网站的http服务一般都用短连接。因为长连接对于服务器来说要耗费一定的资源。像web网站这么频繁的成千上万甚至上亿客户端的连接用短连接更省一些资源。试想如果都用长连接，而且同时用成千上万的用户，每个用户都占有一个连接的话，可想而知服务器的压力有多大。所以并发量大，但是每个用户又不需频繁操作的情况下需要短连接。总之：长连接和短连接的选择要根据需求而定。

* TCP与UDP的区别

TCP/UDP的区别,介绍区别顺带说了使用场景,最后补刀TCP头20字节,UDP头8字节,然后问TCP头结构(自己坑自己)

基于连接与无连接
TCP要求系统资源较多，UDP较少；
UDP程序结构较简单
流模式（TCP）与数据报模式(UDP);
TCP保证数据正确性，UDP可能丢包
TCP保证数据顺序，UDP不保证
· TCP与UDP分别应用在什么方面

TCP:对效率要求低，对准确性要求较高 (如文件传输、重要状态的更新等)
UDP:对效率要求高，对准确性要求较低 (如视频传输、实时通信等)。
· TCP和UDP分别对应协议(应用层)

TCP: STMP, TELNET, HTTP, FTP
UDP: DNS,TFTP,RIP,DHCP,SNMP

* TCP协议细节

tcp慢启动过程
TCP拥塞控制
TCP三次握手四次挥手
TCP三次握手的缺陷

4.TCP如何保证可靠性. 把三次握手很详细地说了一遍,发送什么码进入什么状态..

5.除了三次握手还有么?滑动窗口,1比特等待协议,回退N针协议,选择性重传协议说了一边

6.还有么?不用说的那么详细,时间不太多了 说了 快重传 快恢复 超时重传,然后实在不知道了

SYN- Flood攻击: 通过向网络服务所在端口发送大量的伪造源地址的攻击报文，就可能造成目标服务器中的半开连接队列被占满，从而阻止其他合法用户进行访问。
防范:
无效连接监视释放 这种方法不停的监视系统中半开连接和不活动连接，当达到一定阈值时拆除这些连接，释放系统资源。这种绝对公平的方法往往也会将正常的连接的请求也会被释放掉，”伤敌一千，自损八百“
延缓TCB分配方法
使用SYN 2Proxy防火墙

* IO复用以及select，poll，epoll区别

  select/epoll了解不.答了解,但不知道对不对,希望不要介意,说了select用户态到内核态,epoll只负责活跃的,不管那些不活跃的
  同步和异步的优缺点

* 简单介绍Nginx是如果工作的
  答了masterwork之类的,然后没话可说了赶紧补充一些Apache的prefork时怎么工作的做了一下对比

## HTTP协议相关

输入url，到浏览器显示的过程

get,post的区别

http请求的组成

http和https的区别（没有答的很好）Https，非对称加密等

了解http 2.0么？多路复用在非IO阻塞中有没有用到？

常见的网络攻击方法，比如跨站脚本，sql注入之类的

## 数据库

左连接、右连接

索引、主键的区别

乐观锁和悲观锁

乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读，发生冲突的概率低的应用类型，这样可以提高吞吐量。
悲观锁(Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。悲观锁适用于可靠的持续性连接，诸如C/S应用。对于Web应用的HTTP连接，先天不适用。

## 多线程编程

介绍5种IO模型

异步编程的事件循环

· 进程同步的方式

临界区:通过对多线程的串行化来访问公共资源或一段代码，速度快，适合控制数据访问。
互斥量:为协调共同对一个共享资源的单独访问而设计的。
信号量:为控制一个具有有限数量用户资源而设计。
事 件:用来通知线程有一些事件已发生，从而启动后继任务的开始。

· 自旋锁是什么样的

对于互斥锁，如果资源已经被占用，资源申请者只能进入睡眠状态。但是自旋锁不会引起调用者睡眠，如果自旋锁已经被别的执行单元保持，调用者就一直循环在那里看是否该自旋锁的保持者已经释放了锁。
如果等待的时间比较短，适合使用自旋锁
占用大量的CPU资源

线程池的原理

· 线程池的结构与如何实现同步

线程池管理器(ThreadPool):用于创建并管理线程池，包括 创建线程池，销毁线程池，添加新任务；
工作线程(PoolWorker):线程池中线程，在没有任务时处于等待状态，可以循环的执行任务；
任务接口(Task):每个任务必须实现的接口，以供工作线程调度任务的执行，它主要规定了任务的入口，任务执行完后的收尾工作，任务的执行状态等；
任务队列(taskQueue):用于存放没有处理的任务。提供一种缓冲机制。

· 同步和异步的优缺点

同步和异步的特点
异步传输是面向字符的传输，而同步传输是面向比特的传输。
异步传输的单位是字符而同步传输的单位是帧。
异步传输通过字符起止的开始和停止码抓住再同步的机会，而同步传输则是以数据中抽取同步信息。
异步传输对时序的要求较低，同步传输往往通过特定的时钟线路协调时序。
异步传输相对于同步传输效率较低。
同步的好处：
同步流程对结果处理通常更为简单，可以就近处理。
同步流程对结果的处理始终和前文保持在一个上下文内。
同步流程可以很容易捕获、处理异常。
同步流程是最天然的控制过程顺序执行的方式。
异步的好处：
异步流程可以立即给调用方返回初步的结果。
异步流程可以延迟给调用方最终的结果数据，在此期间可以做更多额外的工作，例如结果记录等等。
异步流程在执行的过程中，可以释放占用的线程等资源，避免阻塞，等到结果产生再重新获取线程处理。
异步流程可以等多次调用的结果出来后，再统一返回一次结果集合，提高响应效率。

多线程中有哪些锁

阻塞和非阻塞

怎么实现线程池
怎么唤醒，调度线程

写代码：多线程实现从A,B,C三个文件中读取文件放到D文件中，优化：如何同步实现

## 算法数据结构

### 数组

二分查找，查第一个出现的位置
无重复元素的二分查找
含重复元素的二分查找
有一个排好序的数组，先将其随机循环右移，求在数组中查找指定target的位置 lgn
撸一个std::lower_bound，不断优化，直到最坏复杂度也为O(logN)

建一个数组循环左移N位，最好不要用辅助空间（我最后还是用了= =）

数组：找第k大的数 （无序数组） O(N) -> 数组无法修改，额外空间O(1) 时间O(N)

求两个有序数组前K大的数(每次取K/2个元素)
拓展：求m个有序数组前K大的数 (维护一个大小为m的堆)

算法题2：给定k个数组，每个数组都是有序的，且每个数组最大值-最小值<1000，1 < k<1000，求所有数的中位数。回答思路

归并排序

n个整数的无序数组，找到每个元素后面比它大的第一个数，要求时间复杂度为O(N)

数组去重（写出一种远远不够，3种方式）

### 链表

复杂指针的复制

LRU cache

单链表旋转

算法题：链表成环判断

算法题：链表交叉判断

合并k个链表

### 字符串

找出字符数组中只出现三次，且最早出现完三次的字符（eg：aabcbba输出b） hashmap

有字符串,将所有连续的ac跟单独的b去掉后的字符串：如acccccb->ccc; aacceacdb->ed
    时间复杂度O(n) 空间复杂度O(n) --> 时间复杂度O(n) 空间复杂度O(1)

### 二叉树

实现一个二叉树的持久化方案，可以伪代码，必须用指针

算法题：二叉树层级遍历，以及follow up是每下一层遍历方向颠倒

算法题：二叉树最小公共祖先

一棵树里最远的两个节点间的距离

### 回溯

按照字典序打印1～N的排序

一棵树上路径和为固定值的那些路径

压缩驼峰字符串

### 其他数据结构

图的邻接矩阵和邻接表的表示，邻接表的数据结构

两个栈实现队列

什么是LRU缓存，怎么设计的LRU缓存，详细

给一个数组返回最小堆

实现hashmap，怎么扩容，怎么处理数据冲突？怎么高效率的实现数据迁移？

给定n，将1,2，，n按字典序排列，求第k大的数

洗牌算法

hadoop已知每一个点的neighbor，求每一个点的二度邻居

### 动态规划

数组的最大连续子集（和最大的连续子集）

求解一个矩阵中找一条最长的递增路径？好像是用DP做，个人用有向图DFS和记忆化搜索处理

求解一个区间的和乘以这个区间最小值的最大值？单调栈，个人很久没有刷题了，这道题复杂度用的比较大
    单调栈 + 前缀数组

## 系统设计

分布式log系统

一个秒杀系统的架构，包括CDN，反向代理，session共享，由于分布式数据库我不熟，所以数据一致性那部分没有解决。

设计一个带有有效时间TTL的KV存储系统，包含set（key，value，ttl）、get（key）方法、怎么优化

## 脑筋急转弯

脑筋急转弯，1000桶牛奶，1桶有毒，用10只小白鼠试出来。其实就是二进制，网上有答案，提示：2^10 = 1024