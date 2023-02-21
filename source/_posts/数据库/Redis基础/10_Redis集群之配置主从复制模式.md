---
title: 10_Redis集群之配置主从复制模式
date: 2023-1-11
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、前言

默认情况下，每台Redis服务器都是主节点，工作中用哨兵模式监控

**主从复制**是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower)；数据的复制是单向的，只能由主节点到从节点。Master以写为主，Slave 以读为主
主要作用：

- 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式
- 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余
- 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量
- 高可用（集群）基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础

# 二、环境配置（单机集群）

1. 基本查看命令

   ```cmd
   127.0.0.1:6379> ping  #测试是否连接成功！
   PONG
   127.0.0.1:6379> info replication  #查看当前redis信息
   # Replication
   role:master  #角色--主机
   connected_slaves:0  #从机数量为0
   master_replid:b9565cf2edea63b7e9860f3ef1a170d59ff7a4d4  #唯一标识的id
   master_replid2:0000000000000000000000000000000000000000
   #下面的这些不用管他是啥
   master_repl_offset:0
   second_repl_offset:-1
   repl_backlog_active:0
   repl_backlog_size:1048576
   repl_backlog_first_byte_offset:0
   repl_backlog_histlen:0
   ```

2. 开启三台服务

   复制三个配置文件-修改端口-修改pid名-修改log文件-修改dump.rdb

3. 全部启动并查看

4. 查看所有Redis端口是否启动成功

# 三、一主二从（单机测试）

1. 认大哥

   ```cmd
   127.0.0.1:6380> ping
   PONG
   127.0.0.1:6380> slaveof 127.0.0.1 6379  #让本机认6379的机器为大哥！
   OK
   127.0.0.1:6380> info replication  #查看信息
   # Replication
   role:slave  #从机
   master_host:127.0.0.1  #主机ip
   master_port:6379   #主机端口
   master_link_status:up
   master_last_io_seconds_ago:3
   master_sync_in_progress:0
   slave_repl_offset:14
   slave_priority:100
   slave_read_only:1
   connected_slaves:0
   ```

2. 查看主机的信息

   ```cmd
   127.0.0.1:6380> ping
   PONG
   127.0.0.1:6380> slaveof 127.0.0.1 6379  #让本机认6379的机器为大哥！
   OK
   127.0.0.1:6380> info replication  #查看信息
   # Replication
   role:slave  #从机
   master_host:127.0.0.1  #主机ip
   master_port:6379   #主机端口
   master_link_status:up
   master_last_io_seconds_ago:3
   master_sync_in_progress:0
   slave_repl_offset:14
   slave_priority:100
   slave_read_only:1
   connected_slaves:0
   ```

3. 注意

   这种通过命令的配置是‘一次性的’，如果机器宕机、断电等，就需要重新认大哥

   在实际工作中，我们都是通过配置文件中修改指定配置的

   ![image-20230207110708244](C:\Users\任宇恒\Desktop\新建文件夹\10_Redis集群之配置主从复制模式.assets\image-20230207110708244.png)

4. 复制原理

   Slave 启动成功连接到 master 后会发送一个sync同步命令

   Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，并完成一次完全同步。

   **全量复制：** 而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

   **增量复制：** Master 继续将新的所有收集到的修改命令依次传给slave，完成同步但是只要是重新连接master，一次完全同步（全量复制）将被自动执行！ 我们的数据一定可以在从机中看到。

5. 层层链路

   上一个M连接下一个S

6. 谋权篡位

   果主机断开了连接，可以使用

   ```cmd
   SLAVEOF no one
   ```

   让自己变成主机！其他的节点就可以手动连接到最新的这个主节点（手动）！如果这个时候原来的老大修复了，那就重新连接成为小弟

   **注意**：老大没挂，这个命令也可以直接让自己成为大哥

# 四、总结

一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机），原因如下：

1. 从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大；
2. 从容量上，单个Redis服务器内存容量有限，就算一Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，单台Redis最大使用内存不应该20G。
3. 主从复制，读写分离！ 80% 的情况下都是在进行读操作！减缓服务器的压力！架构中经常使用！ 一主二从！
4. 只要在公司中，主从复制就是必须要使用的，因为在真实的项目中不可能单机使用Redis