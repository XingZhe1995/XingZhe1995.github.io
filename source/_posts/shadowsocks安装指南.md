---
title: shadowsocks安装指南
abbrlink: ee1d977a
date: 2019-08-06 10:27:59
tags:
  - shadowsocks
categories:
  - 工具
  - shadowsocks
---



有时候想访问国外的网站，但是却访问不了，这时候就需要使用代理访问的方式进行访问。*shadowsocks*就是能提供这么一种服务的工具。



<!-- more -->

# shadowsocks服务

要使用shadowsocks，首先需要一台安装了shadowsokcs服务的服务器，有两种获得方法：

* 购买别人的shadowsokcs服务
* 自建shadowsokcs服务

第一种很好理解，就是花钱买服务，优点是省心而且有多个服务器进行网速优化，但是缺点也很明显：价格偏高、市场上提供的服务质量参差不齐，且容易跑路（建议一个月一个月的买），这部分博主不太熟悉，只能读者们自己去搜索相关信息了，在这里主要说的第二种方法。

要自建shadowsocks服务，先要有一台服务器，需要注意的是这台服务器一定要在国外的，起码要在香港、澳门这些地方，因为一般来说使用shadowsocks主要是访问国内无法访问的网站，所以如果服务器在国内的话就无法访问了。



# 版本说明

shadowsokcs最开始是使用python进行开发的，后续有许多贡献者开发出了不同的版本，我见过的版本如下：

* shadowsocks-libev：c语言开发，内存占用少，没有多用户管理的功能，其它更多的功能不清楚，我一直用的是这个版本。
* shadowsocks-python：python语言开发，应该是最初的版本一直开发下来，能进行多用户管理。
* shadowsocks-go：go语言开发，这个没用过，请自行搜索。



# 一键安装脚本

安装shadowsocks-libev版本

```shell
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh
chmod +x shadowsocks-libev.sh
./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
```

多语言合一版（包含：libev、python、go）

```shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```
