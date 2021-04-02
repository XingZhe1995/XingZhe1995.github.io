---
title: IDEA输出中文乱码解决方法
categories:
  - 开发工具
abbrlink: 3d0562c8
date: 2019-10-23 21:37:47
tags:
  - DevOps
---


IDEA输出中文乱码是个常见的问题了，记录下问题的解决方法，以及网上方法无效的原因。

<!-- more -->

先上结论：首先要确认是idea的问题，如果是则打开idea，选择help->Edit Custom VM Options，加入参数-Dfile.encoding=UTF-8，保存并重启，即可解决问题。

# 定位

要解决中文乱码问题，首先要知道是哪里导致了中文乱码，才能进行针对性的配置。

定位的方法很简单，就是使用排除法：

* 直接用*java -jar*直接运行程序
* 直接用构建工具运行程序
* 是web程序则直接用tomcat运行

这样就很清楚的知道是哪里的问题了。

但是！但是！但是！一般情况下上面的环节其实都是没问题的，主要问题是在idea的身上！

# 配置

> windows默认使用GBK，idea新装的时候也是使用GBK

看到中文乱码，我们会自然的联想到是编码问题，所以会把GBK改为兼容性更好的UTF-8，具体操作如下：

1. 打开idea
2. 选中工具栏中的**Help**标签下的**Edit Custom VM Options**
3. 加入参数 **-Dfile.encoding=UTF-8**
4. 保存并重启idea

经过上述步骤，中文乱码就迎刃而解了。

# 本质

搜索引擎上能搜索到很多解决idea中文乱码的文章，都是在**vmoptions**中添加参数 **-Dfile.encoding=UTF-8**，但是就是没有效果！为啥？

其实是因为idea在用户目录下有一个**vmoptions**副本！！

跟着文章通常修改的是idea安装目录下的idea.exe.vmoptions和idea64.exe.vmoptions，说白点就是你修改的和idea使用的根本不是同一个，才会导修改没有效果。

# 参考

[重点在评论14楼](https://www.cnblogs.com/sxdcgaq8080/p/7648400.html)
