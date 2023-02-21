---
title: 9_Redis实现发布订阅功能
date: 2023-1-10
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、前言

**Redis发布订阅（pub/sub）是一种消息通信模式：发送者（pub）发送消息，订阅者（sub）接受消息**

Redis客户端可以订阅任意数量的频道

# 二、实现方式

1. 命令

   这些命令被广泛用于构建即时通信应用，比如网络聊天室(chatroom)和实时广播、实时提醒

   ![image-20230207104742030](C:\Users\任宇恒\Desktop\新建文件夹\9_Redis实现发布订阅功能.assets\image-20230207104742030.png)

2. 发布订阅的实现

   - 订阅端

     ```cmd
     127.0.0.1:6379> ping
     PONG
     127.0.0.1:6379> SUBSCRIBE dingdada  #订阅名字为 dingdada 的频道
     Reading messages... (press Ctrl-C to quit)
     1) "subscribe"
     2) "dingdada"
     3) (integer) 1
     #等待推送的信息
     1) "message"  #消息
     2) "dingdada"  #来自哪个频道的消息
     3) "hello world\xef\xbc\x81"  # 消息的具体内容
     1) "message"
     2) "dingdada"
     3) "my name is dyj\x81"
     ```

   - 发送端

     ```cmd
     127.0.0.1:6379> ping
     PONG
     127.0.0.1:6379> PUBLISH dingdada "hello world！"  #发送消息到dingdada 频道
     (integer) 1
     127.0.0.1:6379> PUBLISH dingdada "my name is dyj"  #发送消息到dingdada 频道
     (integer) 1
     ```

3. PSUBSCRIBE 命令：订阅指定频道

   ```cmd
   PSUBSCRIBE + 频道.. #订阅给定的模式，可多个
   ```

4. PUBLISH 命令：发送消息至指定频道

   ```
   PUBLISH + 频道 +消息  #将信息 message 发送到指定的频道 channel
   ```

5. PUNSUBSCRIBE命令：退订

   ```
   #指示客户端退订指定模式，若果没有提供模式则退出所有模式
   ```

6. SUBSCRIBE：订阅

7. UNSUBSCRIBE：退订

# 三、总结

Pub/Sub 从字面上理解就是发布（Publish）与订阅（Subscribe），在Redis中，你可以设定对某一个key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能