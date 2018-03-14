---
title: Python之IO操作
tags:
categories:
---

<!-- more -->

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
