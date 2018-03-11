---
title: Python内建方法
tags:
categories:
---

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
