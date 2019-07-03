# no sql

当下互联网的需求：高并发，高性能，高可扩

**非关系型数据库:**

memcache,redis,tair

## NoSQL数据库的四大分类

1. 文档型数据库（bson格式较多）mongodb

2. 列存储数据库(分布式文件系统) Cassandra,HBase

3. 图关系数据库

   注意他不是放图形的，放的是关系

   Noe4J,infoGrid

4. KV键值：redis,memcache

## NoSQL数据库的CAP原理+BASE

### 传统的ACID是什么

- Atomicity 原子性
- Consistency 一致性
- Isolation 独立性
-  Durability 持久性

### NoSQL中的CAP是什么

- Consistency 强一致性
- Avaliability 可用性
- Partition tolerance 分区容错性

**没有NoSQL数据库能够同时满足上述三个特性**

**大多数的网站架构的选择是满足AP(高可用和分区容错性)**

**传统的oracle数据库满足的是CA（一致性和高可用）**

**满足CP特性的redis和Mongodb**

### BASE

**BASE**是为了解决关系型数据库强一致性引起的问题而引起的可用性降低的而提出的解决方案。

它其实是下面三个术语的缩写：

1. 基本可用 Basically Available
2. 软状态 soft state
3. 最终一致 Eventually consistent

### redis数据库的特点

1. redis支持数据的持久化，可以将内在中的数据保存在磁盘中，在重启的时候再次加载使用。
2. redis不仅支持简单的key-value类型的数据，还支持list,set,zset,hash等数据结构的使用。
3. redis支持数据的备份，即master/slave模式的数据备份.

### redis数据库的作用

1.  内存存储和数据持久化：redis支持异步将内在中的数据写到硬盘上，同时不影响继续服务
2. 取最新的n个数据的操作，例如可以将最新的10条评论的id放在redis里面的list集合里面
3. 支持httpsession这种需要设定过期时间的功能 
4. 发布、订阅消息系统
5. 定时器，计数器