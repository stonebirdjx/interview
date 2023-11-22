<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [什么是NOSQL](#%E4%BB%80%E4%B9%88%E6%98%AFnosql)
- [MongoDB 是什么？](#mongodb-%E6%98%AF%E4%BB%80%E4%B9%88)
- [MongoDB 的存储结构是什么？](#mongodb-%E7%9A%84%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84%E6%98%AF%E4%BB%80%E4%B9%88)
- [MongoDB 支持哪些存储引擎？](#mongodb-%E6%94%AF%E6%8C%81%E5%93%AA%E4%BA%9B%E5%AD%98%E5%82%A8%E5%BC%95%E6%93%8E)
- [WiredTiger 基于 LSM Tree 还是 B+ Tree？](#wiredtiger-%E5%9F%BA%E4%BA%8E-lsm-tree-%E8%BF%98%E6%98%AF-b-tree)
- [MongoDB 聚合有什么用？](#mongodb-%E8%81%9A%E5%90%88%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8)
- [MongoDB 提供了哪几种执行聚合的方法？](#mongodb-%E6%8F%90%E4%BE%9B%E4%BA%86%E5%93%AA%E5%87%A0%E7%A7%8D%E6%89%A7%E8%A1%8C%E8%81%9A%E5%90%88%E7%9A%84%E6%96%B9%E6%B3%95)
- [MongoDB 事务](#mongodb-%E4%BA%8B%E5%8A%A1)
- [MongoDB 支持哪些类型的索引？](#mongodb-%E6%94%AF%E6%8C%81%E5%93%AA%E4%BA%9B%E7%B1%BB%E5%9E%8B%E7%9A%84%E7%B4%A2%E5%BC%95)
- [复合索引遵循左前缀原则吗？](#%E5%A4%8D%E5%90%88%E7%B4%A2%E5%BC%95%E9%81%B5%E5%BE%AA%E5%B7%A6%E5%89%8D%E7%BC%80%E5%8E%9F%E5%88%99%E5%90%97)
- [什么是 TTL 索引？](#%E4%BB%80%E4%B9%88%E6%98%AF-ttl-%E7%B4%A2%E5%BC%95)
- [MongoDB 高可用](#mongodb-%E9%AB%98%E5%8F%AF%E7%94%A8)
  - [复制集群](#%E5%A4%8D%E5%88%B6%E9%9B%86%E7%BE%A4)
  - [分片集群](#%E5%88%86%E7%89%87%E9%9B%86%E7%BE%A4)
- [为什么要用分片集群？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E7%94%A8%E5%88%86%E7%89%87%E9%9B%86%E7%BE%A4)
  - [什么是分片键？](#%E4%BB%80%E4%B9%88%E6%98%AF%E5%88%86%E7%89%87%E9%94%AE)
  - [分片数据如何存储？](#%E5%88%86%E7%89%87%E6%95%B0%E6%8D%AE%E5%A6%82%E4%BD%95%E5%AD%98%E5%82%A8)
- [分析器在MongoDB中的作用是什么?](#%E5%88%86%E6%9E%90%E5%99%A8%E5%9C%A8mongodb%E4%B8%AD%E7%9A%84%E4%BD%9C%E7%94%A8%E6%98%AF%E4%BB%80%E4%B9%88)
- [更新操作立刻fsync到磁盘?](#%E6%9B%B4%E6%96%B0%E6%93%8D%E4%BD%9C%E7%AB%8B%E5%88%BBfsync%E5%88%B0%E7%A3%81%E7%9B%98)
- [Mongo 正在使用的链接?](#mongo-%E6%AD%A3%E5%9C%A8%E4%BD%BF%E7%94%A8%E7%9A%84%E9%93%BE%E6%8E%A5)
- [如何理解MongoDB中的GridFS机制](#%E5%A6%82%E4%BD%95%E7%90%86%E8%A7%A3mongodb%E4%B8%AD%E7%9A%84gridfs%E6%9C%BA%E5%88%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 什么是NOSQL

NoSQL是非关系型数据库，NoSQL = Not Only SQL。

> MongoDB、redis

# MongoDB 是什么？

MongoDB 是一个基于 **分布式文件存储** 的开源 NoSQL 数据库系统，由 **C++** 编写的。MongoDB 提供了 **面向文档** 的存储方式，

# MongoDB 的存储结构是什么？

> BSON [bee·sahn] 是 Binary JSON的简称，是 JSON 文档的二进制表示

**文档（Document）**：MongoDB 中最基本的单元，由 BSON 键值对（key-value）组成，类似于关系型数据库中的行（Row）。

**集合（Collection）**：一个集合可以包含多个文档，类似于关系型数据库中的表（Table）。

**数据库（Database）**：一个数据库中可以包含多个集合，可以在 MongoDB 中创建多个数据库，类似于关系型数据库中的数据库（Database）。

# MongoDB 支持哪些存储引擎？

**WiredTiger 存储引擎**：自 MongoDB 3.2 以后。非常适合大多数工作负载，建议用于新部署。WiredTiger 提供文档级并发模型、检查点和数据压缩（后文会介绍到）等功能。

**In-Memory 存储引擎**：它不是将文档存储在磁盘上，而是将它们保留在内存中以获得更可预测的数据延迟。

# WiredTiger 基于 LSM Tree 还是 B+ Tree？

目前绝大部分流行的数据库存储引擎都是基于 B/B+ Tree 或者 LSM(Log Structured Merge) Tree 来实现的。对于 NoSQL 数据库来说，绝大部分（比如 HBase、Cassandra、RocksDB）都是基于 LSM 树，MongoDB 不太一样。

WiredTiger 使用的是 B+ 树作为其存储结构

# MongoDB 聚合有什么用？

我们经常需要将多个文档甚至是多个集合汇总到一起计算分析（比如求和、取最大值）并返回计算后的结果，这个过程被称为 **聚合操作** 

我们可以使用聚合操作来：

- 将来自多个文档的值组合在一起。
- 对集合中的数据进行的一系列运算。
- 分析数据随时间的变化。

# MongoDB 提供了哪几种执行聚合的方法？

**聚合管道（Aggregation Pipeline）**：执行聚合操作的首选方法。

**单一目的聚合方法（Single purpose aggregation methods）**：也就是单一作用的聚合函数比如 `count()`、`distinct()`、`estimatedDocumentCount()`。

绝大部分文章中还提到了 **map-reduce** 这种聚合方法。不过，从 MongoDB 5.0 开始，map-reduce 已经不被官方推荐使用了

```bash
db.orders.aggregate([
   # 第一阶段：$match阶段按status字段过滤文档，并将status等于"A"的文档传递到下一阶段。
    { $match: { status: "A" } },
  # 第二阶段：$group阶段按cust_id字段将文档分组，以计算每个cust_id唯一值的金额总和。
    { $group: { _id: "$cust_id", total: { $sum: "$amount" } } }
])

```

# MongoDB 事务

与关系型数据库一样，MongoDB 事务同样具有 ACID 特性：

- **原子性**（`Atomicity`）：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
- **一致性**（`Consistency`）：执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的；
- **隔离性**（`Isolation`）：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的。WiredTiger 存储引擎支持读未提交（ read-uncommitted ）、读已提交（ read-committed ）和快照（ snapshot ）隔离，MongoDB 启动时默认选快照隔离。在不同隔离级别下，一个事务的生命周期内，可能出现脏读、不可重复读、幻读等现象。
- **持久性**（`Durability`）：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

# MongoDB 支持哪些类型的索引？

**单字段索引：** 建立在单个字段上的索引，索引创建的排序顺序无所谓，MongoDB 可以头/尾开始遍历。

**复合索引：** 建立在多个字段上的索引，也可以称之为组合索引、联合索引。

**多键索引**：MongoDB 的一个字段可能是数组，在对这种字段创建索引时，就是多键索引。MongoDB 会为数组的每个值创建索引。就是说你可以按照数组里面的值做条件来查询，这个时候依然会走索引。

**哈希索引**：按数据的哈希值索引，用在哈希分片集群上。

**文本索引：** 支持对字符串内容的文本搜索查询。文本索引可以包含任何值为字符串或字符串元素数组的字段。一个集合只能有一个文本搜索索引，但该索引可以覆盖多个字段。MongoDB 虽然支持全文索引，但是性能低下，暂时不建议使用。

**地理位置索引：** 基于经纬度的索引，适合 2D 和 3D 的位置查询。

**唯一索引**：确保索引字段不会存储重复值。如果集合已经存在了违反索引的唯一约束的文档，则后台创建唯一索引会失败。

**TTL 索引**：TTL 索引提供了一个过期机制，允许为每一个文档设置一个过期时间，当一个文档达到预设的过期时间之后就会被删除。

# 复合索引遵循左前缀原则吗？

**MongoDB 的复合索引遵循左前缀原则**：拥有多个键的索引，可以同时得到所有这些键的前缀组成的索引，但不包括除左前缀之外的其他子集。

# 什么是 TTL 索引？

TTL 索引提供了一个过期机制，允许为每一个文档设置一个过期时间 `expireAfterSeconds` ，当一个文档达到预设的过期时间之后就会被删除。TTL 索引除了有 `expireAfterSeconds` 属性外，和普通索引一样。

# MongoDB 高可用

## 复制集群

MongoDB 的复制集群又称为副本集群，是一组维护相同数据集合的 mongod 进程。

一个复制集群包含 1 个主节点（Primary），多个从节点（Secondary）以及零个或 1 个仲裁节点（Arbiter）。

**主节点**：整个集群的写操作入口，接收所有的写操作，并将集合所有的变化记录到操作日志中，即 oplog。主节点挂掉之后会自动选出新的主节点。

**从节点**：从主节点同步数据，在主节点挂掉之后选举新节点。不过，从节点可以配置成 0 优先级，阻止它在选举中成为主节点。

**仲裁节点**：这个是为了节约资源或者多机房容灾用，只负责主节点选举时投票不存数据，保证能有节点获得多数赞成票。

主节点与备节点之间是通过 **oplog（操作日志）** 来同步数据的。oplog 是 local 库下的一个特殊的 **上限集合(Capped Collection)** ，用来保存写操作所产生的增量日志，类似于 MySQL 中 的 Binlog。

## 分片集群

相较副本集，分片集群数据被均衡的分布在不同分片中， 不仅大幅提升了整个集群的数据容量上限，也将读写的压力分散到不同分片，以解决副本集性能瓶颈的难题。

- **Config Servers**：配置服务器，本质上是一个 MongoDB 的副本集，负责存储集群的各种元数据和配置，如分片地址、Chunks 等
- **Mongos**：路由服务，不存具体数据，从 Config 获取集群配置讲请求转发到特定的分片，并且整合分片结果返回给客户端。
- **Shard**：每个分片是整体数据的一部分子集，从 MongoDB3.6 版本开始，每个 Shard 必须部署为副本集（replica set）架构

# 为什么要用分片集群？

随着系统数据量以及吞吐量的增长，常见的解决办法有两种：垂直扩展和水平扩展。

- 垂直扩展通过增加单个服务器的能力来实现，比如磁盘空间、内存容量、CPU 数量等；水平扩展则通过将数据存储到多个服务器上来实现，根据需要添加额外的服务器以增加容量。
- 水平扩展，扩展shard。这种方式更灵活，可以满足更大数据量的存储需求，支持更高吞吐量。

## 什么是分片键？

**分片键（Shard Key）** 是数据分区的前提， 从而实现数据分发到不同服务器上，减轻服务器的负担。也就是说，分片键决定了集合内的文档如何在集群的多个分片间的分布状况。

## 分片数据如何存储？

**Chunk（块）** 是 MongoDB 分片集群的一个核心概念，其本质上就是由一组 Document 组成的逻辑数据单元。每个 Chunk 包含一定范围片键的数据，互不相交且并集为全部数据，即离散数学中**划分**的概念。

# 分析器在MongoDB中的作用是什么?

MongoDB中包括了一个可以显示数据库中每个操作性能特点的数据库分析器。通过这个分析器你可以找到比预期慢的查询(或写操作);利用这一信息，比如，可以确定是否需要添加索引。

# 更新操作立刻fsync到磁盘?

不会，磁盘写操作默认是延迟执行的。写操作可能在两三秒(默认在60秒内)后到达磁盘。例如，如果一秒内数据库收到一千个对一个对象递增的操作，仅刷新磁盘一次。(注意，尽管fsync选项在命令行和经过getLastError_old是有效的)

# Mongo 正在使用的链接?

```go
db._adminCommand("connPoolStats");
```

# 如何理解MongoDB中的GridFS机制

GridFS是一种将大型文件存储在MongoDB中的文件规范。使用GridFS可以将大文件分隔成多个小文档存放，这样我们能够有效的保存大文档，而且解决了BSON对象有限制的问题。