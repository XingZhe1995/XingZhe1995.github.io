---
title: 修改Linux网络配置
tags:
  - Linux
categories:
  - 开发
  - Linux
abbrlink: d2906cb4
date: 2021-04-07 12:53:26
---


每一次使用Linux总是忘记了怎么配置网络，因此特意在这里做一下记录。

<!-- more -->

# ip命令

> ip - show/manipulate routing、devices、policy routing and tunnels

展示或操纵路由、设备、路由策略和隧道。

要修改Linux网络配置，首先要找到自己当前机器上的网卡设备。

注意：这里博主使用的是CentOS7，所以命令是ip，如果使用的是CentOS6的话需要使用ifconfig

``` shell
# CentOS 7
ip address

# CentOS 6
ifconfig
```

可以看到当前有两个网卡：

* lo：代表的是本地的循回地址

* ens33：代表的是自己机器上的网卡，这在不同的机器上有不同的名字。

这时候就需要修改网卡（ens33）对应的网络配置文件了

找到网络配置文件的存放地方，存放的地方是/etc/sysconfig/network-scripts/，配置文件命名格式：ifcfg-网卡名，例如ifcfg-ens33

``` shell
ls -l /etc/sysconfig/network-scripts/
```

使用vi修改文件

``` shell
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

首先说说每个参数

* TYPE：代表了该网卡的类型，Ethernet即以太网

* BOOTPROTO：代表了网络的类型，dhcp即动态IP，static即静态IP

* ONBOOT：开机就运行 yes-是，no-否

# 配置动态获取IP地址

设置配置文件

```
TYPE=Ethernet
ONBOOT=yes
BOOTPROTO=dhcp
```
然后重启网络，修改的配置就会生效。

``` shell
# CenttOS 6
service network restart

# CentOS 7
systemctl restart network
```

以上的方式是最简单的，但是有时候也会带来不便，因为有时侯需要IP地址是固定的

# 配置静态IP地址

打开网卡配置文件，修改

``` shell
BOOTPROTO=static
IPADDR=192.168.14.200 #想固定的IP地址
GATEWAY＝192.168.14.1 # 网关
```

重启网络即可生效
