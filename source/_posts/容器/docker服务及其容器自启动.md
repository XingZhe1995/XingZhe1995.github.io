---
title: docker服务及其容器自启动
tags:
  - 容器
  - Docker
categories:
  - Docker
abbrlink: f31ac7c3
date: 2021-03-27 21:08:56
---


服务器重启后docker服务和容器都会停止运行，如果每次都手动启动的话就很繁琐了，因此配置为自启动就很有必要了。

<!-- more -->

# 环境

服务器系统：centos7

docker 版本：19.03.5



# docker服务自启动

``` shell
# 开启自启动
systemctl enable docker

# 关闭自启动
systemctl disable docker
```

PS：systemctl就是直接操作系统服务，是service、chkconfig命令的组合，如果没有systemctl命令，请自行搜索其它方案。



# docker容器自启动

``` shell
# 创建容器前加入参数--restart
docker run --restart=参数项

# 已创建容器则使用如下命令
docker update --restart=参数项 [容器名]

# --restart参数解析
no：不要自动重启容器（默认）
on-failure：如果容器由于非正常退出（非零退出代码）而停止运行则重新启动容器，可以加入重启次数，如--restart=on-failure:3 即尝试3次后不再重新启动
always：总是重启容器，如果是手动停止，则仅在docker服务重启或重新手动启动该容器时才重新启动
unless-stopped：总是重启容器，但是如果容器是已经停止了（手动停止或其他方式），即使docker服务重新启动也不会重新启动容器

# 例子
docker update --restart=on-failure [容器名]
docker update --restart=on-failure:3 [容器名]
docker update --restart=always [容器名]
```

