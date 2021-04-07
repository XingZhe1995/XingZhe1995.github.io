---
title: Python之IO操作
tags:
  - Python
categories:
  - 开发
  - 编程语言
  - Python
abbrlink: 908ea013
date: 2018-06-02 10:40:00
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

# 具体例子：读文件demo.txt
with open('demo.txt','r') as f:
  f.read()
```
需要注意的是：被打开的*文件对象*在操作完成后*一定*要调用自身的*关闭*方法close(例：f.close())来关闭文件对象，释放系统资源，即使依靠特定的写法确保在读写操作后一定会调用到close()方法，但每次都这样写会导致很繁琐，因而python提供了上述的*with..as..*语法，助我们写出更优雅的代码。

打开文件对象使用的方法是*open()*，常用参数如下
``` python
open(file,mode,encoding)
```
- file：要操作的目标文件名，如果该文件不存在则会抛出IOError
- encoding：指定目标文件读/写时的编码模式（该参数应该只用于文本模式），编码模式默认情况下是与当前系统的编码模式一致。
- mode：对目标文件的打开模式
a表示append，r表示read，w表示write，+表示读写模式。，b表示二进制，t表示文本模式，t是默认的模式

|操作模式|描述|
|:---:|:---|
|r|以读模式打开|
|w|以写模式打开|
|a|以追加模式打开 (从 EOF 开始, 必要时创建新文件)|
|r+|以读写模式打开|
|w+|以读写模式打开 (参见 w )|
|a+|以读写模式打开 (参见 a )|
|rb|以二进制读模式打开|
|wb|以二进制写模式打开 (参见 w )|
|ab|以二进制追加模式打开 (参见 a )|
|rb+|以二进制读写模式打开 (参见 r+ )|
|wb+|以二进制读写模式打开 (参见 w+ )|
|ab+|以二进制读写模式打开 (参见 a+ )|

# 读文件

读文件时常用的方法有3个：read()、readline()、readlines()

假设文本text.txt的内容如下：

``` text
First
Second
Third
5th
6th
7th
```

## read()

read()：一次性读取文本中全部的内容，以字符串的形式返回结果

``` python
>>> with open("text.txt","r") as f:
>>>   f.read()
>>> 'First\nSecond\nThird\n5th\n6th\n7th'
```

## readline()

readline()：只读取文本第一行的内容，以字符串的形式返回结果

``` python
>>> with open("text.txt","r") as f:
>>>   f.readline()
>>> 'First\n'
```

## readline()

readlines()：读取文本所有内容，并以数列的形式返回结果，一般配合for in使用

``` python
>>> with open("text.txt","r") as f:
>>>   f.readlines()
>>> ['First\n', 'Second\n', 'Third\n', '5th\n', '6th\n', '7th']
```

# 写文件

相对于读文件而言，写文件的方法就简单的多，只有一个简单的*write()*方法，如果有大量的内容只要不断的调用write()方法写入就可以了。
``` python
>>> with open("write.txt","w") as f:
>>>   f.write("hello python")
>>>
>>> with open("write.txt","r") as f:
>>>   f.read()
>>>
>>> hello python
```

# 序列化与反序列化

Python的序列化与反序列化是通过pickle模块来实现的

pickle.dump()方法把任意对象序列化为字节流，然后就可以写入一个file-like Object之中
``` python
>>> import pickle
>>> d = dict(content="hello world")
>>> with open("demo.txt","wb") as f:
>>>   pickle.dump(d,f)
```

与dump()相对的就是load()方法，可以把已经序列化的对象反序列化
``` python
>>> import pickle
>>> with open("demo.txt","wb") as f:
>>>   d = pickle.load(d,f)
>>>   print(d)
>>> hello world
```

# 结束语

对于Python的IO操作还有很多没了解到的地方，因而本篇文章还有很多不完善的地方，只能随着不断的使用加深了解，日后再继续完善。

# 参考
[廖雪峰的Python3教程之IO编程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143192607210600a668b5112e4a979dd20e4661cc9c97000)
