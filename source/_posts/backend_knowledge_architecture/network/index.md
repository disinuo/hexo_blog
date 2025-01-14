---
title: 计算机网络必知必会
date: 2022-02-16 23:20:03
updated:
mathjax: true
categories:
tags: 
---

## 长链接 VS 短链接

长连接：

长连接多用于操作频繁，点对点的通讯，而且连接数不能太多的情况。

每个TCP连接的建立都需要三次握手，每个TCP连接的断开要四次握手。

如果每次操作都要建立连接然后再操作的话处理速度会降低，所以每次操作后，下次操作时直接发送数据就可以了，不用再建立TCP连接。例如：数据库的连接用长连接，如果用短连接频繁的通信会造成socket错误，频繁的socket创建也是对资源的浪费。

短连接：

web网站的http服务一般都用短连接。因为长连接对于服务器来说要耗费一定的资源。

像web网站这么频繁的成千上万甚至上亿客户端的连接用短连接更省一些资源。试想如果都用长连接，而且同时用成千上万的用户，每个用户都占有一个连接的话，可想而知服务器的压力有多大。所以并发量大，但是每个用户又不需频繁操作的情况下需要短连接。

总之：长连接和短连接的选择要根据需求而定。

## TCP与UDP的区别

流模式（TCP）与数据报模式(UDP);

- 基于连接与无连接TCP保证数据正确性，UDP可能丢包
TCP保证数据顺序，UDP不保证

TCP要求系统资源较多，UDP较少；
UDP程序结构较简单

TCP与UDP分别应用在什么方面

TCP:对效率要求低，对准确性要求较高 (如文件传输、重要状态的更新等)

eg.  STMP, TELNET, HTTP, FTP

UDP:对效率要求高，对准确性要求较低 (如视频传输、实时通信等)。

eg.  DNS,TFTP,RIP,DHCP,SNMP
