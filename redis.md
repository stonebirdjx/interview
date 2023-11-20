<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [什么是Redis](#%E4%BB%80%E4%B9%88%E6%98%AFredis)
- [Redis 数据类型](#redis-%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
- [Redis 是单线程吗](#redis-%E6%98%AF%E5%8D%95%E7%BA%BF%E7%A8%8B%E5%90%97)
- [Redis 采用单线程为什么还这么快？](#redis-%E9%87%87%E7%94%A8%E5%8D%95%E7%BA%BF%E7%A8%8B%E4%B8%BA%E4%BB%80%E4%B9%88%E8%BF%98%E8%BF%99%E4%B9%88%E5%BF%AB)
  - [Redis 6.0 之后为什么引入了多线程？](#redis-60-%E4%B9%8B%E5%90%8E%E4%B8%BA%E4%BB%80%E4%B9%88%E5%BC%95%E5%85%A5%E4%BA%86%E5%A4%9A%E7%BA%BF%E7%A8%8B)
- [Redis 持久化](#redis-%E6%8C%81%E4%B9%85%E5%8C%96)
- [Redis 如何实现服务高可用？](#redis-%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%9C%8D%E5%8A%A1%E9%AB%98%E5%8F%AF%E7%94%A8)
  - [主从复制](#%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6)
  - [Sentinel（哨兵）模式](#sentinel%E5%93%A8%E5%85%B5%E6%A8%A1%E5%BC%8F)
  - [Cluster模式](#cluster%E6%A8%A1%E5%BC%8F)
  - [Ring模式](#ring%E6%A8%A1%E5%BC%8F)
- [Redis 使用的过期删除策略是什么？](#redis-%E4%BD%BF%E7%94%A8%E7%9A%84%E8%BF%87%E6%9C%9F%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5%E6%98%AF%E4%BB%80%E4%B9%88)
  - [什么是惰性删除策略？](#%E4%BB%80%E4%B9%88%E6%98%AF%E6%83%B0%E6%80%A7%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5)
  - [什么是定期删除策略？](#%E4%BB%80%E4%B9%88%E6%98%AF%E5%AE%9A%E6%9C%9F%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5)
- [Redis 持久化时，对过期键会如何处理的](#redis-%E6%8C%81%E4%B9%85%E5%8C%96%E6%97%B6%E5%AF%B9%E8%BF%87%E6%9C%9F%E9%94%AE%E4%BC%9A%E5%A6%82%E4%BD%95%E5%A4%84%E7%90%86%E7%9A%84)
- [Redis 内存满了，会发生什么](#redis-%E5%86%85%E5%AD%98%E6%BB%A1%E4%BA%86%E4%BC%9A%E5%8F%91%E7%94%9F%E4%BB%80%E4%B9%88)
- [LRU 算法和 LFU 算法有什么区别？](#lru-%E7%AE%97%E6%B3%95%E5%92%8C-lfu-%E7%AE%97%E6%B3%95%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)
- [什么是缓存雪崩、击穿、穿透](#%E4%BB%80%E4%B9%88%E6%98%AF%E7%BC%93%E5%AD%98%E9%9B%AA%E5%B4%A9%E5%87%BB%E7%A9%BF%E7%A9%BF%E9%80%8F)
  - [缓存雪崩](#%E7%BC%93%E5%AD%98%E9%9B%AA%E5%B4%A9)
  - [缓存击穿](#%E7%BC%93%E5%AD%98%E5%87%BB%E7%A9%BF)
  - [缓存穿透](#%E7%BC%93%E5%AD%98%E7%A9%BF%E9%80%8F)
- [缓存如何保证一致性](#%E7%BC%93%E5%AD%98%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81%E4%B8%80%E8%87%B4%E6%80%A7)
  - [可以做到强一致吗？](#%E5%8F%AF%E4%BB%A5%E5%81%9A%E5%88%B0%E5%BC%BA%E4%B8%80%E8%87%B4%E5%90%97)
- [说说常见的缓存更新策略？](#%E8%AF%B4%E8%AF%B4%E5%B8%B8%E8%A7%81%E7%9A%84%E7%BC%93%E5%AD%98%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5)
  - [Cache Aside（旁路缓存）策略](#cache-aside%E6%97%81%E8%B7%AF%E7%BC%93%E5%AD%98%E7%AD%96%E7%95%A5)
  - [Read/Write Through（读穿 / 写穿）策略；](#readwrite-through%E8%AF%BB%E7%A9%BF--%E5%86%99%E7%A9%BF%E7%AD%96%E7%95%A5)
  - [Write Back（写回）策略](#write-back%E5%86%99%E5%9B%9E%E7%AD%96%E7%95%A5)
- [Redis 如何实现延迟队列？](#redis-%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E5%BB%B6%E8%BF%9F%E9%98%9F%E5%88%97)
- [Redis 大 key](#redis-%E5%A4%A7-key)
- [Redis 管道有什么用？](#redis-%E7%AE%A1%E9%81%93%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8)
- [为什么Redis 不支持事务回滚？](#%E4%B8%BA%E4%BB%80%E4%B9%88redis-%E4%B8%8D%E6%94%AF%E6%8C%81%E4%BA%8B%E5%8A%A1%E5%9B%9E%E6%BB%9A)
- [如何用 Redis 实现分布式锁的？](#%E5%A6%82%E4%BD%95%E7%94%A8-redis-%E5%AE%9E%E7%8E%B0%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81%E7%9A%84)
- [Lua](#lua)
- [限速器](#%E9%99%90%E9%80%9F%E5%99%A8)
- [遍历 Key](#%E9%81%8D%E5%8E%86-key)
  - [Cluster 和 Ring](#cluster-%E5%92%8C-ring)
  - [删除无过期时间的 Key](#%E5%88%A0%E9%99%A4%E6%97%A0%E8%BF%87%E6%9C%9F%E6%97%B6%E9%97%B4%E7%9A%84-key)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 什么是Redis

<font color="red">Redis 是一种基于内存的数据库</font>，对数据的读写操作都是在内存中完成，因此读写速度非常快，<font color="red">常用于缓存，消息队列、分布式锁等场景</font>。

> 高性能、高并发（10wQPS）

# Redis 数据类型

Redis 提供了丰富的数据类型，常见的有五种数据类型：String（字符串），Hash（哈希），List（列表），Set（集合）、Zset（有序集合）

> 还有其他的流、地图等类型

# Redis 是单线程吗

Redis 程序并不是单线程的。

Redis 单线程指的是「接收客户端请求->解析请求 ->进行数据读写等操作->发送数据给客户端」这个过程是由一个线程（主线程）来完成的。

# Redis 采用单线程为什么还这么快？

- Redis 的大部分操作都在内存中完成，并且采用了高效的数据结构，因此 Redis 瓶颈可能是机器的内存或者网络带宽，而并非 CPU，既然 CPU 不是瓶颈，那么自然就采用单线程的解决方案了；
- Redis 采用了 I/O 多路复用机制处理大量的客户端 Socket 请求，IO 多路复用机制是指一个线程处理多个 IO 流，就是我们经常听到的 select/epoll 机制

## Redis 6.0 之后为什么引入了多线程？

在 Redis 6.0 版本之后，也采用了多个 I/O 线程来处理网络请求，这是因为随着网络硬件的性能提升，Redis 的性能瓶颈有时会出现在网络 I/O 的处理上。

<font color="red">但是对于命令的执行，Redis 仍然使用单线程来处理</font>，所以大家不要误解 Redis 有多线程同时执行命令。

# Redis 持久化

- AOF 日志：每执行一条写操作命令，就把该命令以追加的方式写入到一个文件里；
- RDB 快照：将某一时刻的内存数据，以二进制的方式写入磁盘；
- 混合持久化方式：Redis 4.0 新增的方式，集成了 AOF 和 RBD 的优点

# Redis 如何实现服务高可用？

## 主从复制

主从复制模式中包含一个主数据库实例（master）与一个或多个从数据库实例（slave）

- slave启动后，向master发送SYNC命令，master接收到SYNC命令后通过bgsave保存快照（即上文所介绍的RDB持久化），并使用缓冲区记录保存快照这段时间内执行的写命令
- master将保存的快照文件发送给slave，并继续记录执行的写命令
-  slave接收到快照文件后，加载快照文件，载入数据

`slave`的`redis.conf`的主要配置 

```Go
replicaof 127.0.0.1 6379 # master的ip，port  
masterauth 123456 # master的密码 
replica-serve-stale-data no # 如果slave无法与master同步，设置成slave不可读，方便监控脚本发现问题 

redis-server master.conf  
redis-server slave1.conf  
redis-server slave2.conf 
```

主从复制的优缺点:

优点：

- master能自动将数据同步到slave，可以进行读写分离，分担master的读压力
- master、slave之间的同步是以非阻塞的方式进行的，同步期间，客户端仍然可以提交查询或更新请求

缺点:

- 不具备自动容错与恢复功能，master或slave的宕机都可能导致客户端请求失败，需要等待机器重启或手动切换客户端IP才能恢复
-  master宕机，如果宕机前数据没有同步完，则切换IP后会存在数据不一致的问题
- 难以支持在线扩容，Redis的容量受限于单机配置

## Sentinel（哨兵）模式

<font color="red">哨兵模式基于主从复制模式，只是引入了哨兵来监控与自动处理故障</font>。

哨兵顾名思义，就是来为Redis集群站哨的，一旦发现问题能做出相应的应对处理。其功能包括

- 监控master、slave是否正常运行
- 当master出现故障时，能自动将一个slave转换为master（大哥挂了，选一个小弟上位）
- 多个哨兵可以监控同一个Redis，哨兵之间也会自动监控

`sentinel.conf`配置文件

```Bash
sentinel monitor mymaster 127.0.0.1 6379 1 # mymaster定义一个master数据库的名称，后面是master的ip， port，1表示至少需要一个Sentinel进程同意才能将master判断为失效，如果不满足这个条件，则自动故障转移（failover）不会执行 
sentinel auth-pass mymaster 123456 # master的密码 
sentinel down-after-milliseconds mymaster 5000 # 5s未回复PING，则认为master主观下线，默认为30s 
sentinel parallel-syncs mymaster 2  # 指定在执行故障转移时，最多可以有多少个slave实例在同步新的master实例，在slave实例较多的情况下这个数字越小，同步的时间越长，完成故障转移所需的时间就越长 
sentinel failover-timeout mymaster 300000 # 如果在该时间（ms）内未能完成故障转移操作，则认为故障转移失败，生产环境需要根据数据量设置该值 


redis-server sentinel1.conf --sentinel  
redis-server sentinel2.conf --sentinel 
redis-server sentinel3.conf --sentinel 
```

哨兵模式的优缺点

优点：

- 哨兵模式基于主从复制模式，所以主从复制模式有的优点，哨兵模式也有
- 哨兵模式下，master挂掉可以自动进行切换，系统可用性更高

缺点：

-  同样也继承了主从模式难以在线扩容的缺点，Redis的容量受限于单机配置
-  需要额外的资源来启动sentinel进程，实现相对复杂一点，同时slave节点作为备份节点不提供服务

## Cluster模式

> 集群部署至少要 3 台以上的master节点，最好使用 3 主 3 从六个节点的模式。

哨兵模式解决了主从复制不能自动故障转移，达不到高可用的问题，但还是存在难以在线扩容，Redis容量受限于单机配置的问题。

Cluster采用无中心结构:

- 所有的redis节点彼此互联(PING-PONG机制),内部使用二进制协议优化传输速度和带宽
- 节点的fail是通过集群中超过半数的节点检测失效时才生效
- 客户端与redis节点直连,不需要中间代理层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可

```
redis.conf
# 端口
port 7001  
# 启用集群模式
cluster-enabled yes 
# 根据你启用的节点来命名，最好和端口保持一致，这个是用来保存其他节点的名称，状态等信息的
cluster-config-file nodes_7001.conf 
# 超时时间
cluster-node-timeout 5000
appendonly yes
# 后台运行
daemonize yes
# 非保护模式
protected-mode no 
pidfile  /var/run/redis_7001.pid


redis-server redis7001.conf
redis-server redis7006.conf

# 启动集群
redis-cli --cluster create 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 127.0.0.1:7006 --cluste r-replicas 1

# # 注意 集群模式下要带参数 -c，表示集群，否则不能正常存取数据！！！
127.0.0.1:7001> set k1 v1
-> Redirected to slot [12706] located at 127.0.0.1:7003
OK
# 把数据存到计算得出的 slot，这里还自动跳到了 7003
127.0.0.1:7003> get k1
"v1"
```

<font color="red">Redis Cluster 方案采用哈希槽（Hash Slot），来处理数据和节点之间的映射关系。在 Redis Cluster 方案中，一个切片集群共有 16384 个哈希槽，</font>

- 平均分配： 在使用 cluster create 命令创建 Redis 集群时，Redis 会自动把所有哈希槽平均分布到集群节点上。比如集群中有 9 个节点，则每个节点上槽的个数为 16384/9 个。
- 手动分配： 可以使用 cluster meet 命令手动建立节点间的连接，组成集群，再使用 cluster addslots 命令，指定每个节点上的哈希槽个数。

## Ring模式

Ring 分片客户端，是采用了一致性 HASH 算法在多个 redis 服务器之间分发 key，每个节点承担一部分 key 的存储。

Ring 客户端会监控每个节点的健康状况，并从 Ring 中移除掉宕机的节点，当节点恢复时，会再加入到 Ring 中。这样实现了可用性和容错性，但节点和节点之间没有一致性，仅仅是通过多个节点分摊流量的方式来处理更多的请求。如果你更注重一致性、分区、安全性，请使用 `Redis Cluster`。

# Redis 使用的过期删除策略是什么？

Redis 使用的过期删除策略是「惰性删除+定期删除」这两种策略配和使用。

## 什么是惰性删除策略？

不主动删除过期键，每次从数据库访问 key 时，都检测 key 是否过期，如果过期则删除该 key。

- 优点：因为每次访问时，才会检查 key 是否过期，所以此策略只会使用很少的系统资源，因此，惰性删除策略对 CPU 时间最友好。
- 缺点：如果一个 key 已经过期，而这个 key 又仍然保留在数据库中，那么只要这个过期 key 一直没有被访问，它所占用的内存就不会释放，造成了一定的内存空间浪费。所以，惰性删除策略对内存不友好。

## 什么是定期删除策略？

每隔一段时间「随机」从数据库中取出一定数量的 key 进行检查，并删除其中的过期key。

# Redis 持久化时，对过期键会如何处理的

RDB：

- RDB 文件生成阶段：<font color="red">会对 key 进行过期检查，过期的键「不会」被保存到新的 RDB 文件中</font>，因此 Redis 中的过期键不会对生成新 RDB 文件产生任何影响。
- RDB 加载阶段：
  - 如果 Redis 是「主服务器」运行模式的话，在载入 RDB 文件时，程序会对文件中保存的键进行检查，过期键「不会」被载入到数据库中。
  - 如果 Redis 是「从服务器」运行模式的话，在载入 RDB 文件时，不论键是否过期都会被载入到数据库中。

AOF 

- AOF 文件写入阶段：<font color="red">当 Redis 以 AOF 模式持久化时，如果数据库某个过期键还没被删除，那么 AOF 文件会保留此过期键，当此过期键被删除后，Redis 会向 AOF 文件追加一条 DEL 命令来显式地删除该键值</font>。
- AOF 重写阶段：执行 AOF 重写时，会对 Redis 中的键值对进行检查，已过期的键不会被保存到重写后的 AOF 文件中，因此不会对 AOF 重写造成任何影响。

# Redis 内存满了，会发生什么

在 Redis 的运行内存达到了某个阀值，就会触发内存淘汰机制，这个阀值就是我们设置的最大运行内存，此值在 Redis 的配置文件中可以找到，配置项为 maxmemory。

- 不进行数据淘汰的策略：它表示当运行内存超过最大设置内存时，不淘汰任何数据，而是不再提供服务，直接返回错误。
- 进行数据淘汰的策略：
  - volatile-random：随机淘汰设置了过期时间的任意键值；
  - volatile-ttl：优先淘汰更早过期的键值。
  - volatile-lru（Redis3.0 之前，默认的内存淘汰策略）：淘汰所有设置了过期时间的键值中，最久未使用的键值；
  - volatile-lfu（Redis 4.0 后新增的内存淘汰策略）：淘汰所有设置了过期时间的键值中，最少使用的键值；

# LRU 算法和 LFU 算法有什么区别？

LRU 全称是 Least Recently Used 翻译为最近最少使用

- 传统 LRU 算法的实现是基于「链表」结构。

LFU 全称是 Least Frequently Used 翻译为最近最不常用的

- 它的核心思想是“如果数据过去被访问多次，那么将来被访问的频率也更高”。

# 什么是缓存雪崩、击穿、穿透

用户的数据一般都是存储于数据库，数据库的数据是落在磁盘上的，磁盘的读写速度可以说是计算机里最慢的硬件了。

当用户的请求，都访问数据库的话，请求数量一上来，数据库很容易就奔溃的了，所以为了避免用户直接访问数据库，会用 Redis 作为缓存层。

引入了缓存层，就会有缓存异常的三个问题，分别是缓存雪崩、缓存击穿、缓存穿透。

## 缓存雪崩

<font color="red">当大量缓存数据在同一时间过期（失效）或者 Redis 故障宕机时</font>，如果此时有大量的用户请求，都无法在 Redis 中处理，于是全部请求都直接访问数据库，从而导致数据库的压力骤增，严重的会造成数据库宕机，从而形成一系列连锁反应，造成整个系统崩溃，这就是缓存雪崩的问题。

**大量缓存数据在同一时间过期（失效）**：

- 均匀设置过期时间：避免将大量的数据设置成同一个过期时间。
- 互斥锁：如果发现访问的数据不在 Redis 里，就加个互斥锁，保证同一时间内只有一个请求来构建缓存
- 后台更新缓存：业务线程不再负责更新缓存，缓存也不设置有效期，而是让缓存“永久有效”，并将更新缓存的工作交由后台线程定时更新。

**Redis 故障宕机**：

- 服务熔断或请求限流机制: 启动服务熔断机制，暂停业务应用对缓存服务的访问，直接返回错误。
- 构建 Redis 缓存高可靠集群:  主从节点的方式构建 Redis 缓存高可靠集群。

## 缓存击穿

如果缓存中的某个热点数据过期了，此时大量的请求访问了该热点数据，就无法从缓存中读取，直接访问数据库，数据库很容易就被高并发的请求冲垮，这就是缓存击穿的问题。

> 可以发现缓存击穿跟缓存雪崩很相似，你可以认为缓存击穿是缓存雪崩的一个子集。

- 互斥锁方案: 保证同一时间只有一个业务线程更新缓存
- 不给热点数据设置过期时间: 由后台更新

## 缓存穿透

<font color="red">当用户访问的数据，既不在缓存中，也不在数据库中</font>，导致请求在访问缓存时，发现缓存缺失，再去访问数据库时，发现数据库中也没有要访问的数据，没办法构建缓存数据，来服务后续的请求。那么当有大量这样的请求到来时，数据库的压力骤增，这就是缓存穿透的问题。

- 非法请求的限制；
- 使用默认值（不推荐）。

# 缓存如何保证一致性

无论是「先更新数据库，再更新缓存」，还是「先更新缓存，再更新数据库」，这两个方案都存在并发问题，当两个请求并发更新同一条数据的时候，可能会出现缓存和数据库中的数据不一致的现象。

先删除缓存，再更新数据库，在「读 + 写」并发的时候，还是会出现缓存和数据库的数据不一致的问题。

<font color="red">先更新数据库，再删除缓存也是会出现数据不一致性的问题。加上了「过期时间」，就算在这期间存在缓存数据不一致，有过期时间来兜底，这样也能达到最终一致</font>。

**延迟双删: 两次删除**

```Go
// 删除缓存
redis.delKey(X)
// 更新数据库
db.update(X)
// 睡眠
Thread.sleep(N)
// 再删除缓存
redis.delKey(X)
```

> 最终一致性：先更新数据库，再删除缓存（最后必须删除一次）

## 可以做到强一致吗？

引入缓存的目的是什么？性能。一旦我们决定使用缓存，那必然要面临一致性问题。性能和一致性就像天平的两端，无法做到都满足要求。

- 「更新数据库 + 更新缓存」的方案，在更新缓存前先加个分布式锁，保证同一时间只运行一个请求更新缓存，就会不会产生并发问题了，当然引入了锁后，对于写入的性能就会带来影响。

# 说说常见的缓存更新策略？

常见的缓存更新策略共有3种：

- Cache Aside（旁路缓存）策略；
- Read/Write Through（读穿 / 写穿）策略；
- Write Back（写回）策略；

## Cache Aside（旁路缓存）策略

<font color="red">Cache Aside（旁路缓存）策略是最常用的，应用程序直接与「数据库、缓存」交互，并负责对缓存的维护，该策略又可以细分为「读策略」和「写策略」</font>。

- 写策略的步骤：
  - 先更新数据库中的数据，再删除缓存中的数据。
- 读策略的步骤：
  - 如果读取的数据命中了缓存，则直接返回数据；
  - 如果读取的数据没有命中缓存，则从数据库中读取数据，然后将数据写入到缓存，并且返回给用户。

<font color="red">注意，写策略的步骤的顺序不能倒过来，即不能先删除缓存再更新数据库，原因是在「读+写」并发的时候，会出现缓存和数据库的数据不一致性的问题</font>。

## Read/Write Through（读穿 / 写穿）策略；

Read/Write Through（读穿 / 写穿）策略原则是应用程序只和缓存交互，不再和数据库交互，而是由缓存和数据库交互，相当于更新数据库的操作由缓存自己代理了。

- Read Through 策略：先查询缓存中数据是否存在，如果存在则直接返回，如果不存在，则由缓存组件负责从数据库查询数据，并将结果写入到缓存组件，最后缓存组件将数据返回给应用。
- Write Through 策略：当有数据更新的时候，先查询要写入的数据在缓存中是否已经存在：
  - 如果缓存中数据已经存在，则更新缓存中的数据，并且由缓存组件同步更新到数据库中，然后缓存组件告知应用程序更新完成。
  - 如果缓存中数据不存在，直接更新数据库，然后返回；

## Write Back（写回）策略

Write Back（写回）策略在更新数据的时候，只更新缓存，同时将缓存数据设置为脏的，然后立马返回，并不会更新数据库。对于数据库的更新，会通过批量异步更新的方式进行。

Write Back 策略特别适合写多的场景

但是带来的问题是，数据不是强一致性的，而且会有数据丢失的风险

# Redis 如何实现延迟队列？

延迟队列是指把当前要做的事情，往后推迟一段时间再做。如<font color="red">订单会自动取消</font>

在 Redis 可以使用有序集合（ZSet）的方式来实现延迟消息队列的，ZSet 有一个 Score 属性可以用来存储延迟执行的时间。

使用 zadd score1 value1 命令就可以一直往内存中生产消息。再利用 zrangebysocre 查询符合条件的所有待处理的任务， 通过循环执行队列任务即可。

![](./images/zadd.png)

# Redis 大 key 

大 key 并不是指 key 的值很大，而是 key 对应的 value 很大。

一般而言，下面这两种情况被称为大 key：

- String 类型的值大于 10 KB；
- Hash、List、Set、ZSet 类型的元素的个数超过 5000个；

大 key 会带来以下四种影响:

- 客户端超时阻塞
- 引发网络阻塞
- 阻塞工作线程
- 内存分布不均

查找大key

Cli

```Bash
redis-cli -h 127.0.0.1 -p6379 -a "password" -- bigkeys
```

scan

```go
var cursor uint64
for {
        var keys []string
        var err error
        keys, cursor, err = rdb.Scan(ctx, cursor, "prefix:*", 0).Result()
        if err != nil {
                panic(err)
        }

        for _, key := range keys {
                fmt.Println("key", key)
        }

        // 没有更多key了
        if cursor == 0 {
                break
        }
}
```

# Redis 管道有什么用？

管道技术（Pipeline）是客户端提供的一种批处理技术，用于一次处理多个 Redis 命令，从而提高整个交互的性能。

# 为什么Redis 不支持事务回滚？

Redis 中并没有提供回滚机制，虽然 Redis 提供了 DISCARD 命令，但是这个命令只能用来主动放弃事务执行，把暂存的命令队列清空，起不到回滚的效果。

Redis 并不一定保证原子性

- 不支持事务回滚是因为这种复杂的功能和 Redis 追求的简单高效的设计主旨不符合。

# 如何用 Redis 实现分布式锁的？

分布式锁是用于分布式环境下并发控制的一种机制，用于控制某个资源在同一时刻只能被一个应用所使用。

Redis 本身可以被多个客户端共享访问，正好就是一个共享存储系统，可以用来保存分布式锁，而且 Redis 的读写性能高，可以应对高并发的锁操作场景。

<font color="red">Redis 锁主要利用 Redis 的 setnx 命令</font>。

- 加锁命令：SETNX key value，当键不存在时，对键进行设置操作并返回成功，否则返回失败。KEY 是锁的唯一标识，一般按业务来决定命名。
- 解锁命令：DEL key，通过删除键值对释放锁，以便其他线程可以通过 SETNX 命令来获取锁。
- 锁超时：EXPIRE key timeout, 设置 key 的超时时间，以保证即使锁没有被显式释放，锁也可以在一定时间后自动释放，避免资源被永远锁住。

```Bash
if (setnx(key, 1) == 1){
    expire(key, 30)
    try {
        //TODO 业务逻辑
    } finally {
        del(key)
    }
}
```

<font color="red">SETNX 和 EXPIRE 非原子性, 不能删掉别人的东西</font>。

如果 SETNX 成功，在设置锁超时时间后，服务器挂掉、重启或网络问题等，导致 EXPIRE 命令没有执行，锁没有设置超时时间变成死锁。

通过在 value 中设置当前线程加锁的标识，在删除之前验证 key 对应的 value 判断锁是否是当前线程持有。可生成一个 UUID 标识当前线程，使用 lua 脚本做验证标识和解锁操作。

```Go
// 加锁
String uuid = UUID.randomUUID().toString().replaceAll("-","");
SET key uuid NX EX 30
// 解锁
if (redis.call('get', KEYS[1]) == ARGV[1])
    then return redis.call('del', KEYS[1])
else return 0
end
```

# Lua

API: https://redis.io/docs/interact/programmability/lua-api/

# 限速器

`go-redis/redis_rate `库实现了一个漏桶调度算法（又名通用信元速率算法）。

# 遍历 Key

 SCAN 来迭代遍历

```Go
var cursor uint64
for {
        var keys []string
        var err error
        keys, cursor, err = rdb.Scan(ctx, cursor, "prefix:*", 0).Result()
        if err != nil {
                panic(err)
        }

        for _, key := range keys {
                fmt.Println("key", key)
        }

        // 没有更多key了
        if cursor == 0 {
                break
        }
}
```

## Cluster 和 Ring

如果你使用的是 Redis Cluster 或 Redis Ring，需要分别扫描集群每个节点：

```go
err := rdb.ForEachMaster(ctx, func(ctx context.Context, rdb *redis.Client) error {
	iter := rdb.Scan(ctx, 0, "prefix:*", 0).Iterator()

	...

	return iter.Err()
})
if err != nil {
	panic(err)
}
```

## 删除无过期时间的 Key

```go
iter := rdb.Scan(ctx, 0, "", 0).Iterator()

for iter.Next(ctx) {
	key := iter.Val()

    d, err := rdb.TTL(ctx, key).Result()
    if err != nil {
        panic(err)
    }

    if d == -1 { // -1 means no TTL
        if err := rdb.Del(ctx, key).Err(); err != nil {
            panic(err)
        }
    }
}

if err := iter.Err(); err != nil {
	panic(err)
}
```

