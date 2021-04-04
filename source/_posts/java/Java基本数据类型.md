---
title: Java基本数据类型
tags:
  - Java
categories:
  - Java
abbrlink: c80e1ec8
date: 2021-04-04 16:32:14
---

要用Java必然绕不开它的基本数据类型，



<!-- more -->



# 8种基本数据类型

Java的基本数据类型分为数值型、布尔值和字符三个大类，共计8种基本数据类型：byte、short、int、long、float、double、boolean、char。

![Java的8种基本数据类型](/images/Java的8种基本数据类型.png)


基本数据类型对应的位数、字节和默认值如下

| 基本数据类型 | 位数 | 字节 | 默认值 |
| ------------ | ---- | ---- | ------ |
| byte         | 8    | 1    | 0      |
| short        | 16   | 2    | 0      |
| int          | 32   | 4    | 0      |
| long         | 64   | 8    | 0L     |
| float        | 32   | 4    | 0f     |
| double       | 64   | 8    | 0d     |
| char         | 16   | 2    | u0000  |
| boolean      | 1    |      | false  |

注：对于boolean，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1 位，但是实际中会考虑计算机高效存储因素。