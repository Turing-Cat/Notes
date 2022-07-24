狂神Redis教学视频学习笔记，包括NoSQL介绍、Redies数据类型、Redis事务、整合SpringBoot、Redis持久化、Redis主从复制等内容

> [1] 主要框架及内容都是狂神Redis的课堂笔记 https://www.kuangstudy.com/
> 
> [2] 参考了javaguide的文章 [redis](https://snailclimb.gitee.io/javaguide/#/docs/database/Redis/redis-all?id=_1-简单介绍一下-redis-呗)

# 1 NoSQL概述

## 1.1 数据库架构的演变

> 数据库架构演进：
> 
> 1 单机MySQL的美好年代
> 
> 2 Memcached（缓存）+ MySQL + 垂直拆分（多个完整的数据库）
> 
> 3 MySQL主从读写分离 （读写分离、主从复制）
> 
> 4 分表分库 + 水平拆分 + Mysql 集群
> 
> 5 现在的架构

**1 单机MySQL的美好年代**

- 在90年代，一个网站的访问量一般不大，用单个数据库完全可以轻松应付！

![image-20210704162507809](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211332.png)

**2 Memcached（缓存）+ MySQL + 垂直拆分（多个完整的数据库）**

- Memcached缓解数据库的读取压力
- 垂直拆分：多个完整的数据库供读写，缓解压力

![image-20210704162715236](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211334.png)

**3 MySQL主从读写分离 （读写分离、主从复制）**

- 读写分离：使的大量的数据库用于读，部分用于写
- 主从复制：写数据库修改后、立马更新到读数据库，提高了读写性能和读库的可扩展性

![image-20210704162949719](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211339.png)

**4 分表分库 + 水平拆分 + Mysql 集群**

- MySQL主库的写压力开始出现瓶颈，开始流行使用分表分库来缓解写压力和数据增长的扩展问题 【重要】
- MySQL推出了MySQL Cluster集群，但性能也不能很好满足互联网的需求，只是在高可靠性上提供了非常大的保证。

![image-20210704163213199](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211342.png)

**5 现在的架构**

![image-20210704163340611](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211346.png)

**目前的困境：**

MySQL关系数据库很强大，但是它并不能很好的应付所有的应用场景，MySQL的扩展性差（需要复杂的技术来实现），大数据下IO压力大，表结构更改困难，正是当前使用MySQL的开发人员面临的问题。

比如1000万4KB大小的文本就接近40GB的大小，如果能把这些数据从MySQL省去，MySQL将变的非常的小 。这时就需要用非关系型数据库NoSQL

## 1.2 什么是NoSQL

### 1 NoSQL 概述

**NoSQL = Not Only SQL 不仅仅是SQL，泛指非关系型的数据库 。**

**Nosql特点**

1 方便扩展（数据之间没有关系，很好扩展！）

2 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）

3 数据类型是多样型的！（不需要事先设计数据库，随取随用）

**大数据时代的3V和3高b**

大数据时代的3V ：指描述问题的**海量Velume、多样Variety、实时Velocity**

大数据时代的3高 ： 指对程序的要求：**高并发、高扩展性、高性能**

### 2 Nosql的四大分类

| **分类**             | **Examples举例**                                         | **典型应用场景**                                         | **数据模型**                        | **优点**                                | **缺点**                                  |
|:------------------ |:------------------------------------------------------ |:-------------------------------------------------- |:------------------------------- |:------------------------------------- |:--------------------------------------- |
| **键值对（key-value）** | Tokyo Cabinet/Tyrant, **Redis**, Voldemort, Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。                 | Key 指向 Value 的键值对，用hash table实现 | 查找速度快                                 | 数据无结构化，通常只被当作字符串或者二进制数据                 |
| **列存储数据库**         | Cassandra, HBase, Riak                                 | 分布式的文件系统                                           | 以列簇式存储，将同一列数据存在一起               | 查找速度快，可扩展性强，更容易进行分布式扩展                | 功能相对局限                                  |
| **文档型数据库**         | CouchDB, MongoDb                                       | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） | Key-Value对应的键值对，Value为结构化数据     | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                     |
| **图形(Graph)数据库**   | Neo4J, InfoGrid, Infinite Graph                        | 社交网络，推荐系统等。专注于构建关系图谱                               | 图结构                             | 利用图结构相关算法。比如最短路径寻址，N度关系查找等            | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群 |

### 3 关系型数据库和非关系型数据库的区别【理解背】

- 关系型数据库：
  - 关系型数据库的最大特点就是事务的一致性：传统的关系型数据库读写操作都是事务的，具有ACID的特点
  - 关系型数据库为了维护一致性所付出的巨大代价就是其读写性能比较差
  - 关系数据库的另一个特点就是其具有固定的表结构，因此，其扩展性较差
- 非关系型数据库 not only SQL
  - 指非关系型的，分布式的，且一般不保证遵循ACID原则的数据存储系统
  - 面向高性能并发读写的key-value数据库
  - 面向可扩展性的分布式数据库
- 数据的持久存储，尤其是海量数据的持久存储，还是需要一种关系数据库

# 2 Redis入门

> Redis：REmote DIctionary Server（远程字典服务器）
> 
> https://redis.io/ 官网
> 
> [http://www.redis.cn](http://www.redis.cn/) 中文网

## 2.1 Redis概述

Redis 是速度非常快的非关系型（NoSQL）内存键值数据库（可以称之为内存中的数据库），可以存储键和五种不同类型的值之间的映射。

Redis 支持很多特性，**例如将内存中的数据持久化到硬盘中，使用复制来扩展读性能，使用分片来扩展写性能。**具体的功能有：将内存异步写入硬盘、发布订阅系统消息、地图信息分析、定时器计数器等

## 2.2 启动Redis

> 宝塔安装有问题，我是后来自己安装，见Linux笔记。启动和运行的命令如下
> 
> redis-server /www/server/redis/redis.conf #指定配置文件
> redis-cli #直接运行 舒服了

启动Redis、并测试

```
redis-server /www/server/redis/redis.conf #指定配置文件
redis-cli -p 6379 #使用默认端口6379开启连接
ping #测试是否成功，成功就返回PONG
set k1 helloworld #设置一个键为k1,值为helloworld的键值对
get k1 #获取k1键的值，返回helloworld则成功
```

查看系统当前进程、关闭redis连接

```
#新开一个连接窗口
ps -ef|grep redis #查看当前进程
shutdown #关闭连接
exit #退出
ps -ef|grep redis #查看当前进程
```

- 执行ps命令 发现进程正开启

![image-20210705220738127](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211353.png)

- 关闭连接并退出

![image-20210705221251869](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211356.png)

- 执行ps命令 发现进程已关闭

![image-20210705221355936](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211359.png)

## 2.3 基础知识说明

默认16个数据库，类似数组下标从零开始，初始默认使用零号库

> Select命令切换数据库
> 
> Dbsize查看当前数据库的key的数量
> 
> Flushdb：清空当前库
> 
> Flushall：清空全部的库

Redis为什么使用单线程？

1. 单线程编程容易并且更容易维护；
2. Redis 的性能瓶颈不再 CPU ，主要在内存和网络；
3. 多线程就会存在死锁、线程上下文切换等问题，甚至会影响性能。

为什么要用Redis/为什么要用缓存？

高性能：

- 缓存位于内存中，直接操作内存比读取数据库更快
- 为了保证数据的一致性，数据库中的数据改变时需要同时改变缓存中的数据

高并发

- 一般像 MySQL 这类的数据库的 QPS 大概都在 1w 左右（4 核 8g）
- 使用 Redis 缓存之后很容易达到 10w+，甚至最高能达到 30w+（就单机 redis 的情况，redis 集群的话会更高）。

> QPS（Query Per Second）每秒查询率，是用来衡量服务性能的一个重要指标

# 3 五大数据类型[2]

## 3.1 string

1. **介绍** ：string 数据结构是简单的 key-value 类型。虽然 Redis 是用 C 语言写的，但是 Redis 并没有使用 C 的字符串表示，而是自己构建了一种 **简单动态字符串**（simple dynamic string，**SDS**）。相比于 C 的原生字符串，Redis 的 SDS 不光可以保存文本数据还可以保存二进制数据，并且获取字符串长度复杂度为 O(1)（C 字符串为 O(N)）,除此之外,Redis 的 SDS API 是安全的，不会造成缓冲区溢出。
2. **常用命令:** `set,get,strlen,exists,decr,incr,setex` 等等。
3. **应用场景** ：一般常用在需要计数的场景，比如用户的访问次数、热点文章的点赞转发数量等等。

下面我们简单看看它的使用！

**普通字符串的基本操作：**

```
127.0.0.1:6379> set key value #设置 key-value 类型的值
OK
127.0.0.1:6379> get key # 根据 key 获得对应的 value
"value"
127.0.0.1:6379> exists key  # 判断某个 key 是否存在
(integer) 1
127.0.0.1:6379> strlen key # 返回 key 所储存的字符串值的长度。
(integer) 5
127.0.0.1:6379> del key # 删除某个 key 对应的值
(integer) 1
127.0.0.1:6379> get key
(nil)
```

**批量设置** :

```
127.0.0.1:6379> mset key1 value1 key2 value2 # 批量设置 key-value 类型的值
OK
127.0.0.1:6379> mget key1 key2 # 批量获取多个 key 对应的 value
1) "value1"
2) "value2"
```

**计数器（字符串的内容为整数的时候可以使用）：**

```
127.0.0.1:6379> set number 1
OK
127.0.0.1:6379> incr number # 将 key 中储存的数字值增一
(integer) 2
127.0.0.1:6379> get number
"2"
127.0.0.1:6379> decr number # 将 key 中储存的数字值减一
(integer) 1
127.0.0.1:6379> get number
"1"
```

**过期**：

```
127.0.0.1:6379> expire key  60 # 数据在 60s 后过期
(integer) 1
127.0.0.1:6379> setex key 60 value # 数据在 60s 后过期 (setex:[set] + [ex]pire)
OK
127.0.0.1:6379> ttl key # 查看数据还有多久过期
(integer) 56
```

## 3.2 list

1. **介绍** ：**list** 即是 **链表**。链表是一种非常常见的数据结构，特点是易于数据元素的插入和删除并且且可以灵活调整链表长度。Redis 的 list 的实现为一个 **双向链表**，即可以支持反向查找和遍历，更方便操作，不过带来了部分额外的内存开销。
2. **常用命令:** `rpush,lpop,lpush,rpop,lrange、llen` 等。
3. **应用场景:** 发布与订阅或者说消息队列、慢查询。

下面我们简单看看它的使用！

**通过 `rpush/lpop` 实现队列：**

```
127.0.0.1:6379> rpush myList value1 # 向 list 的头部（右边）添加元素
(integer) 1
127.0.0.1:6379> rpush myList value2 value3 # 向list的头部（最右边）添加多个元素
(integer) 3
127.0.0.1:6379> lpop myList 1 # 将 list的尾部(最左边)1个元素取出
"value1"
```

**通过 `rpush/rpop` 实现栈：**

```
127.0.0.1:6379> rpush myList2 value1 value2 value3
(integer) 3
127.0.0.1:6379> rpop myList2 1 # 将 list的头部(最右边)的1个元素取出
"value3"
```

我专门花了一个图方便小伙伴们来理解：

![redis list](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211406.png)

**通过 `lrange` 查看对应下标范围的列表元素：**

```
127.0.0.1:6379> rpush myList value1 value2 value3
(integer) 3
127.0.0.1:6379> lrange myList 0 1 # 查看对应下标的list列表， 0 为 start,1为 end
1) "value1"
2) "value2"
127.0.0.1:6379> lrange myList 0 -1 # 查看列表中的所有元素，-1表示倒数第一
1) "value1"
2) "value2"
3) "value3"
```

通过 `lrange` 命令，你可以基于 list 实现分页查询，性能非常高！

**通过 `llen` 查看链表长度：**

```
127.0.0.1:6379> llen myList
(integer) 3
```

## 3.3 hash

1. **介绍** ：hash 类似于 JDK1.8 前的 HashMap(数组 + 链表)。不过，Redis 的 hash 做了更多优化。另外，hash 是一个 string 类型的 field 和 value 的映射表，**特别适合用于存储对象**，后续操作的时候，你可以直接仅仅修改这个对象中的某个字段的值。 比如我们可以 hash 数据结构来存储用户信息，商品信息等等。
2. **常用命令：** `hset,hmset,hexists,hget,hgetall,hkeys,hvals` 等。
3. **应用场景:** 系统中对象数据的存储。

下面我们简单看看它的使用！

```
127.0.0.1:6379> hmset userInfoKey name "guide" description "dev" age "24"
OK
127.0.0.1:6379> hexists userInfoKey name # 查看 key 对应的 value中指定的字段是否存在。
(integer) 1
127.0.0.1:6379> hget userInfoKey name # 获取存储在哈希表中指定字段的值。
"guide"
127.0.0.1:6379> hget userInfoKey age
"24"
127.0.0.1:6379> hgetall userInfoKey # 获取在哈希表中指定 key 的所有字段和值
1) "name"
2) "guide"
3) "description"
4) "dev"
5) "age"
6) "24"
127.0.0.1:6379> hkeys userInfoKey # 获取 key 列表
1) "name"
2) "description"
3) "age"
127.0.0.1:6379> hvals userInfoKey # 获取 value 列表
1) "guide"
2) "dev"
3) "24"
127.0.0.1:6379> hset userInfoKey name "GuideGeGe" # 修改某个字段对应的值
127.0.0.1:6379> hget userInfoKey name
"GuideGeGe"
```

## 3.4 set

1. **介绍 ：** set 类似于 Java 中的 `HashSet` 。Redis 中的 set 类型是一种无序集合，集合中的元素没有先后顺序。当你需要存储一个列表数据，又不希望出现重复数据时，set 是一个很好的选择，并且 set 提供了判断某个成员是否在一个 set 集合内的重要接口，这个也是 list 所不能提供的。可以基于 set 轻易实现交集、并集、差集的操作。比如：你可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis 可以非常方便的实现如共同关注、共同粉丝、共同喜好等功能。这个过程也就是求交集的过程。
2. **常用命令：** `sadd,spop,smembers,sismember,scard,sinterstore,sunion` 等。
3. **应用场景:** 需要存放的数据不能重复以及需要获取多个数据源交集和并集等场景

下面我们简单看看它的使用！

```
127.0.0.1:6379> sadd mySet value1 value2 # 添加元素进去
(integer) 2
127.0.0.1:6379> sadd mySet value1 # 不允许有重复元素
(integer) 0
127.0.0.1:6379> smembers mySet # 查看 set 中所有的元素
1) "value1"
2) "value2"
127.0.0.1:6379> scard mySet # 查看 set 的长度
(integer) 2
127.0.0.1:6379> sismember mySet value1 # 检查某个元素是否存在set 中，只能接收单个元素
(integer) 1
127.0.0.1:6379> sadd mySet2 value2 value3
(integer) 2
127.0.0.1:6379> sinterstore mySet3 mySet mySet2 # 获取 mySet 和 mySet2 的交集并存放在 mySet3 中
(integer) 1
127.0.0.1:6379> smembers mySet3
1) "value2"
```

## 3.5 sorted set

1. **介绍：** 和 set 相比，sorted set 增加了一个权重参数 score，使得集合中的元素能够按 score 进行有序排列，还可以通过 score 的范围来获取元素的列表。有点像是 Java 中 HashMap 和 TreeSet 的结合体。
2. **常用命令：** `zadd,zcard,zscore,zrange,zrevrange,zrem` 等。
3. **应用场景：** 需要对数据根据某个权重进行排序的场景。比如在直播系统中，实时排行信息包含直播间在线用户列表，各种礼物排行榜，弹幕消息（可以理解为按消息维度的消息排行榜）等信息。

```
127.0.0.1:6379> zadd myZset 3.0 value1 # 添加元素到 sorted set 中 3.0 为权重
(integer) 1
127.0.0.1:6379> zadd myZset 2.0 value2 1.0 value3 # 一次添加多个元素
(integer) 2
127.0.0.1:6379> zcard myZset # 查看 sorted set 中的元素数量
(integer) 3
127.0.0.1:6379> zscore myZset value1 # 查看某个 value 的权重
"3"
127.0.0.1:6379> zrange  myZset 0 -1 # 顺序输出某个范围区间的元素，0 -1 表示输出所有元素
1) "value3"
2) "value2"
3) "value1"
127.0.0.1:6379> zrange  myZset 0 1 # 顺序输出某个范围区间的元素，0 为 start  1 为 stop
1) "value3"
2) "value2"
127.0.0.1:6379> zrevrange  myZset 0 1 # 逆序输出某个范围区间的元素，0 为 start  1 为 stop
1) "value1"
2) "value2"
```

## 3.6 bitmap(不太懂)

1. **介绍 ：** bitmap 存储的是连续的二进制数字（0 和 1），通过 bitmap, 只需要一个 bit 位来表示某个元素对应的值或者状态，key 就是对应元素本身 。我们知道 8 个 bit 可以组成一个 byte，所以 bitmap 本身会极大的节省储存空间。
2. **常用命令：** `setbit` 、`getbit` 、`bitcount`、`bitop`
3. **应用场景:** 适合需要保存状态信息（比如是否签到、是否登录…）并需要进一步对这些信息进行分析的场景。比如用户签到情况、活跃用户情况、用户行为统计（比如是否点赞过某个视频）。

```
# 使用 bitmap 来记录上述事例中一周的打卡记录如下所示：
# 周一：1，周二：0，周三：0，周四：1，周五：1，周六：0，周天：0 （1 为打卡，0 为不打卡）
127.0.0.1:6379> setbit sign 0 1
0
127.0.0.1:6379> setbit sign 1 0
0
127.0.0.1:6379> setbit sign 2 0
0
127.0.0.1:6379> setbit sign 3 1
0
127.0.0.1:6379> setbit sign 4 1
0
127.0.0.1:6379> setbit sign 5 0
0
127.0.0.1:6379> setbit sign 6 0
0
# getbit 获取操作
127.0.0.1:6379> getbit sign 3 # 查看周四是否打卡
1
127.0.0.1:6379> getbit sign 6 # 查看周七是否打卡
0
# 统计这周打卡的记录，可以看到只有3天是打卡的状态：
127.0.0.1:6379> bitcount sign
3
```

# 4 Redis事务

Redis事务的简单理解就是将命令以队列的形式打包、放入一个队列中、然后一起执行所有命令。某条命令执行失败不会影响其他命令。Redis事务的操作和执行过程如下：

```
开始事务（MULTI）。
命令入队(批量操作 Redis 的命令，先进先出（FIFO）的顺序执行)。
执行事务(EXEC)。
```

Redis 可以通过 **`MULTI`，`EXEC`，`DISCARD` 和 `WATCH`** 等命令来实现事务(transaction)功能。

- 使用 [`MULTI`](https://redis.io/commands/multi)命令后可以输入多个命令。Redis 不会立即执行这些命令，而是将它们放到队列，当调用了[`EXEC`](https://redis.io/commands/exec)命令将执行所有命令。

```
> MULTI
OK
> SET USER "Guide哥"
QUEUED
> GET USER
QUEUED
> EXEC
1) OK
2) "Guide哥"
```

- 你也可以通过 [`DISCARD`](https://redis.io/commands/discard) 命令取消一个事务，它会清空事务队列中保存的所有命令。

```
> MULTI
OK
> SET USER "Guide哥"
QUEUED
> GET USER
QUEUED
> DISCARD
OK
```

- [`WATCH`](https://redis.io/commands/watch) 命令用于监听指定的键，当调用 `EXEC` 命令执行事务时，如果一个被 `WATCH` 命令监视的键被修改的话(在本事务外被修改)，整个事务都不会执行，直接返回失败。

在本事务中修改watch的变量a,不会有问题

```
127.0.0.1:6379> set a 11
OK
127.0.0.1:6379> watch a
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> get a
QUEUED
127.0.0.1:6379(TX)> set a 22
QUEUED
127.0.0.1:6379(TX)> get a
QUEUED
127.0.0.1:6379(TX)> exec
1) "11"
2) OK
3) "22"
```

在本事务外修改name的值，将会出错，**整个事务都不会执行**

```
# 窗口1 事务
127.0.0.1:6379> set name "wukang1"
OK
127.0.0.1:6379> watch name
OK
127.0.0.1:6379> multi #执行完这条后去窗口2
OK
127.0.0.1:6379(TX)> get name
QUEUED
127.0.0.1:6379(TX)> get name
QUEUED
127.0.0.1:6379(TX)> exec #发现执行的结果为nil 出问题了
(nil)
# 窗口2 修改name的值
127.0.0.1:6379> clear
127.0.0.1:6379> get name
"wukang1"
127.0.0.1:6379> set name "wukang2"
OK
```

总结

- Redis 是不支持 roll back 的，因而不满足原子性的（而且不满足持久性）
- Redis 事务提供了一种将多个命令请求打包的功能。然后，再按顺序执行打包的所有命令，并且不会被中途打断。
- watch指令类似于乐观锁，在事务提交时，如果watch监控的多个KEY中任何KEY的值已经被其他客户端更改，则使用EXEC执行事务时，事务队列将不会被执行，同时返回Nullmulti-bulk应答以通知调用者事务执行失败。

# 5 Jedis

Jedis是Redis官方推荐的Java连接开发工具。要在Java开发中使用好Redis中间件。是springboot集成Redis的前置知识，了解即可。

**5.1 Jedis连接Redis**

直接new一个Jedis对象，填入ip和端口号，就可以了。步骤如下：

> 1 新建一个redis-study的空项目，项目下建一个普通maven项目resid-01-jedis，注意我这里jdk用的11
> 
> 2 导入redis依赖，开启本地windows下的redis软件，双击redis-server.exe 和 redis-cli.exe
> 
> 3 编写测试代码、连接redis

2 导入redis依赖，开启本地redis软件

```
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.2.0</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.58</version>
</dependency>
```

3 编写测试代码、连接redis

```
package com.kuang.ping;
import redis.clients.jedis.Jedis;
public class Ping {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("127.0.0.1",6379);
        System.out.println("连接成功");
        //查看服务是否运行
        System.out.println("服务正在运行: "+jedis.ping());
    }
}
```

结果如下：连接成功！

![image-20210706135357768](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211416.png)

**5.2 Jedis类的API调用**

连接和关闭连接

```
Jedis jedis = new Jedis("127.0.0.1", 6379);
jedis.connect(); //连接
jedis.disconnect(); //断开连接
```

对key操作的命令

```
"清空数据："+jedis.flushDB();
"判断某个键是否存在："+jedis.exists("username")
"新增<'username','kuangshen'>的键值对："+jedis.set("username", "kuangshen")
"系统中所有的键如下：" Set<String> keys = jedis.keys("*");
"删除键password:"+jedis.del("password")
"查看键username所存储的值的类型："+jedis.type("username")
"随机返回key空间的一个："+jedis.randomKey()
"重命名key："+jedis.rename("username","name")
"按索引查询："+jedis.select(0)
"返回当前数据库中key的数目："+jedis.dbSize()
"删除所有数据库中的所有key："+jedis.flushAll()
```

对String操作的命令

```
jedis.mget();
jedis.setnx("key1", "value1");
```

对List操作命令

```
jedis.lpush();
jedis.rpop;
jedis.lrange();
jedis.ltrim();
jedis.sort();
```

对Set的操作

```
jedis.sadd();
jedis.smembers();
jedis.spop();
jedis.sismember();
jedis.sinter(); //交集并集
```

对Hash的操作命令

```
jedis.hmset();
jedis.hset();
jedis.hgetAll();
jedis.hkeys();
jedis.hvals();
jedis.hlen();
jedis.hdel();
jedis.hexists();
jedis.hmget();
```

**5.3 Jedis处理事务**

> //开启事务
> 
> Transaction multi = jedis.multi();

示例如下：

```
public class TestMulti {
    public static void main(String[] args) {
        //创建客户端连接服务端，redis服务端需要被开启
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        jedis.flushDB();
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello", "world");
        jsonObject.put("name", "java");
        //开启事务
        Transaction multi = jedis.multi();
        String result = jsonObject.toJSONString();
        try{
            //向redis存入一条数据
            multi.set("json", result);
            //再存入一条数据
            multi.set("json2", result);
            //这里引发了异常，用0作为被除数
            //int i = 100/0;
            //如果没有引发异常，执行进入队列的命令
            multi.exec();
        }catch(Exception e){
            e.printStackTrace();
            //如果出现异常，回滚
            multi.discard();
        }finally{
            System.out.println(jedis.get("json"));
            System.out.println(jedis.get("json2"));
            //最终关闭客户端
            jedis.close();
        }
    }
}
```

- 没有出现异常（注释掉`int i = 100/0`)

![image-20210706142409593](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211421.png)

- 出现异常

![image-20210706142445829](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211423.png)

# 6 SpringBoot整合Redis

## 6.1 使用内置RedisTemplate

先简单使用内置的RedisTemplate对象，用来连接和使用Redis

> 0 开启本地windows下的redis软件，双击redis-server.exe 和 redis-cli.exe
> 
> 1 新建一个springboot项目 勾选Redis
> 
> 2 在application.properties配置文件中配置redis
> 
> 3 在test文件夹下的Redis02SpringbootApplicationTests类中编写测试代码
> 
> 4 运行contextLoads方法、测试

1 新建一个springboot项目 勾选Redis，初始化的配置如下图

![image-20210706144801199](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211427.png)

2 在application.properties配置文件中配置redis

```
# Redis服务器地址
spring.redis.host=127.0.0.1
# Redis服务器连接端口
spring.redis.port=6379
```

3 在test文件夹下的Redis02SpringbootApplicationTests类中编写测试代码

```
@SpringBootTest
class Redis02SpringbootApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {
        redisTemplate.opsForValue().set("key","wukang");
        System.out.println(redisTemplate.opsForValue().get("key"));
    }

}
```

4 测试结果，打印值wukang成功！

![image-20210706185427629](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211431.png)

事实上，所有Redis的命令都集成在RedisTemplate中，使用RedisTemplate.XX()即可调用，一些基本的原生命令，如下

```
//1.基本命令（原生命令，实际开发中需要使用工具类RedisUtils）
// redisTemplate  操作不同的数据类型，api和我们的指令是一样的
// opsForValue    操作字符串 类似String
// opsForList     操作List 类似List
// opsForSet      操作Set
// opsForHash     操作Hash
// opsForZSet     操作ZSet
// opsForGeo      操作Geo
// opsForHyperLogLog  操作HyperLogLog
```

## 6.2 手动配置一个RedisTemplate

### 1 为什么要自己配置

对每一个组件springboot中都有一个XXXAutoConfiguration的自动配置类，和对应的XXXProperties，这里我们先看 **RedisAutoConfiguration 自动配置类**

```
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class,JedisConnectionConfiguration.class })
public class RedisAutoConfiguration {
    @Bean
    @ConditionalOnMissingBean(name = "redisTemplate") // 我们可以自己定义一个redisTemplate来替换这个默认的！
    public RedisTemplate<Object, Object>redisTemplate(RedisConnectionFactory redisConnectionFactory)throws UnknownHostException {
        // 默认的 RedisTemplate 没有过多的设置，redis 对象都是需要序列化！
        // 两个泛型都是 Object, Object 的类型，我们后使用需要强制转换 <String, Object>
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }

    @Bean
    @ConditionalOnMissingBean // 由于 String 是redis中最常使用的类型，所以说单独提出来了一个bean！
    public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory)throws UnknownHostException {
        StringRedisTemplate template = new StringRedisTemplate();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }
}
```

通过源码可以看出，SpringBoot自动帮我们在容器中生成了一个RedisTemplate和一个StringRedisTemplate。内置的RedisTemplate有一些缺点

- 内置的RedisTemplate的泛型是<Object,Object>，，泛型为<String,Object>将会更好用
- 内置的RedisTemplate没有设置key及value的序列化方式

**@ConditionalOnMissingBean(name = “redisTemplate”) 该注解表明我们自己配置一个RedisTemplate对象后、内置的RedisTemplate就不会被实例化了。所以这里我们自己写一个配置类RedisConfig配置之。**

### 2 编写RedisConfig和工具类

> 1 编写RedisConfig类
> 
> 2 写一个Redis工具类
> 
> 3 测试

1 编写RedisConfig类：主要工作是序列化、算是一个模板，也不知道到底有没有用。

```java
@Configuration
public class RedisConfig {
    //RedisTemplate序列化配置 -- >  注意要使用 @Qualifier("redisTemplate") 避免歧义（测试类中有使用案例）
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        // 我们为了自己开发方便，一般直接使用 <String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        // Json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // String 的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```

2 写一个Redis工具类（直接用RedisTemplate操作Redis，需要很多行代码，因此直接封装好一个RedisUtils，这样写代码更方便点。这个RedisUtils交给Spring容器实例化，使用时直接注解注入。）

```
package com.kuang.utils;

@Component
public final class RedisUtil {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    // =============================common============================
    /**
     * 指定缓存失效时间
     * @param key  键
     * @param time 时间(秒)
     */
    public boolean expire(String key, long time) {
        try {
            if (time > 0) {
                redisTemplate.expire(key, time, TimeUnit.SECONDS);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    public long getExpire(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }


    /**
     * 判断key是否存在
     * @param key 键
     * @return true 存在 false不存在
     */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除缓存
     * @param key 可以传一个值 或多个
     */
    @SuppressWarnings("unchecked")
    public void del(String... key) {
        if (key != null && key.length > 0) {
            if (key.length == 1) {
                redisTemplate.delete(key[0]);
            } else {
                redisTemplate.delete(String.valueOf(CollectionUtils.arrayToList(key)));
            }
        }
    }


    // ============================String=============================

    /**
     * 普通缓存获取
     * @param key 键
     * @return 值
     */
    public Object get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }

    /**
     * 普通缓存放入
     * @param key   键
     * @param value 值
     * @return true成功 false失败
     */
    public boolean set(String key, Object value) {
        try {
            redisTemplate.opsForValue().set(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 普通缓存放入并设置时间
     * @param key   键
     * @param value 值
     * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */
    public boolean set(String key, Object value, long time) {
        try {
            if (time > 0) {
                redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
            } else {
                set(key, value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 递增
     * @param key   键
     * @param delta 要增加几(大于0)
     */
    public long incr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, delta);
    }


    /**
     * 递减
     * @param key   键
     * @param delta 要减少几(小于0)
     */
    public long decr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递减因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, -delta);
    }


    // ================================Map=================================

    /**
     * HashGet
     * @param key  键 不能为null
     * @param item 项 不能为null
     */
    public Object hget(String key, String item) {
        return redisTemplate.opsForHash().get(key, item);
    }

    /**
     * 获取hashKey对应的所有键值
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object, Object> hmget(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * HashSet
     * @param key 键
     * @param map 对应多个键值
     */
    public boolean hmset(String key, Map<String, Object> map) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * HashSet 并设置时间
     * @param key  键
     * @param map  对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    public boolean hmset(String key, Map<String, Object> map, long time) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value, long time) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除hash表中的值
     *
     * @param key  键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    public void hdel(String key, Object... item) {
        redisTemplate.opsForHash().delete(key, item);
    }


    /**
     * 判断hash表中是否有该项的值
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return true 存在 false不存在
     */
    public boolean hHasKey(String key, String item) {
        return redisTemplate.opsForHash().hasKey(key, item);
    }


    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回
     *
     * @param key  键
     * @param item 项
     * @param by   要增加几(大于0)
     */
    public double hincr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, by);
    }


    /**
     * hash递减
     *
     * @param key  键
     * @param item 项
     * @param by   要减少记(小于0)
     */
    public double hdecr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, -by);
    }


    // ============================set=============================

    /**
     * 根据key获取Set中的所有值
     * @param key 键
     */
    public Set<Object> sGet(String key) {
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    public boolean sHasKey(String key, Object value) {
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将数据放入set缓存
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSet(String key, Object... values) {
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 将set数据放入缓存
     *
     * @param key    键
     * @param time   时间(秒)
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSetAndTime(String key, long time, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0)
                expire(key, time);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 获取set缓存的长度
     *
     * @param key 键
     */
    public long sGetSetSize(String key) {
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 移除值为value的
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 移除的个数
     */

    public long setRemove(String key, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    // ===============================list=================================

    /**
     * 获取list缓存的内容
     *
     * @param key   键
     * @param start 开始
     * @param end   结束 0 到 -1代表所有值
     */
    public List<Object> lGet(String key, long start, long end) {
        try {
            return redisTemplate.opsForList().range(key, start, end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 获取list缓存的长度
     *
     * @param key 键
     */
    public long lGetListSize(String key) {
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 通过索引 获取list中的值
     *
     * @param key   键
     * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
     */
    public Object lGetIndex(String key, long index) {
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     */
    public boolean lSet(String key, Object value) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将list放入缓存
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     */
    public boolean lSet(String key, Object value, long time) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    public boolean lSet(String key, List<Object> value) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    public boolean lSet(String key, List<Object> value, long time) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据索引修改list中的某条数据
     *
     * @param key   键
     * @param index 索引
     * @param value 值
     * @return
     */
    public boolean lUpdateIndex(String key, long index, Object value) {
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 移除N个值为value
     *
     * @param key   键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */
    public long lRemove(String key, long count, Object value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }

    }

}
```

3 测试类,位于test包下的哦

```
@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    //@Qualifier("redisTemplate")
    private RedisTemplate redisTemplate;
    @Autowired
    private RedisUtil redisUtil;

    @Test
    void contextLoads() {
        redisTemplate.opsForValue().set("key","wukang");
        System.out.println(redisTemplate.opsForValue().get("key"));
    }

    @Test
    void redisConfigTest(){
        User user = new User("吴康", 18);
        //将传入的对象序列化为json（自己配置RedisTemplate后就不需要这个）
        //String jsonUser=new ObjectMapper().writeValueAsString(user);
        redisTemplate.opsForValue().set("user", user);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }

    @Test
    void redisUtilTest(){
        redisUtil.set("name","吴康工具人");
        System.out.println(redisUtil.get("name"));
    }
}
```

# 7 Redis配置conf

目录下有redis.conf时

```
vim redis.conf #进入编辑该文件的界面
i #进入编辑模式
Esc #退出编辑进入一般模式
:q #退出
:wq #保存并退出
```

简单列几个重要的配置

- 网络

```
bind 127.0.0.1 # 绑定的ip
protected-mode yes # 保护模式
port 6379 # 端口设置
```

- 通用 GENERAL

```
daemonize yes # 以守护进程的方式运行，默认是 no，我们需要自己开启为yes！
pidfile /var/run/redis_6379.pid # 如果以后台的方式运行，我们就需要指定一个 pid 文件！
databases 16 # 数据库的数量，默认是 16 个数据库
```

- 持久化， 在规定的时间内，执行了多少次操作，则会持久化到文件 .rdb. aof。redis 是内存数据库，如果没有持久化，那么数据断电及失！

```
# 如果900s内，如果至少有一个1 key进行了修改，我们及进行持久化操作
save 900 1
# 如果300s内，如果至少10 key进行了修改，我们及进行持久化操作
save 300 10
# 如果60s内，如果至少10000 key进行了修改，我们及进行持久化操作
save 60 10000
# 我们之后学习持久化，会自己定义这个测试！
stop-writes-on-bgsave-error yes # 持久化如果出错，是否还需要继续工作！
rdbcompression yes # 是否压缩 rdb 文件，需要消耗一些cpu资源！
rdbchecksum yes # 保存rdb文件的时候，进行错误的检查校验！
dir ./ # rdb 文件保存的目录！
```

- SECURITY 安全，这里演示命令行设置一个密码

```
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> config get requirepass # 获取redis的密码
1) "requirepass"
2) ""
127.0.0.1:6379> config set requirepass "123456" # 设置redis的密码
OK
127.0.0.1:6379> config get requirepass # 发现所有的命令都没有权限了
(error) NOAUTH Authentication required.
127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456 # 使用密码进行登录！
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "123456"
```

- APPEND ONLY 模式 aof配置

```
appendonly no # 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，rdb完全够用！
appendfilename "appendonly.aof" # 持久化的文件的名字
# appendfsync always # 每次修改都会 sync。消耗性能
appendfsync everysec # 每秒执行一次 sync，可能会丢失这1s的数据！
# appendfsync no # 不执行 sync，这个时候操作系统自己同步数据，速度最快！
```

# 8 Redis 持久化

Redis 是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。Redis 不同于 Memcached 的很重要一点就是，Redis 支持持久化。

Redis 支持两种不同的持久化操作。**Redis 的一种持久化方式叫快照（snapshotting，RDB），另一种方式是只追加文件（append-only file, AOF）**。

## 8.1 快照（snapshotting）持久化（RDB）

RDB（Redis DataBase）：Redis 可以通过创建快照来获得存储在内存里面的数据在某个时间点上的副本。Redis 创建快照之后，可以对快照进行备份，可以将快照复制到其他服务器从而创建具有相同数据的服务器副本（Redis 主从结构，主要用来提高 Redis 性能），还可以将快照留在原地以便重启服务器的时候使用。

快照持久化是 Redis 默认采用的持久化方式，在 Redis.conf 配置文件中默认有此下配置：

```
# 如果900s(15分钟)内，如果至少有一个1 key进行了修改，我们及进行持久化操作
save 900 1
# 如果300s(5分钟)内，如果至少10 key进行了修改，我们及进行持久化操作
save 300 10
# 如果60s(1分钟)内，如果至少10000 key进行了修改，我们及进行持久化操作
save 60 10000
```

RDB的优缺点

- Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了极高的性能。**如果需要进行大规模数据的恢复**，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。
- RDB的缺点是最后一次持久化后的数据可能丢失。
- fork进程的时候，会占用一定的内存空间！！

![image.png](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211447.png)

## 8.2 AOF（append-only file）持久化

AOF将我们的所有命令都记录下来。开启 AOF 持久化后每执行一条会更改 Redis 中的数据的命令，Redis 就会将该命令写入硬盘中的 AOF 文件。AOF 文件的保存位置和 RDB 文件的位置相同，都是通过 dir 参数设置的，默认的文件名是 appendonly.aof。

与快照持久化相比，AOF 持久化 的实时性更好，因此已成为主流的持久化方案。默认情况下 Redis 没有开启 AOF（append only file）方式的持久化，可以通过 appendonly 参数开启：

```
appendonly yes
```

在 Redis 的配置文件中存在三种不同的 AOF 持久化方式，它们分别是：

```
#每次有数据修改发生时都会写入AOF文件,这样会严重降低Redis的速度
appendfsync always
#每秒钟同步一次，显示地将多个写命令同步到硬盘
appendfsync everysec
#让操作系统决定何时进行同步Copy to clipboardErrorCopied
appendfsync no
```

为了兼顾数据和写入性能，用户可以考虑 appendfsync everysec 选项 ，让 Redis 每秒同步一次 AOF 文件，Redis 性能几乎没受到任何影响。而且这样即使出现系统崩溃，用户最多只会丢失一秒之内产生的数据。当硬盘忙于执行写入操作的时候，Redis 还会优雅的放慢自己的速度以便适应硬盘的最大写入速度。

AOF的优缺点

- 每一次修改都同步，文件的完整会更加好！
- 从不同步，效率最高的！
- 相对于数据文件来说，aof远远大于 rdb，修复的速度也比 rdb慢！

# 9 Redis主从复制

## 9.1 主从复制架构

在第一节，数据库的架构演变中我们就提到过读写分离和主从复制。同样Redis所谓一种缓存（易失型数据），也有类似的主从复制的模式。

![image-20210704162949719](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211451.png)

Redis主从复制是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点（Master/Leader）,后者称为从节点（Slave/Follower）。

**主从复制的作用主要包括：**

1. ==数据冗余==：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式。
2. ==故障恢复==：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式
3. ==负载均衡==：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写的场景下，通过多个从节点分担负载，提高并发量。
4. ==高可用基石==：主从复制还是哨兵和集群能够实施的基础。

真实的项目为了防止宕机不可能使用单机的Redis，所以主从复制的架构是必须的，并且最简单的情况是一主二从。

![image.png](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211454.png)

## 9.2 一主二从环境配置

```
info replication   # 查看当前库的信息 执行
# 执行结果：
role:master #该服务器的角色是主机
connected_slaves:0 #该服务器没有从机
```

我的redis.conf在 /www/server/redis目录下，经过全局后简化了开启redis的命令，如下：（全局命令见Linux笔记）

```
redis-server /www/server/redis/redis.conf #指定配置文件
redis-cli #直接运行 舒服了
ps -aux | grep redis #查看redis相关的进程
```

### 1 开始配置主从环境

> 这里通过开启多个进程来模拟主从环境只是一种演示，实际业务中是需要多台服务器的！！
> 
> 1 复制并修改三个配置文件 redis79 redis80 redis81
> 
> 2 三个会话窗口开启三个不同的服务
> 
> 3 配置一主（79）二从（80、81），从机认老大

1 复制并修改三个配置文件 redis79 redis80 redis81

```
cd /www/server/redis
cp redis.conf redis79.conf
cp redis.conf redis80.conf
cp redis.conf redis81.conf
vim redis79.conf
vim redis80.conf
vim redis81.conf
//分别修改 端口、oid文件名、log文件名、dump文件名
1 指定端口 6379，依次类推
2 Pid文件名字 pidfile /var/run/redis_6379.pid, 依次类推
3 Log文件名字 logfile "6379.log", 依次类推
4 Dump.rdb文件名字 dbfilename dump6379.rdb, 依次类推
```

2 三个会话窗口开启三个不同的服务

```
redis-server /www/server/redis/redis79.conf
redis-cli -p 6379
redis-server /www/server/redis/redis80.conf
redis-cli -p 6380
redis-server /www/server/redis/redis81.conf
redis-cli -p 6381
ps -aux | grep redis #查看发现有三个进程 
```

![image-20210707105010907](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211500.png)

3 配置一主（79）二从（80、81），从机认老大

- 默认情况下，每台Redis服务器都是主节点；我们一般情况下只用配置从就好了！
- 真实的从主配置应该在配置文件中配置，这样的话是永久的，我们这里用的是命令，是暂时的！

```
SLAVEOF 127.0.0.1 6379 #两个从机分别执行
```

### 2 主从机的特点

- 主机可以写，从机不能写只能读！主机中的所有信息和数据，都会自动从机保存！
- 主机断开连接，从机依旧连接到主机的，但是没有写操作，这个候，主机如果回来了，从机依旧可以直接获取到该主机写的信息！
- 如果从机重启了（命令行来配置的主从的情况），就会变回主机（默认）！只要（重新设置）变为从机，立马就会从主机中获取值！
  - 只要是重新连接master，一次完全同步（全量复制）将被自动执行！

> 复制原理
> 
> Slave 启动成功连接到 master 后会发送一个sync同步命令。
> 
> Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，并完成一次完全同步。
> 
> - 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
> - 增量复制：Master 继续将新的所有收集到的修改命令依次传给slave，完成同步

在没有使用哨兵模式前，如果主机断开了连接，我们只能手动配置主机、从机！！

```
SLAVEOF no one #让自己变成主机
```

## 9.3 哨兵模式

哨兵模式能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库。哨兵是一个独立的进程，作为进程，它会独立运行。

==其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。==

![image.png](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211503.png)

这里的哨兵有两个作用

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式。

![image.png](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211509.png)

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为 **==主观下线==** 。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover[故障转移]操作。
切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为**==客观下线==**。

**优点：**

1. 哨兵集群，基于主从复制模式，所有的主从配置优点，它全有
2. 主从可以切换，故障可以转移，系统的可用性就会更好
3. 哨兵模式就是主从模式的升级，手动到自动，更加健壮！

**缺点：**

1. Redis 不好线扩容的，集群容量一旦到达上限，在线扩容就十分麻烦！
2. 实现哨兵模式的配置其实是很麻烦的，里面有很多选择！

# 10 缓存穿透和缓存雪崩[2]

## 10.1 缓存穿透

缓存穿透说简单点就是大量请求的 key 根本不存在于缓存中，导致请求直接到了数据库上，根本没有经过缓存这一层。举个例子：某个黑客故意制造我们缓存中不存在的 key 发起大量请求，导致大量请求落到数据库。

![image-20210707115232430](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211509.png)

解决缓存穿透问题，最基本的就是首先做好参数校验，一些不合法的参数请求直接抛出异常信息返回给客户端。比如查询的数据库 id 不能小于 0、传入的邮箱格式不对的时候直接返回错误消息给客户端等等。

除了基本的参数校验方法外，还有缓存无效key和布隆过滤器

### 1 缓存无效key

如果缓存和数据库都查不到某个 key 的数据就写一个到 Redis 中去并设置过期时间，具体命令如下： `SET key value EX 10086` 。这种方式可以解决请求的 key 变化不频繁的情况.

如果黑客恶意攻击，每次构建不同的请求 key，会导致 Redis 中缓存大量无效的 key 。很明显，这种方案并不能从根本上解决此问题。如果非要用这种方式来解决穿透问题的话，尽量将无效的 key 的过期时间设置短一点比如 1 分钟。

如果用 Java 代码展示的话，差不多是下面这样的：

```
public Object getObjectInclNullById(Integer id) {
    // 从缓存中获取数据
    Object cacheValue = cache.get(id);
    // 缓存为空
    if (cacheValue == null) {
        // 从数据库中获取
        Object storageValue = storage.get(key);
        // 缓存空对象
        cache.set(key, storageValue);
        // 如果存储数据为空，需要设置一个过期时间(300秒)
        if (storageValue == null) {
            // 必须设置过期时间，否则有被攻击的风险
            cache.expire(key, 60 * 5);
        }
        return storageValue;
    }
    return cacheValue;
}
```

### 2 布隆过滤器

布隆过滤器是一个非常神奇的数据结构，通过它我们可以非常方便地判断一个给定数据是否存在于海量数据中。

具体是这样做的：把所有可能存在的请求的值都存放在布隆过滤器中，当用户请求过来，先判断用户发来的请求的值是否存在于布隆过滤器中。不存在的话，直接返回请求参数错误信息给客户端，存在的话才会走下面的流程。

加入布隆过滤器之后的缓存处理流程图如下。

![image](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211513.png)

但是，需要注意的是布隆过滤器可能会存在误判的情况。总结来说就是： **布隆过滤器说某个元素存在，小概率会误判。布隆过滤器说某个元素不在，那么这个元素一定不在。**

*为什么会出现误判的情况呢? 我们还要从布隆过滤器的原理来说！*

我们先来看一下，**当一个元素加入布隆过滤器中的时候，会进行哪些操作：**

1. 使用布隆过滤器中的哈希函数对元素值进行计算，得到哈希值（有几个哈希函数得到几个哈希值）。
2. 根据得到的哈希值，在位数组中把对应下标的值置为 1。

我们再来看一下，**当我们需要判断一个元素是否存在于布隆过滤器的时候，会进行哪些操作：**

1. 对给定元素再次进行相同的哈希计算；
2. 得到值之后判断位数组中的每个元素是否都为 1，如果值都为 1，那么说明这个值在布隆过滤器中，如果存在一个值不为 1，说明该元素不在布隆过滤器中。

然后，一定会出现这样一种情况：**不同的字符串可能哈希出来的位置相同。** （可以适当增加位数组大小或者调整我们的哈希函数来降低概率）

## 10.2 缓存雪崩

**缓存在同一时间大面积的失效，后面的请求都直接落到了数据库上，造成数据库短时间内承受大量请求。**可能导致宕机。

### 缓存雪崩的原因

- 系统的缓存模块出了问题比如宕机导致不可用。造成系统的所有访问，都要走数据库。
- 有一些被大量访问数据（热点缓存）在某一时刻大面积失效，导致对应的请求直接落到了数据库上。

### 缓存雪崩的解决办法：

**针对 Redis 服务不可用的情况：**

1. 采用 Redis 集群，避免单机出现问题整个缓存服务都没办法使用。
2. 限流，避免同时处理大量的请求。

**针对热点缓存失效的情况：**

1. 设置不同的失效时间比如随机设置缓存的失效时间。
2. 缓存永不失效。

# 11 Redis发布订阅

简单来说就是用Redis server作为一个消息队列，实现消息通信！！Redis可以做，但我后面要学RabittMQ，这里就不深究了

**发布订阅的示意图如下：**

![image.png](https://blog-figure.oss-cn-hangzhou.aliyuncs.com/img/20211112211517.png)

**使用场景：**

1. 实时消息系统！
2. 事实聊天！（频道当做聊天室，将信息回显给所有人即可！）
3. 订阅，关注系统都是可以的！ 稍微复杂的场景我们就会使用 消息中间件MQ