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