---
title: Redis的淘汰策略
tags:
  - Redis
categories:
  - 开发
  - 缓存
  - Redis
abbrlink: e0bb8399
date: 2021-03-29 20:59:27
---


redis共有8种淘汰策略。具体可分成3种类型。

<!-- more -->

# 淘汰策略

类型一
no-eviction：禁止淘汰数据，当内存不足无法写入新数据时会报错，真实项目中应该没有人会使用。

类型二（对所有数据而言）
allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
allkeys-lru（least recently used）：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）
allkeys-lfu（least frequently used）：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的 key

类型三（对于设置了过期时间的数据而言）
volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
volatile-ttl（time to live）：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
volatile-lru（least recently used）：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
volatile-lfu（least frequently used）：从已设置过期时间的数据集(server.db[i].expires)中挑选最不经常使用的数据淘汰

其中volatile-lfu和allkeys-lfu是4.0之后才加入的策略。

# 关于LFU和LRU的理解

LFU按访问频次排序，一个数据被访问则频次+1，淘汰时，把频次最低的淘汰。
LRU按访问时间排序，淘汰时，把访问时间最旧的淘汰。

# 总结

淘汰策略分为了可淘汰和不可淘汰两部分。可淘汰中根据目标数据范围分为了：所有数据和设置了过期时间 这两类，主要的淘汰策略是：随机（random）、最近最少使用（lru）和最不经常使用（lfu），对于设置了过期时间的数据而言还有一个策略就是挑选将要过期的数据（ttl）。

