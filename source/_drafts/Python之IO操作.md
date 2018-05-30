---
title: Python之IO操作
tags:
  - Python
categories:
  - 编程语言
  - Python
---

编码过程中，总是绕不开要对文件进行读写操作，本文章讲述的是Python如何进行IO操作。

<!-- more -->

在开始前先看一段引用自他人的描述：

> 在磁盘上读写文件的功能都是由操作系统提供的，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

由以上内容可以知道对文件的读写操作都是通过文件对象（文件描述符）来完成的。

# 打开文件对象

要进行读写操作，需要先打开一个文件对象，一般的语法是：
``` python
with open(目标文件,操作模式) as 文件描述符:
  文件描述符.做点什么

# 具体例子：读文件
with open('demo.txt','r') as f:
  f.read()
```
需要注意的是：被打开的*文件对象*在操作完成后*一定*要调用自身的*关闭*方法close(例：f.close())来关闭文件对象，释放系统资源，即使依靠特定的写法确保在读写操作后一定会调用到close()方法，但每次都这样写会导致很繁琐，因而python提供了上述的*with..as..*语法，助我们写出更优雅的代码。

打开文件对象使用的方法是*open()*，常用参数如下
``` python
open(file,mode,encoding)
```
- file：指的是要操作的目标文件名，如果该文件不存在则会抛出IOError
- mode：指的是对文件的操作模式

|操作模式|描述|
|:---:|:---|
|r|w|
|r|w|
|r|w|
|r|w|
|r|w|
|r|w|
|r|w|

- encoding：

# 读操作

# 写操作

# 对内存进行操作

# 序列化与反序列化

# read()、readline()、readlines()

假设文本text.txt的内容如下：

``` text
First
Second
Third
5th
6th
7th
```

read()：一次性读取文本中全部的内容，以字符串的形式返回结果

``` python
>>> f = open("text.txt")
>>> f.read()
'First\nSecond\nThird\n5th\n6th\n7th'
```

readline()：只读取文本第一行的内容，以字符串的形式返回结果

``` python
>>> f = open("text.txt")
>>> f.readline()
'First\n'
```

readlines()：读取文本所有内容，并以数列的形式返回结果，一般配合for in使用

``` python
>>> f = open("text.txt")
>>> f.readlines()
['First\n', 'Second\n', 'Third\n', '5th\n', '6th\n', '7th']
```
