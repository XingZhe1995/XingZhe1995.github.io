---
title: Gogs配置为服务开机自启动
tags:
  - Gogs
categories:
  - 开发
  - 开发工具
  - Gogs
abbrlink: bdb89eee
date: 2021-03-27 21:09:44
---


`Gogs是一款极易搭建的自助 Git 服务`，安装起来也确实是这样，在安装目录下输入命令`./gogs web`就能启动Gogs服务，但如果是通过二进制方式进行安装每次都要输入命令拉起服务就很不方便了，因此需要手动把Gogs配置为Linux服务并开机自启动。

<!-- more -->

**注：假设当前的操作用户是git（为啥要用git用户下文细说），安装目录是/home/git/gogs ！！！**



# 环境

系统：CentOS 7

Gogs版本：0.12.3



# 配置Gogs服务

要配置为Linux服务，先要写一个配置脚本，而这一步Gogs已经贴心的帮我们完成了，放在了如下的位置

``` shell
/home/git/gogs/scripts/systemd/gogs.service
```

然后复制到`/usr/lib/systemd/system/`目录下

```shell
cp /home/git/gogs/scripts/systemd/gogs.service /usr/lib/systemd/system/
```

执行如下命令，让Linux重新加载服务的配置信息

```
systemctl daemon-reload
```

如果不执行这一步会弹出警告信息

``` shell
Warning: gogs.service changed on disk. Run 'systemctl daemon-reload' to reload units.
```

到了这里就可以正常使用了。



# Gogs服务自启动

配置好Gogs服务，剩下的就和其它的Linux服务没区别，直接用`systemctl`命令来控制就可以了。

开启自启动

```shell
systemctl enable gogs
```

关闭自启动

```shell
systemctl disable gogs
```

手动启动服务

```shell
systemctl start gogs
```

手动关闭服务

```shell
systemctl stop gogs
```

查看服务状态

``` shell
systemctl status gogs
```



# Gogs默认用户git

翻看一下Gogs安装目录下的脚本可以发现，很多地方的默认用户都是git，例如上文提到的`gogs.service`脚本

``` shell
# 打开gogs.service脚本
vi /home/git/gogs/scripts/systemd/gogs.service

#这里只截取了部分内容
[Service]
# Modify these two values and uncomment them if you have
# repos with lots of files and get an HTTP error 500 because
# of that
###
#LimitMEMLOCK=infinity
#LimitNOFILE=65535
Type=simple
User=git
Group=git
WorkingDirectory=/home/git/gogs
ExecStart=/home/git/gogs/gogs web
Restart=always
Environment=USER=git HOME=/home/git
```

因此怕麻烦不想修改脚本的，可以考虑新建一个git用户，不会新建的看这里[Linux操作指南：01-新建用户](https://www.zhixing.icu/archives/linux-cao-zuo-zhi-nan-01--xin-jian-yong-hu)。

