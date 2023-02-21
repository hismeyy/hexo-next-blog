---
title: 7_Redis的配置文件
date: 2023-1-8
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、单位

Redis配置大小写不敏感

# 二、包含INCLUDES

搭建Redis集群时，可以使用includes包含其他配置文件

# 三、网络NETWORK

```cmd
bind 127.0.0.1 # 绑定的ip 
protected-mode yes # 保护模式 
port 6379 # 端口设置
```

# 四、通用GENERAL

```cmd
daemonize yes # 以守护进程的方式运行，默认是 no，我们需要自己开启为yes！ 
pidfile /var/run/redis_6379.pid # 如果以后台的方式运行，我们就需要指定一个 pid 文件！ 
# 日志 
# Specify the server verbosity level. 
# This can be one of:
# debug (a lot of information, useful for development/testing) 
# verbose (many rarely useful info, but not a mess like the debug level) 
# notice (moderately verbose, what you want in production probably) 生产环境 
# warning (only very important / critical messages are logged)
loglevel notice 
logfile "" # 日志的文件位置名 
databases 16 # 数据库的数量，默认是 16 个数据库 
always-show-logo yes # 是否总是显示LOGO
```

# 五、快照（RDB）SNAPSHOTTING

> 持久化，在规定的时间内，执行了多少次操作则会持久化到文件 .rdb .aof文件。**Redis是内存数据库，如果没有持久化，那么数据断电即失**

```cmd
# 如果900s内，如果至少有一个1 key进行了修改，我们及进行持久化操作 
save 900 1 
# 如果300s内，如果至少10 key进行了修改，我们及进行持久化操作 
save 300 10 
# 如果60s内，如果至少10000 key进行了修改，我们及进行持久化操作 
save 60 10000 
```

# 六、安全SECURITY

> 可以在这里设置Redis的密码，默认是没有密码的

1. 通过命令设置

   ```cmd
   127.0.0.1:6379> ping
   PONG
   127.0.0.1:6379> config get requirepass  #获取Redis的密码
   1) "requirepass"
   2) ""
   127.0.0.1:6379> config set requirepass "123456"  #设置Redis的密码为123456
   OK
   # Ctrl+C 退出当前连接
   [root@dyjcomputer bin]# redis-cli -p 6379  #重新连接
   127.0.0.1:6379> ping  #测试ping，失败，所有的命令都显示无权限
   (error) NOAUTH Authentication required.  
   127.0.0.1:6379> set k1 v1  #失败，所有的命令都显示无权限
   (error) NOAUTH Authentication required.  
   127.0.0.1:6379> auth 123456  #auth + 密码  登陆上去
   OK 
   127.0.0.1:6379> ping  #正常
   PONG
   127.0.0.1:6379> config get requirepass  #获取密码，正常
   1) "requirepass"
   2) "123456"
   ```

2. 修改配置

   ```cmd
   requirepass foobared
   ```

# 七、限制CLIENTS

```cmd
maxclients 10000   #设置能连接上redis的最大客户端的数量 
maxmemory <bytes>  #redis 配置最大的内存容量 
maxmemory-policy noeviction  #内存到达上限之后的处理策略 
```

内存淘汰策略：

- no-envicition：
  该策略对于写请求不再提供服务，会直接返回错误，当然排除del等特殊操作，redis默认是no-envicition策略。（返回错误，不会删除任何键值）
- allkeys-random：
  从redis中随机选取key进行淘汰
- allkeys-lru：
  使用LRU（Least Recently Used，最近最少使用）算法，从redis中选取使用最少的key进行淘汰
- allkeys-lfu:
  使用LFU（Least Frequently Used，最不经常使用），从所有的键中选择某段时间之内使用频次最少的键值对清除
- volatile-random：
  从redis中设置过过期时间的key，进行随机淘汰
- volatile-ttl：
  从redis中选取即将过期的key，进行淘汰
- volatile-lru：
  使用LRU（Least Recently Used，最近最少使用）算法，从redis中设置过过期时间的key中，选取最少使用的进行淘汰
- volatile-lfu:
  使用LFU（Least Frequently Used，最不经常使用），从设置了过期时间的键中选择某段时间之内使用频次最小的键值对清除掉

# 八、APPEND ONLY MODE AOF配置（持久化保存）

```cmd
appendonly no  #默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下,rdb完全够用！ 
appendfilename "appendonly.aof"  #持久化的文件的名字 
# appendfsync always # 每次修改都会 sync。消耗性能 
appendfsync everysec # 每秒执行一次 sync，可能会丢失这1s的数据！ 
# appendfsync no  #不执行 sync，这个时候操作系统自己同步数据，速度最快！
```

