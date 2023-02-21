---
title: 2_Redis的五大数据类型
date: 2023-1-2
description: 2023年初。我开始学习Redis！开始了NoSQL的学习！
tags:
  - 学习
categories: Redis基础
---

# 一、String（字符串）

1. **添加**、**查询**、**追加**、**获取长度**，**判断是否存在**的操作

   | 命令             | 意义           |
   | ---------------- | -------------- |
   | set key value    | 添加数据       |
   | get key          | 查询数据       |
   | keys *           | 查询所有数据   |
   | exists key       | 判断是否存在   |
   | append key value | 追加           |
   | strlen key       | 查看字符串长度 |

2. **自增**、**自减**操作

   | 命令              | 意义                    |
   | ----------------- | ----------------------- |
   | incr key          | 指定key的数据自增1      |
   | decr key          | 指定key的数据自减1      |
   | incrby key number | 指定key的数据自增number |
   | decrby key number | 指定key的数据自减number |

3. **截取**、**替换**字符串操作

   | 命令                   | 意义                       |
   | ---------------------- | -------------------------- |
   | getrange key 开始 结束 | 截取字符串，不会改变源数据 |
   | setrange key 开始 值   | 替换字符串，不会改变源数据 |

4. **设置过期时间**、**不存在设置**操作

   | 命令                 | 意义                                         |
   | -------------------- | -------------------------------------------- |
   | setex key time value | 存入数据时设置过期时间                       |
   | ttl key              | 查看过期时间                                 |
   | setnx key value      | 如果key为value的值不存在就新增，存在就不新增 |

5. **mset**、**mget**操作

   | 命令   | 意义                                     |
   | ------ | ---------------------------------------- |
   | mset   | 插入多条数据                             |
   | mget   | 查询多条数据                             |
   | msetnx | 是一个原子性操作，要么全成功，要么全失败 |

6. **添加获取对象**、**getset**操作

   | 命令        | 意义      |
   | ----------- | --------- |
   | get key     | 获取数据  |
   | set key val | 新增k数据 |

> 表面上它是字符串，但其实可以灵活的表示字符串、整数、浮点数3种值。Redis会自动的识别这3种值

# 二、**List**（列表）

1. lpush（左插入）、lrange（查询集合）、rpush（右插入）操作

   | 命令                 | 意义                 |
   | -------------------- | -------------------- |
   | lpush key value      | 新增一个集合(左插入) |
   | lrange key 开始 结束 | 查询集合             |
   | rpush key value      | 新增一个集合(右插入) |

2. lpop（左移除）、rpop（右移除）操作

   | 命令     | 意义       |
   | -------- | ---------- |
   | lpop key | 从头部移除 |
   | rpop key | 从尾部移除 |

3. lindex（查询指定下标元素）、llen（获取集合长度） 操作

   | 命令            | 意义             |
   | --------------- | ---------------- |
   | lindex key 下标 | 获取指定下标元素 |
   | llen key        | 获取指定集合长度 |

4. lrem（根据value移除指定的值）

   | 命令                 | 意义                       |
   | -------------------- | -------------------------- |
   | lrem key count value | 移除集合中count个元素value |

5. ltrim（截取元素）、rpoplpush（移除指定集合中最后一个元素到一个新的集合中）操作

   | 命令                    | 意义                             |
   | ----------------------- | -------------------------------- |
   | ltrim key 开始 结束     | 通过下标截取指定长度，集合改变   |
   | rpoplpush oleKey newKey | 移除oldKey中的元素到新的newKey中 |

6. lset（更新）、linsert操作

   | 命令                              | 意义                  |
   | --------------------------------- | --------------------- |
   | lset key 下标 新值                | 更新                  |
   | linsert key after value newValue  | 在value后插入newValue |
   | linsert key before value newValue | 在value前插入newValue |

7. 小结

   > 实际上是一个链表，before Node after ， left，right 都可以插入值
   > 如果key 不存在，创建新的链表
   > 如果key存在，新增内容
   > 如果移除了所有值，空链表，也代表不存在
   > 在两边插入或者改动值，效率最高！ 中间元素，相对来说效率会低一点
   > 消息排队，消息队列 （Lpush Rpop）， 栈（ Lpush Lpop）

# 三、**Set**（集合）

> 元素唯一不重复

1. sadd（添加）、smembers（查看所有元素）、sismember（判断是否存在）、scard（查看长度）、srem（移除指定元素）操作

   | 命令              | 意义            |
   | ----------------- | --------------- |
   | sadd key val      | 添加            |
   | smembers key      | 查看所有元素    |
   | sismember key val | 判断val是否存在 |
   | scard key         | 查看集合长度    |
   | srem key val      | 移除指定元素    |

2. srandmember（抽随机）操作

   | 命令                  | 意义                |
   | --------------------- | ------------------- |
   | srandmember key count | 随机抽取count个元素 |

3. srandmember（抽随机）操作

   | 命令                 | 意义                |
   | -------------------- | ------------------- |
   | spop key count       | 随机删除count个元素 |
   | smove key1 key2  val | 移除指定元素到key2  |

4. sdiff（差集）、sinter（交集）、sunion（并集）操作

   | 命令             | 意义 |
   | ---------------- | ---- |
   | sdiff key1 key2  | 差集 |
   | sinter key1 key2 | 交集 |
   | sunion key1 key2 | 并集 |

5. 小结

   > 可实现共同好友、共同关注等需求

# 四、**Hash**（哈希）

1. hset（添加hash）、hget（查询）、hgetall（查询所有）、hdel（删除hash中指定的值）、hlen（获取hash的长度）、hexists（判断key是否存在）操作

   | 命令                | 意义                                      |
   | ------------------- | ----------------------------------------- |
   | hset key  field val | 添加hash                                  |
   | hget  key  field    | 获取hash                                  |
   | hgetall key         | 获取所有的hash中的值包含key               |
   | hdel key filed      | 删除指定的filed，filed删除后val也会被删除 |
   | hlen key            | 获取指定hash的长度                        |
   | hexists key filed   | 判断key是否存在于指定的hash               |

2. hkeys（获取所有key）、hvals（获取所有value）、hincrby（给值加增量）、hsetnx（存在不添加）操作

   | 命令                     | 意义                        |
   | ------------------------ | --------------------------- |
   | hkeys key                | 获取所有的filed             |
   | hvals key                | 获取所有的val               |
   | hincrby key filed number | 让filed的值自增或自减number |
   | hsetnx key filed val     | 不存在就新增                |

3. 小结

   > 比String更适合存储对象

# 五、**zSet**（有序集合）

1. zadd（添加）、zrange（查询）、zrangebyscore（排序小-大）、zrevrange（排序大-小）、zrangebyscore withscores（查询所有值包含key）操作

   | 命令                                   | 意义              |
   | -------------------------------------- | ----------------- |
   | zadd key 1 val 2 val                   | 添加              |
   | zrange key 开始 结束                   | 查询              |
   | zrangebyscore key -inf +inf            | 排序 小到大       |
   | zrevrange myzset 1 -1                  | 排序 大到小       |
   | zrangebyscore key -inf +inf withscores | 查询所有值包含key |

2. zrem（移除元素）、zcard（查看元素个数）、zcount（查询指定区间内的元素个数）操作

   | 命令           | 意义                     |
   | -------------- | ------------------------ |
   | zrem key val   | 移除元素                 |
   | zcard key      | 查看元素个数             |
   | zcount key 0 2 | 查询指定区间内的元素个数 |

3. 小结

   >  成绩表排序，工资表排序，年龄排序等需求可以用zset来实现