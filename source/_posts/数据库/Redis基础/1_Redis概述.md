---
title: 1_Redis的概述
date: 2023-1-1
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、什么是Redis

Redis（Remote Dictionary Server )，即远程字典服务 !
是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

Redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。
免费和开源，是当下最热门的 NoSQL 技术之一，也被人们称之为结构化数据库。

# 二、Redis可以干什么

1. 内存存储、持久化，内存中是断电即失、所以说持久化很重要（RDB，AOF）
2. 效率高，可以用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器（浏览量）

# 三、Redis的基本了解

> Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

# 四、命令

> 命令中心：[Redis命令中心](http://www.redis.cn/commands.html)

常用命令介绍

| 命令            | 意义                                    |
| --------------- | --------------------------------------- |
| ping            | 查看当前连接是否正常，正常返回PONG      |
| clear           | 清楚当前控制台                          |
| keys *          | 查看当前库里所有的key                   |
| set key value   | 添加一个key-value                       |
| get key         | 查询key对应的值                         |
| exists key      | 判断key是否存在                         |
| move key 1      | 移除当前库1的key的数据                  |
| flushall        | 清空所有库内容                          |
| expire key time | 设置key的过期时间为time单位为s          |
| ttl key         | 查看key的生命周期时间，返回-2说明已过期 |
| type key        | 查看类型                                |

