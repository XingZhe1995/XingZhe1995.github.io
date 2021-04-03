---
title: Git常见使用问题
tags:
  - Git
categories:
  - 工具
  - 开发工具
  - Git
abbrlink: 36fb1fe
date: 2021-04-03 09:38:02
---



Git是使用频率非常高的一个工具，在使用过程中遇到过不少的问题，现在汇总一下遇到过的问题以及对应的解决方法。



<!-- more -->



# 无法访问Github

Github服务器位于国外，因此经常出现无法访问的情况，影响了代码的拉取和提交等。要解决只能使用代理或者修改hosts文件。

注：值得一提的就是，如果拉取代码的速度慢，也可以考虑通过设置代理来提高下载速度。


## 使用代理

使用代理是比较常见的方法，对于Github网页的访问，直接运行代理软件就可以了，对于Git工具就需要进行配置，对应的代理配置项是*http.proxy*和*https.proxy*

查看当前代理设置

``` shell
# 查看全局代理配置
git config --global http.proxy
git config --global https.proxy

# 查看当前项目代理配置
git config --local http.proxy
git config --local https.proxy
```

设置代理配置

``` shell
# 设置全局代理配置
# http代理
git config --global http.proxy 'http://127.0.0.1:1080'
git config --global https.proxy 'https://127.0.0.1:1080'

# 设置当前项目代理配置
# http代理
git config --local http.proxy 'http://127.0.0.1:1080'
git config --local https.proxy 'https://127.0.0.1:1080'
```

删除代理配置

``` shell
# 删除全局代理配置
git config --global --unset http.proxy
git config --global --unset https.proxy

# 删除当前项目代理配置
git config --local --unset http.proxy
git config --local --unset https.proxy
```



## 修改hosts文件

这样方法没用过，请自行搜索解决方案。



# 不支持具有 socks5 方案的代理

Git提交代码到远程服务器，虽然提交成功，但显示异常信息，异常信息如下

``` shell
fatal: NotSupportedException encountered.
   ServicePointManager 不支持具有 socks5 方案的代理。
```

关键词：*socks5*和*代理*，这两个词有点熟悉，就是代理设置时经常遇到的字眼，先查看git配置

``` shell
git config --global -l

# 显示信息如下
......
http.proxy=socks5://127.0.0.1:1080
https.proxy=socks5://127.0.0.1:1080
```

问题就是这里了，*http.proxy*和*https.proxy*支持的都是http协议，不能使用socks协议。

因此需要改回使用http协议的代理

``` shell
git config --global http.proxy 'http://127.0.0.1:1080'
git config --global https.proxy 'https://127.0.0.1:1080'
```

或者考虑不使用代理

``` shell
git config --global --unset http.proxy
git config --global --unset https.proxy
```



# 拒绝访问 Connection refused

提交代码到远程时遇到如下问题

``` shell
fatal: unable to access 'https://github.com/xxxxxxxxx.git/': Failed to connect to 127.0.0.1 port 1080: Connection refused
```

这个问题很明显就是配置了代理信息，但是又忘记运行代理软件，导致无法连通远程服务器。