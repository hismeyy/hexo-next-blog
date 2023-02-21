---
title: 5_Redis在Jedis中如何使用
date: 2023-1-6
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、前言

**Jedis是Redis官方推荐的Java连接开发工具**，虽然现在的SpringBoot2.×版本已经将Jedis换成了Lettuce，但是还是有必要了解一下Jedis的使用

# 二、Jedis连接Redis数据库

1. 创建一个maven空项目

2. 导入依赖

   ```xml
   <!--导入jedis的包-->
   <dependency>
       <groupId>redis.clients</groupId>
       <artifactId>jedis</artifactId>
       <version>3.2.0</version>
   </dependency>
   <!--fastjson-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>fastjson</artifactId>
       <version>1.2.67_noneautotype2</version>
   </dependency>
   ```

3. 连接Redis测试

   ```java
   // 1 创建对象
   Jedis jedis = new Jedis("127.0.0.1", 6379);
   // 2 输入指令 和之前命令完全一致
   System.out.println(jedis.keys("*"));
   ```

   