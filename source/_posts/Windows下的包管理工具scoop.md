---
title: Windows下的包管理工具scoop
tags:
  - scoop
categories:
  - 工具
  - scoop
abbrlink: b4c6dbe7
date: 2021-04-03 07:07:20
---




在Windows系统上，安装软件总是要到官网上去下载安装，又不想使用XX管家类的软件，因此如果有像Linux上的包管理工具就方便了，*scoop*就是Windows下的包管理工具！



<!-- more -->



# 安装

scoop的安装很简单，官方提供了执行脚本让我们使用了，具体可看官网[Installs in seconds](https://scoop.sh/)，步骤大概如下：

安装要求：

1. PowerShell 5 或更高的版本
2. .NET Framework 4.5 或更高的版本

安装命令如下

``` shell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

或者

``` shell
iwr -useb get.scoop.sh | iex
```

上面两条命令的效果是一样的。

如果安装时抛出异常，可以尝试如下命令修改执行策略，然后再尝试重新安装

``` shell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```



# scoop文件结构

scoop是安装在当前用户的用户文件夹下的，例如当前用户是test，则在Windows的位置如下

``` shell
C:\Users\test\scoop
```

scoop根目录下，有如下几个文件夹

* apps：应用安装区域，所有应用都会安装在这里，每个安装的软件都有一个以自己名字命名的文件夹，内部如下
  * 以版本命名的文件夹，该文件夹下存放的是指定版本的该软件
  * current：快捷方式，连接到当前使用的版本
* buckets：安装源
* cache：下载缓存
* persist：应用的配置文件放置地方
* shims：映射应用的bin中的文件，集中存放应用命令的链接或调用脚本，自动加入 path 路径中（自动进行兼容性处理），方便命令行的使用，且这样 path 的定义就会比较简洁，也容易管理。



# 常用操作

安装完成后就可以开始使用了，打开cmd命令行界面输入所需命令即可。

不知道怎么用？使用帮助，即可了解到所有的命令及其用法

``` shell
scoop help
```

注：下面以软件potplayer为例

输入关键字搜索想安装的软件，如果能安装则会显示可安装的软件列表，不能则会显示*Not Found*字眼

``` shell
scoop search potplayer
```

安装软件

``` shell
scoop install potplayer
```

卸载软件

``` shell
scoop uninstall potplayer
```

更新软件

``` shell
scoop update potplayer
```

更新scoop本身

``` shell
scoop update
```

查看已安装软件列表

``` shell
scoop list
```

查看当前的软件状态（显示当前的版本信息和可更新的版本信息）

``` shell
scoop status
```



# 添加bucket

bucket就是我们下载软件的源，查看scoop可以直接识别并添加的bucket

``` shell
scoop bucket known

# 显示如下
main # 默认就有，但收录条件严格，软件较少
extras # 这个最常用，软件最多
versions
nightlies
nirsoft
php
nerd-fonts
nonportable
java
games
jetbrains
```

添加bucket（这里以extras为例）

``` shell
scoop bucket add extras
```

展示已有的bucket

``` shell
scoop bucket list
```

移除bucket（这里以extras为例）

``` shell
scoop bucket rm extras
```



## 添加第三方bucket

上面添加的bucket是官方认证的，我们也可以添加自己的bucket或者别人制作的bucket，这里是一个按照 Github score（由 Star 数量、Fork 数量和 App 数量综合决定的 Github score）排列的 bucket 列表：[Scoop buckets by Github score](https://github.com/rasa/scoop-directory/blob/master/by-score.md)。对于第三方的操作稍微有些不一样：

添加第三方的bucket

``` shell
scoop bucket add <仓库名> <仓库地址>
```

安装第三方bucket的应用

``` shell
scoop install <仓库名>/<应用名>
```

安装应用前，可以用关键词“应用名+scoop”搜索一下，看看存不存在对应的bucket。



注：如果动手能力强的可以自己制作bucket。




# 配置

scoop允许我们对其进行配置，而我遇到过需要配置的就只有设置代理和aria2下载加速。

设置配置方式如下，如果多次设置则以最新的为准

``` shell
scoop config 配置名 配置值
```

查看已设置的配置（这里只能一个一个查看，没有列出所有的方法）

``` shell
scoop config 配置名

# 如果设置了的话会显示配置的值
配置值

# 如果没有设置的话会显示如下信息
配置名 is not set
```

移除配置

``` shell
scoop config rm 配置名

# 成功移除显示如下信息
配置名 has been removed
```



## 配置代理

有时候在国内下载速度慢，可以通过设置代理来提高下载速度。

``` shell
scoop config proxy '127.0.0.1:1080'
```

这里只列举了我知道的方式，还有其它的设置代理方式[Configuring Scoop to use your proxy](https://github.com/lukesampson/scoop/wiki/Using-Scoop-behind-a-proxy)



# 配置aria2下载加速

单纯的使用scoop下载速度可能不够快，这时候可以用scoop下载aria2来进一步提高下载速度，scoop会自动调用下载的aria2（注：一定要用scoop安装）

安装aria2

``` shell
scoop install aria2
```

本身有默认参数值

``` shell
aria2-enabled (default: true) # 是否使用aria2加速下载，开启true，关闭false
aria2-retry-wait (default: 2)
aria2-split (default: 5)
aria2-max-connection-per-server (default: 5) # 同时下载的线程数
aria2-min-split-size (default: 5M)
```

当然也可以自己进行参数设置，具体的参考[Multi-connection downloads with `aria2`](https://scoop-docs.now.sh/docs/misc/Multi-connection-downloads-with-aria2.html)



# 设置不同的开发环境版本

做程序开发的对于同一个开发环境可能需要安装不同的版本，例如：Java8和Java11、python2和python3，如果由自己做环境控制，就很麻烦了，通过scoop可以轻易的进行切换

假设我安装了jdk8和jdk11

``` shell
scoop list

# 下面显示的是已安装的软件列表
......
openjdk11 11.0.2-9 
openjdk8-redhat 8u282-b08
......
```

切换至jdk8

``` shell
scoop reset openjdk8-redhat
```

切换至jdk11

``` shell
scoop reset openjdk11
```



# 批量导入和导出

scoop安装了一系列软件后，换台设备还想继续使用这些软件，但是一个一个手动安装的话就太麻烦了，对于这方面scoop没有提供原生的支持，但是也有额外的方式能实现该要求：

[方式一：通过记录应用列表来实现](https://github.com/lukesampson/scoop/issues/1543#issuecomment-308894312)

[方式二：使用备份工具]([ScoopBackup-pwsh](https://github.com/Cologler/ScoopBackup-pwsh))