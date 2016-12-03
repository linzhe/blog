---
title:  cache server： 冷热数据切换
date: 2016-07-14 19:43:00
tags:
  - leveldb
  - redis
  - cache_server 
category:
  - cache_server
---

### 背景
leveldb是一个Google开发的高性能的字符串类型的K-V存储C/C++类库，其详细介绍可参考[Github主页](https://github.com/google/leveldb)，刚开始工作时使用过一段时间，用于存储爬虫系统爬取的大量网页数据。但是后来就一直没有使用过。
最近在做推荐时，发现user profile的数据实在太大，即使有类似于Redis集群1T的内存也没法将用户所有有数据cache下来，一直想着能不能做个冷热的备件系统加大对user profile数据存储数量，结合leveldb和redis。

### leveldb的优势
LevelDB内部采用连续的块存储数据，充分发挥了顺序磁盘I/O的性能，并且运用了现代操作系统里的高性能缓冲区管理。这样的结构正好迎合了现代内存的层次式结构， 避免了与产生高性能的操作系统决策之间的冲突。

### redis
Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。
Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
从3.0版本开始，Redis支持数据的备份，即master-slave模式的数据备份。

### 冷热数据
因为redis强劲的性能，与leveldb互相配合，将冷热数据分开存储，互相交换数据，以达到冷热切换的目的。

