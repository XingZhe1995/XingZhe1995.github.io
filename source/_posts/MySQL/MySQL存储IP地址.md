---
title: MySQL存储IP地址
categories:
  - MySQL
abbrlink: e38e4cbe
date: 2019-10-26 17:07:44
tags:
  - 数据库
  - MySQL
---


在MySQL中，没有专门用于存储IP地址的数据类型，但是可以使用inet_aton()函数把IP地址转换成整型数值进行存储，使用inet_ntoa()函数把整型数值转换回IP地址。

<!-- more -->

# inet_aton()和inet_ntoa()函数

inet_aton()和inet_ntoa()是MySQL提供的一对函数，一般情况下都是互相配合使用的：

* inet_aton()：把IP地址转换成一个整型数值，其中的aton可以理解成 IP Address->Number
* inet_ntoa()：把整型数值转换成IP地址，其中的ntoa可以理解成Number->IP Address

具体的流程：inet_aton(IP地址)—>整型数值，inet_ntoa(整型数值)->IP地址

``` shell
> select inet_aton('255.255.255.255');
> 4294967295

> select inet_ntoa('4294967295');
> 255.255.255.255

> select inet_aton('0.0.0.0');  
> 0

> select inet_ntoa('0');
> 0.0.0.0
```

# 性能

在不知道inet_aton()和inet_ntoa()函数存在的情况下，我们会想到使用varchar解决IP地址长度不固定的存储问题。但是为了更高的性能，MySQL为我们提供了专用的函数。

> IP地址范围：0.0.0.0 - 255.255.255.255

根据IP地址的范围，使用varchar类型，存储IP地址的最小值（0.0.0.0）要占用7个字符，存储IP地址的最大值（255.255.255.255）要占用15个字符，按照一般使用UTF-8编码计算，存储占用在**22~46个字节**之间。

使用inet_aton()函数，IP地址的最大值（255.255.255.255）转换后产生的整型数值为*4294967295*，即只需要一个int类型即可存储，而一个int类型数据在数据库中只占用**4个字节**。

通过以上的分析可以了解到，使用函数对IP地址转换后进行存储比直接存储字符串在存储空间上要节省得多，其次在运算上也有明显的优势。

注意：因为没有在真实的场景中用IP地址进行过运算，这里没法给出例子，等以后以后遇到了再回来修改！
