---
title: Spark开发环境配置
tags:
  - Spark
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - Spark
abbrlink: ec94ecc9
date: 2019-01-12 11:27:37
---

要开发Spark程序，当然少不了要配置开发环境，本文将会带领大家如何在Windows和Linux上搭建开发环境。

<!-- more -->

# Windows篇

## 下载

在开始配置前，首先需要下载搭建开发环境所必须的文件：

1. [**Hadoop**](http://spark.apache.org/downloads.html)
2. [**Spark**](https://hadoop.apache.org/releases.html)
3. [**winutils**](https://github.com/steveloughran/winutils/releases)

上述列表中的文件下载完成后，Hadoop和Spark可解压至任意目录（读者自己决定即可）。

比较特别的是**winutils**，在Windows环境下要对Hadoop进行调试开发*必须*要添加winutils组件，即把winutils压缩包下的所有文件全部放到**Hadoop的bin目录**下。

## 配置

配置开发环境其实很简单，只需要分别为Hadoop和Spark配置对应的环境变量HADOOP_HOME和SPAEK_HOME即可。

### 配置Hadoop

进入windows的环境变量配置，并新建变量：

|变量|值|
|:--|:--|
|HADOOP_HOME|你的hadoop的根目录路径|

### 配置Spark

进入windows的环境变量配置，并新建变量：

|变量|值|
|:--|:--|
|SPARK_HOME|你的spark的根目录路径|

当然，如果需要在命令行中使用spark的命令，可以在path变量中加入spark的bin目录路径：

|变量|值|
|:--|:--|
|path|%SPARK_HOME%\bin|

至此，Windows篇的Spark开发环境就配置完成了。

# 结束语

spark的开发环境配置其实和Java的开发环境配置一样，唯一的区别是windowns下需要额外添加winutils的相关文件。

至于Linix部分，会在后续进行跟新。
