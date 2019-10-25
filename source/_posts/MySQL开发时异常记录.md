---
title: MySQL开发时异常记录
categories:
  - MySQL
abbrlink: a7ec3638
date: 2019-10-25 23:03:02
tags:
---


记录开发过程中遇到过的MySQL异常。

<!-- more -->

# Unknown error 1045

数据库的账号密码输入错误。

# Data truncation: #22001

``` java
Cause: com.mysql.cj.jdbc.exceptions.MysqlDataTruncation: Data truncation: #22001
```

插入的数据值范围超过了字段在数据库中定义的范围。

# The server time zone value '�й���׼ʱ��' is unrecognized or represents more than one time zone

在较新版本的*mysql-connector-java*里，需要在连接数据库的url上加入*serverTimeZone*参数

``` java
# 需要注意serverTimeZone的大小写
jdbc:mysql://localhost:3306/test?serverTimeZone=serverTimeZone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8
```

一般情况使用参数*serverTimeZone=Asia/Shanghai*，代表中国的时区（UTC+8）

如果有多国的时差问题，直接使用*serverTimeZone=UTC*，即全球标准时间

可用参数参考：[mysql serverTimezone](https://blog.csdn.net/Shezzer/article/details/80201264)

# java.sql.SQLException: #HY000

字段在数据库中定义为非空字段，但是插入数据时该字段对应的值却为空。

