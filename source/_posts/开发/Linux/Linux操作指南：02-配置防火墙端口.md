---
title: Linux操作指南：02-配置防火墙端口
tags:
  - Linux
categories:
  - 开发
  - Linux
abbrlink: c62f803c
date: 2021-03-27 21:10:14
---


新装的Linux系统，ssh、nginx、tomcat等各种服务都安装好了，可是在外网却访问不了？？这十有八九是防火墙的端口没有打开了。

<!-- more -->

# 环境

系统：CentOS 7



# firewall-cmd与iptables

到了CentOS 7，防火墙的操作命令由*iptables*改为了*firewall-cmd*了。以下是摘抄自[他人](https://wangchujiang.com/linux-command/c/firewall-cmd.html)的解释：

> firewall-cmd 是 firewalld的字符界面管理工具，firewalld是centos7的一大特性，最大的好处有两个：支持动态更新，不用重启服务；第二个就是加入了防火墙的“zone”概念。
>
> firewalld跟iptables比起来至少有两大好处：
>
> 1. firewalld可以动态修改单条规则，而不需要像iptables那样，在修改了规则后必须得全部刷新才可以生效。
> 2. firewalld在使用上要比iptables人性化很多，即使不明白“五张表五条链”而且对TCP/IP协议也不理解也可以实现大部分功能。
>
> firewalld自身并不具备防火墙的功能，而是和iptables一样需要通过内核的netfilter来实现，也就是说firewalld和 iptables一样，他们的作用都是用于维护规则，而真正使用规则干活的是内核的netfilter，只不过firewalld和iptables的结 构以及使用方法不一样罢了。

总的来说这次变更带来的直观好处就是变的更方便更好用了。



# 端口开发的两种方式

开放端口有两种方式：指定端口和指定服务。

指定端口很好理解，就是写上那个端口号就开放那个端口。

指定服务可以理解为内置的一个端口映射，默认情况下：ssh服务端口是22、http服务端口是80、MySQL服务端口是3306、tomcat服务端口是8080，因此用服务名来代替直接指定端口，方便记忆和使用。

**要注意的是：通过指定服务名开放的就要通过指定服务名关闭；通过指定端口号开放的就要通过指定端口号关闭，且指定端口的时候一定要指定是什么协议，tcp 还是 udp。**



# permanent参数和zone参数

配置端口的时候这两个参数是会经常使用到的。

``` shell
firewall-cmd --permanent
```

这个很好理解，就是让配置永久生效的意思。



``` shell
firewall-cmd --zone=public
```

**zone**参数就难以理解一点，这个参数的作用是指定一套规则集合，依靠这些规则来判断是否放行数据。这里使用的规则集是**public**，即只放行已配置的服务（端口）。

更详细的可以参考：[Firewalld的结构](http://www.excelib.com/article/287/show/#g5vTC3)，[用活Firewalld防火墙中的zone](https://www.cnblogs.com/excelib/p/5155951.html)



# firewall-cmd常用命令

增加端口

``` shell
# 指定端口
firewall-cmd --zone=public --add-port=80/tcp --permanent

# 指定服务
firewall-cmd --add-service=http --permanent
```

移除端口

``` shell
# 指定端口
firewall-cmd --permanent --remove-port=80/tcp

# 指定服务
firewall-cmd --permanent --remove-service=http
```

显示防火墙运行状态

``` shell
firewall-cmd --state
```

仅显示打开的端口信息

``` shell
firewall-cmd --list-ports
```

仅显示增加的服务信息

``` shell
firewall-cmd --list-service
```

显示所有信息

``` shell
firewall-cmd --list-all
```

配置完之后，必须要重启防火墙让配置生效，如下命令的意思是不中断连接重新加载防火墙配置

``` shell
firewall-cmd --reload
```



# 总结

这里只是列举了自己常用的一些命令，更具体的可以参考以下文章

* [firewall-cmd详解](https://wangchujiang.com/linux-command/c/firewall-cmd.html)

* [Firewalld的结构](http://www.excelib.com/article/287/show/#g5vTC3)
* [用活Firewalld防火墙中的zone](https://www.cnblogs.com/excelib/p/5155951.html)

