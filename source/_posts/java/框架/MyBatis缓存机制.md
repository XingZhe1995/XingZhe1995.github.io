---
title: MyBatis缓存机制
tags:
  - MyBatis
categories:
  - Java
  - 框架
  - MyBatis
abbrlink: 201dbebe
date: 2021-04-03 22:31:18
---




MyBatis提供了强大的事务性查询缓存机制，用于提高查询性能！



<!-- more -->



MyBatis的缓存分为：本地缓存和二级缓存，另外还可以实现自己的缓存或者使用第三方的缓存机制。



# 本地缓存

MyBatis默认开启本地缓存，且无法手动关闭。

本地缓存属于会话级别的（sqlsession），生命周期与sqlsession一致，只对同一个sqlsession有效，不同sqlsession之间不共享缓存，

任何的修改操作（insert、update、delete）都会导致缓存清除，也可以通过配置使得查询前清空缓存。



# 二级缓存

二级缓存默认关闭，需要手动开启。

二级缓存属于全局级别的（sqlsessionfactory），能够跨会话访问，弥补本地缓存无法跨会话访问的缺陷，开启后缓存使用时是先访问二级缓存再访问一级缓存。



# 总结

对于MyBatis的缓存机制只是流于表面，还没有更多的深入了解，且实现自己的缓存或使用第三方缓存机制这些也没有涉略，因此该篇文章仅做记录用。