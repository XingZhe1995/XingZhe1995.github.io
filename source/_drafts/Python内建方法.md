---
title: Python内建方法
date: 2018-06-02 09:27:00
tags:
  - Python
categories:
  - 编程语言
  - Python
---

在本文章中将会记录Python常用的方法，并且将会不断更新。

<!-- more -->

# sorted

``` python
def sorted(iterable, cmp=None, key=None, reverse=False)
```

作用：接受一个可迭代对象，返回一个排序后的list对象

参数：

1. iterable：接受一个可迭代的对象（因为sorted实现了迭代协议，所以接受的参数不一定需要list，可以迭代的对象就可以，也就是鸭子类型）
2. cmp：在python3.x中已被移除
3. key：指定一个方法用于每一个列表元素进行比较；python提供了便利的方法去访问方法,在*operator*模块中有*itemgetter()*, *attrgetter()*, 和*methodcaller()*方法。
4. reverse：升序或降序。True：降序；False：升序。默认升序。

``` python
sorted([3,1,2],reverse=True)
[3, 2, 1]

>>> sorted([3,1,2],reverse=False)
[1, 2, 3]
```

# len()

``` python
def len(object)
```

作用：返回字符串、列表、字典、元组等长度

参数：
1.object：要计算的字符串、列表、字典、元组等

``` python
>>> len('12345')
5

>>> len([1,2,3,4,5])
5

>>> len({"first":1,"second":2,"third":3})
3

>>> len(('1','2','3'))
3
```

# strip()、lstrip()、rstrip()

``` python
S.strip([chars])
```

作用：删除前导和后缀字符并返回字符串；当不加参数时，默认删除前后空格；

``` python
>>> str = ' abcd '
>>> str
' abcd '
>>> str.strip()
'abcd'
```
