---
title: 11_Redis集群之哨兵模式
date: 2023-1-12
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、前言

>在Redis集群说，主机断开后，得手动设置另一个从机变成主机！这是不智能的！在实际工作中，都是用哨兵模式来自动切换主机

# 二、概述

> 主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑 **哨兵模式** 。Redis从2.8开始正式提供了Sentinel哨兵） 架构来解决这个问题

**谋朝篡位 的自动版**，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库。
哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的 进程 ，作为进程，它会独立运行。其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例

# 三、配置哨兵

1. 添加哨兵配置文件 sentinel.conf

   ```CMD
   # sentinel monitor 被监控的名称 host port 1 （代表自动投票选举大哥！）
   sentinel monitor myredis 127.0.0.1 6379 1
   ```

2. 启动哨兵

   ```cmd
   redis-sentinel dyjConfig/sentinel.conf   #和启动Redis一致
   ```

3. 前提条件

   开启一台主机，两台从机，一主二从是最基本的

# 四、总结

1. 优点

   1. 哨兵集群，**基于主从复制模式** ，所有的主从配置优点，它全有
   2. 主从可以切换，**故障可以转移** ，系统的 **可用性** 就会更好
   3. 哨兵模式就是主从模式的升级，手动到自动，更加**健壮**

2. 缺点

   1. Redis **不好在线扩容** 的，集群容量一旦到达上限，在线扩容就十分麻烦
   2. 实现哨兵模式的配置其实是很 **麻烦** 的，里面有很多选择

3. 注意：除了监控各个服务器外，哨兵之间也可以相互监控

4. 哨兵配置文件解析

   ```cmd
   # Example sentinel.conf 
   
   # 哨兵sentinel实例运行的端口 默认26379 
   port 26379 
   
   # 哨兵sentinel的工作目录 
   dir /tmp 
   
   # 哨兵sentinel监控的redis主节点的 ip port 
   # master-name 可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。 
   # quorum 配置多少个sentinel哨兵统一认为master主节点失联 那么这时客观上认为主节点失联了 
   # sentinel monitor <master-name> <ip> <redis-port> <quorum> sentinel monitor mymaster 127.0.0.1 6379 2 
   
   # 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供 密码
   # 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码 
   # sentinel auth-pass <master-name> <password> 
   sentinel auth-pass mymaster MySUPER--secret-0123passw0rd 
   
   # 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒 
   # sentinel down-after-milliseconds <master-name> <milliseconds> 
   sentinel down-after-milliseconds mymaster 30000 
   
   # 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步
   #这个数字越小，完成failover所需的时间就越长，
   # 但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。 
   #可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。 
   # sentinel parallel-syncs <master-name> <numslaves> 
   sentinel parallel-syncs mymaster 1 
   
   
   # 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
   #1. 同一个sentinel对同一个master两次failover之间的间隔时间。 
   #2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那 里同步数据时。 
   #3.当想要取消一个正在进行的failover所需要的时间。 
   #4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时， slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了 
   # 默认三分钟 # sentinel failover-timeout <master-name> 
   sentinel failover-timeout mymaster 180000 
   
   
   # SCRIPTS EXECUTION #配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知 相关人员。 
   #对于脚本的运行结果有以下规则： 
   #若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10 #若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。 
   #如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。 
   #一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。 
   #通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等）， 将会去调用这个脚本，这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信 息。调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。如果sentinel.conf配 置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无 法正常启动成功。 
   #通知脚本 
   # shell编程 
   # sentinel notification-script <master-name> <script-path> 
   sentinel notification-script mymaster /var/redis/notify.sh 
   
   
   # 客户端重新配置主节点参数脚本 
   # 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已 经发生改变的信息。 
   # 以下参数将会在调用脚本时传给脚本: 
   # <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port> 
   # 目前<state>总是“failover”, 
   # <role>是“leader”或者“observer”中的一个。 
   # 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通 信的
   # 这个脚本应该是通用的，能被多次调用，不是针对性的。 
   # sentinel client-reconfig-script <master-name> <script-path> 
   sentinel client-reconfig-script mymaster /var/redis/reconfig.sh 
   # 一般都是由运维来配置！
   ```

   