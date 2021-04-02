# Redis

Redis是一个C语言编写的，开源、支持网络、基于内存、分布式、可选持久化的键值对存储数据库。

## Redis与Memcached

**共同点**：都是基于内存的数据库，都有过期策略

**区别**：

1.  Redis支持更丰富的数据类型：string、List、Set、Zset、Hash、Bitmap，Memcached只支持最简单的string。
2.  Redis支持数据持久化，有灾难恢复机制。
3.  Redis在内存使用完之后，可以将不用的数据方法磁盘上。Memcached在内存使用完之后直接报错。
4.  Redis的过期策略使用了惰性删除和定期删除。Memcached只用了惰性删除。

## 5种常见数据结构

### String(字符串)

1.  **介绍**：String是简单的key-value类型
2.  **常用命令**：`set/mset，get/mget，exists，strlen，del`等
3.  **应用场景**：计数，比如用户的访问次数、文章点赞转发数量等

**普通字符串的基本操作：**

```bash
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

**批量设置**：

```bash
127.0.0.1:6379> mset key1 value1 key2 value2 # 批量设置 key-value 类型的值
OK
127.0.0.1:6379> mget key1 key2 # 批量获取多个 key 对应的 value
1) "value1"
2) "value2"
```

**计数器(字符串为整数的时候可以使用)**

```bash
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

```bash
127.0.0.1:6379> expire key  60 # 数据在 60s 后过期
(integer) 1
127.0.0.1:6379> setex key 60 value # 数据在 60s 后过期 (setex:[set] + [ex]pire)
OK
127.0.0.1:6379> ttl key # 查看数据还有多久过期
(integer) 56
```

### List(列表)

1.  **介绍**：Redis的List实现为**双向链表**，支持反向查找和遍历。
2.  **常用命令**：`rpush,lpop,lpush,rpop,lrange、llen`等。
3.  **应用场景**：发布与订阅或者说消息队列、慢查询。

**通过 `rpush/lpop` 实现队列：**

```bash
127.0.0.1:6379> rpush myList value1 # 向 list 的头部（右边）添加元素
(integer) 1
127.0.0.1:6379> rpush myList value2 value3 # 向list的头部（最右边）添加多个元素
(integer) 3
127.0.0.1:6379> lpop myList # 将 list的尾部(最左边)元素取出
"value1"
127.0.0.1:6379> lrange myList 0 1 # 查看对应下标的list列表， 0 为 start,1为 end
1) "value2"
2) "value3"
127.0.0.1:6379> lrange myList 0 -1 # 查看列表中的所有元素，-1表示倒数第一
1) "value2"
2) "value3"
```

**通过 `rpush/rpop` 实现栈：**

```bash
127.0.0.1:6379> rpush myList2 value1 value2 value3
(integer) 3
127.0.0.1:6379> rpop myList2 # 将 list的头部(最右边)元素取出
"value3"
```

![redis list](F:\编程教程\个人笔记\图片\中间件\redis-list.png)

**通过 `lrange` 查看对应下标范围的列表元素，可以基于 list 实现分页查询，性能非常高：**

```bash
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

**通过 `llen` 查看链表长度：**

```bash
127.0.0.1:6379> llen myList
(integer) 3
```

### Hash(字典)

1.  **介绍**：Hash类似于java的HashMap，内部实现数组+链表。Hash是一个string的key和value的映射表，**特别适合用于存储对象**，可以直接仅仅修改这个对象中的某个字段的值。
2.  **常用命令：** `hset,hmset,hexists,hget,hgetall,hkeys,hvals` 等。
3.  **应用场景:** 系统中对象数据的存储。

```bash
127.0.0.1:6379> hset userInfoKey name "guide" description "dev" age "24"
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

### Set(集合)

1.  **介绍**：Set类似Java的HashSet。无序、不重复，并且 set 提供了判断某个成员是否在一个 set 集合内的重要接口，这个也是 list 所不能提供的。可以基于 set 轻易实现交集、并集、差集的操作。比如：你可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis 可以非常方便的实现如**共同关注、共同粉丝、共同喜好**等功能。这个过程也就是求**交集**的过程。

2.  **常用命令：** `sadd,spop,smembers,sismember,scard,sinterstore,sunion` 等。

3.  **应用场景**：需要存放的数据不能重复以及需要获取多个数据源交集和并集等场景。

4.  ```bash
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

### Zset/Sorted Set(有序集合)

1.  **介绍：** 和 set 相比，sorted set 增加了一个权重参数 score，使得集合中的元素能够**按 score 进行有序排列**，还可以通过 score 的范围来获取元素的列表。有点像是 Java 中 HashMap 和 TreeSet 的结合体。
2.  **常用命令：** `zadd,zcard,zscore,zrange,zrevrange,zrem` 等。
3.  **应用场景：** 需要对数据根据某个权重进行排序的场景。比如在直播系统中，实时排行信息包含直播间在线用户列表，各种礼物排行榜，弹幕消息（可以理解为按消息维度的消息排行榜）等信息。

```bash
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

### **Bitmap(位图、点阵图)**

**Redis 其实只支持 5 种数据类型，并没有 BitMap 这种类型，BitMap 底层是基于 Redis 的字符串类型实现的。**

在 Redis 中，可以把 Bitmaps 想象成一个以比特位为单位的数组，数组的每个单元只能存储0和1，数组的下标在 Bitmaps 中叫做偏移量。

![img](F:\编程教程\个人笔记\图片\中间件\redis-bitmap.png)

1.  **介绍**：Bitmap存储的是连续的二进制数字(0和1)，通过bitmap，只需要一个bit位来表示某个元素对应的值或者状态，key就是对应元素本身。8个bit一个字节，所以Bitmap极大的节省空间。
2.  **常用命令：** `setbit` 、`getbit` 、`bitcount`、`bitop`
3.  **应用场景**： 适合需要保存状态信息（比如是否签到、是否登录...）并需要进一步对这些信息进行分析的场景。比如用户签到情况、活跃用户情况、用户行为统计（比如是否点赞过某个视频）、是否在线。

```bash
# SETBIT 会返回之前位的值（默认是 0）这里会生成 7 个位
127.0.0.1:6379> setbit mykey 7 1
(integer) 0
127.0.0.1:6379> setbit mykey 7 0
(integer) 1
127.0.0.1:6379> getbit mykey 7
(integer) 0
127.0.0.1:6379> setbit mykey 6 1
(integer) 0
127.0.0.1:6379> setbit mykey 8 1
(integer) 0
# 通过 bitcount 统计被被设置为 1 的位的数量。
127.0.0.1:6379> bitcount mykey
(integer) 2
```

## Redis如何判断数据过期的？

Redis通过一个叫做**过期字典**(可以看作是hash表)来保存数据过期的时间。过期字典的键指向Redis数据库中的某个key，过期字典的值是一个long类型的整数，这个整数保存了key所指向的数据的过期时间。

## 过期键的删除策略

-   **惰性删除**：只会在取出key的时候才对数据检查是否过期。立即删除的话会占用大量的CPU，惰性删除这样对CPU的压力没那么大，但是可能会造成很多过期的key没有被删除。
-   **定时删除**：每隔一段时间抽取一批key执行删除过期的key，并且通过限制删除操作执行的时间和频率来减少删除操作对CPU的影响。

## 6种内存淘汰策略

仅仅给key设置过期时间还是有问题的，还是可能存在定期删除和惰性删除漏删过期key的情况，就会导致大量过期key占用内存。Redis的内存淘汰机制，就是解决这个问题。

设置内存最大使用量，当内存使用量超出时，会施行数据淘汰策略。最常用的是allkeys-lru。

| 策略            | 描述                                                         | 应用场景                                                     |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| volatile-lru    | 从已设置过期时间的数据集中挑选最近最少使用的数据淘汰         | 如果设置了过期时间，且分热数据与冷数据，推荐使用 volatile-lru 策略。 |
| volatile-ttl    | 从已设置过期时间的数据集中挑选将要过期的数据淘汰             | 如果让 Redis 根据 TTL 来筛选需要删除的key，请使用 volatile-ttl 策略。 |
| volatile-random | 从已设置过期时间的数据集中任意选择数据淘汰                   | 很少使用                                                     |
| allkeys-lru     | 从所有数据集中挑选最近最少使用的数据淘汰                     | 使用 Redis 缓存数据时，为了提高缓存命中率，需要保证缓存数据都是热点数据。可以将内存最大使用量设置为热点数据占用的内存量，然后启用 allkeys-lru 淘汰策略，将最近最少使用的数据淘汰。<br/>值得一提的是，设置 expire 会消耗额外的内存，所以使用 allkeys-lru 策略，可以更高效地利用内存，因为这样就可以不再设置过期时间了。 |
| allkeys-random  | 从所有数据集中任意选择数据进行淘汰                           | 如果需要循环读写所有的key，或者各个key的访问频率差不多，可以使用 allkeys-random 策略 |
| noeviction      | 不删除策略，达到最大内存限制时，如果需要更多内存，直接返回错误信息。大多数写命令都会导致占用更多的内存 | 很少使用                                                     |

## Redis持久化

很多时候需要把数据写到硬盘存储，为了之后可以重用数据，防止Redis挂掉然后恢复数据。

### 快照（snapshotting）持久化（RDB）

通过**指定时间间隔创建快照**来获得内存中的数据在某个时间点上的副本。创建快照之后，可以对快照进行备份，可以把快照复制到其他服务器来创建具有相同数据的服务器副本(Redis主从结构)，还可以把快照留在原地以便重启服务器的时候使用。

**快照持久化是 Redis 默认采用的持久化方式**，在 Redis.conf 配置文件中默认有此下配置：

```Redis.conf
save 900 1     #在900秒(15分钟)之后，如果至少有1个key发生变化，Redis就会自动触发BGSAVE命令创建快照。

save 300 10    #在300秒(5分钟)之后，如果至少有10个key发生变化，Redis就会自动触发BGSAVE命令创建快照。

save 60 10000  #在60秒(1分钟)之后，如果至少有10000个key发生变化，Redis就会自动触发BGSAVE命令创建快照。
```

#### 优点

1.  如果要进行大规模数据的恢复，RDB方式要比AOF方式恢复速度要快。
2.  RDB可以最大化Redis的性能，父进程会继续接受客户端请求，让子进程负责持久化操作。

#### 缺点

1.  不适合对数据完整性要求严格的情况，因为可能还没有创建快照服务就宕机了。
2.  如果数据集庞大，父进程folk出子进程的过程将非常耗时，可能出现服务器暂停客户端请求，将内存中的数据复制一份给子进程。

### 只追加文件（append-only file）持久化 (AOF)

与快照持久化相比，AOF持久化的实时性更好，默认没有开启，可以通过`appendonly yes`开启。

**以日志的形式记录Redis每一个写操作**，每执行一条更改Redis数据的命令，**Redis就会将该命令写入硬盘的AOF文件**。AOF 文件的保存位置和 RDB 文件的位置相同，都是通过 dir 参数设置的，默认的文件名是 appendonly.aof。

在 Redis 的配置文件中存在**三种不同的 AOF 持久化策略**别是：

```conf
appendfsync always    #每次有数据修改发生时都会写入AOF文件,这样会严重降低Redis的速度
appendfsync everysec  #每秒钟同步一次，显示地将多个写命令同步到硬盘
appendfsync no        #让操作系统决定何时进行同步
```

#### 优点

1.  与快照持久化相比，AOF持久化的实时性更好

#### 缺点

1.  对于相同的数据集，AOF文件要比RDB文件大
2.  使用 持久化策略，AOF的速度要慢于RDB

## 缓存穿透

**就是大量请求一个不存在的key，导致所有请求直接落到了数据库上。**

**解决方案**

最基本的是做好参数校验，一些不合法的参数请求直接返回异常信息给客户端

1.  **缓存无效key**

    如果Redis中和数据库都查不到这个key的数据，就把这个key写入到Redis并设置过期时间。

    这种方式只能解决请求的key变化不频繁的情况，每次请求大量不同的key，还会导致Redis中缓存大量无效的key。

    如果要采用这种方式，过期时间尽量设置短一些。

2.  **布隆过滤器**

    **布隆过滤器说某个元素存在，小概率会误判。布隆过滤器说某个元素不在，那么这个元素一定不在。**

    具体是这样做的：把所有可能存在的请求的值都存放在布隆过滤器中，当用户请求过来，先判断用户发来的请求的值是否存在于布隆过滤器中。不存在的话，直接返回请求参数错误信息给客户端，存在的话才会走下面的流程。

    ![image](F:\编程教程\个人笔记\图片\中间件\加入布隆过滤器后的缓存处理流程.png)

## 缓存雪崩

**缓存在同一时间大面积的失效，后面的请求都直接落到了数据库上，造成数据库短时间内承受大量请求。**

举个例子 ：秒杀开始 12 个小时之前，我们统一存放了一批商品到 Redis 中，设置的缓存过期时间也是 12 个小时，那么秒杀开始的时候，这些秒杀的商品的访问直接就失效了。导致的情况就是，相应的请求直接就落到了数据库上，就像雪崩一样可怕。

**解决方案**

1.  **针对 Redis 服务不可用的情况：**
    -   采用 Redis 集群，避免单机出现问题整个缓存服务都没办法使用。
    -   限流，避免同时处理大量的请求。
2.  **针对热点缓存失效的情况：**
    -   设置不同的失效时间比如随机设置缓存的失效时间。
    -   缓存永不失效。

## 保证Redis数据和数据库数据一致性？

### Cache Aside Pattern（旁路缓存模式）

-   读的时候，先读缓存，缓存没有的话，就读数据库，然后取出数据后放入缓存，同时返回响应。
-   更新的时候，**先更新数据库，然后再删除缓存**，如果删除缓存失败，隔一段时间重试，多次重试失败，就把这个失败的key放入队列，等Redis服务可用的时候再将key删除。

