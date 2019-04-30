---
title: Python之Matplotlib
abbrlink: 58f9dd24
tags:
categories:
---

Matplotlib是Python中最常用的可视化工具之一，可以非常方便地创建海量类型地2D图表和一些基本的3D图表。

<!-- more -->

# scatter

``` python
matplotlib.pyplot.scatter(x, y, s=None, c=None, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, verts=None, edgecolors=None, hold=None, data=None, **kwargs)
```

作用：绘制x与y的散点图，具有不同的标记大小和颜色

详细说明：x与y的**维度及其长度必须一致**的数组，通过在x与y数组中的同一位置的数据就构成了在直角座标系中的坐标(x,y)，从而构成了散点图中的其中一个点。

参数

|参数|参数说明|说明|
|:---|:---|:---|
|x,y|维度及其长度必须一致的数组|数据的位置|
|s|标量或者与x,y的shape相同的数组；大小计算：指定数值的二次方|数据标记的大小|
|c|颜色、序列或颜色的序列|数据标记的颜色|

scatter的颜色说明

可选值：

1. 单一颜色格式的字符串
2. 长度为n的颜色序列
3. 使用cmap和norm将n个数字映射到颜色的序列
4. 一个二维数组，其中行是RGB或RGBA

注意：参数不应该是单个RGB或RGBA数值，因为它难以与数组的值进行色彩映射。如果要为所有点指定相同的RGB或RGBA值，请使用单行的二维数组。

|缩写|颜色字符串|
|:---|:---|
|b|blue|
|c|cyan(青色、蓝绿色)|
|g|green|
|k|black|
|m|magenta(紫红色)|
|r|red|
|w|white|
|y|yellow|

``` python
>>> plt.scatter([1,2,3],[4,5,6])
```

效果：![x,y为一维数组](/images/matplotlib/scatter01.png)

``` python
>>> plt.scatter([[1,2,3],[4,5,6]],[[7,8,9],[10,11,12]])
```

效果：![x,y为二维数组](/images/matplotlib/scatter02.png)

``` python
>>> plt.scatter([1,2,3],[4,5,6],50)
```

效果：![散点图标记的大小](/images/matplotlib/scatter03.png)

``` python
>>> plt.scatter([1,2,3],[4,5,6],50,,'r')
```

效果：![散点图标记的颜色](/images/matplotlib/scatter04.png)

# 参考

[MatplotLib参考文档](https://matplotlib.org/index.html)
