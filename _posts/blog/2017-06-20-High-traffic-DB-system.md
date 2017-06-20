---
layout: post
title: 如何用DB抗住大量读写的业务
categories: blog
excerpt: 用Mysql的分库分表来抗住大量读写的业务
tags: [Mysql,分布式]
---

## 前言

系统如何抗住大并发量、快速增长的业务，看起来是个很大的问题，也是个选择很多的问题，然而各自的选择有各自的坑。

我们这里不考虑类似于Redis这种NoSQL的case，只考虑有事务的高并发读写场景，主要的思路和相关的考虑事项。

## 基本思路

读并发量高挂从库，写并发量高水平切分主库。

首先是读并发量高挂从库。读的场景往往容易首先考虑Redis这种场景，但对于事务型应用，DB落库以后的Redis更新本身就引入了另一个问题。挂从库是解决读压力的最好的解决方案，在事务性场景下，几乎没有之一。Mysql的主从机制是基于binlog事实，因此主从之间已经处理好了一致性和时序的问题。

从库的主要问题是会有短暂延迟，因此很多实时性要求极高的服务，可以强制读主库，其余业务读从库。这种方案的可扩展性极高，因此从库上还可以挂从库。事务型应用达到读从库瓶颈可能性极小，只要考虑从库延迟和读量的权衡即可。

其次是写并发量高的业务水平切分主库。事务型场景下，写的业务更容易达到瓶颈。这里瓶颈的点不同业务可能不同，如果PHP的业务很可能是连接数，对于写入量大的可能是磁盘I/O，对于高并发插入和更新，可能是锁资源问题。解决方案就是水平切分主库（例如把id模2为0的放在奇数分库，为1的放在偶数分库），水平分库后，依赖的资源基本上会等比例下降。

水平分库的主要问题是垮库事务、垮库联查等问题。主库水平切分，对应的从库也会拆分，这就会带来经典的分库查询和排序问题。垮库事务往往需要在系统设计之初规避，从需求上规避技术上很难解决的问题。尽量规避实时性要求高的垮库联查直接查询线上事务数据库，而通过牺牲实时性，查询离线数据集群（例如Hive）的方案。

## 技术的选择

存储系统迅速发展，导致市面上的选择比较多，关系型数据库是事务型业务的不二之选、对于很多读写量很大的业务，仍然可以通过关系型数据库来做，而不是盲目选择例如文档型数据库这类系统。

对于技术方案的选择而言，团队技术栈和熟悉度也是很重要的指标。