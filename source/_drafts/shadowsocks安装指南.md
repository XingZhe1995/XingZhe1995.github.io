---
title: shadowsocks安装指南
draft: true
abbrlink: ee1d977a
date: 2019-08-06 10:27:59
tags:
categories:
  - 工具
---



<!-- more -->

# 前言

要使用shadowsocks，首先需要一台安装了shadowsokcs服务的服务器，有两种获得方法：

* 购买别人的shadowsokcs服务
* 自建shadowsokcs服务

第一种很好理解，就是花钱买服务，优点是省心而且有多个服务器进行网速优化，但是缺点也很明显：价格偏高、市场上提供的服务质量参差不齐，这部分博主不太熟悉，只能读者们自己去搜索相关信息了，在这里主要说的第二种方法。

要自建shadowsocks服务，先要有一台服务器，需要注意的是这台服务器一定要在国外的，最少要在香港、澳门这些地方，因为一般来说使用shadowsocks主要是访问国内无法访问的网站，所以如果服务器在国内的话就无法访问了。

# 版本说明

目前主流的shadowsokcs版本

* shadowsocks-libev
* shadowsocks-python
* shadowsocks-go

# 一键安装

安装shadowsocks-libev

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh
chmod +x shadowsocks-libev.sh
./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
```

多版本合一版

```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

# 相关资源
## 逗比
## 全球主机论坛
