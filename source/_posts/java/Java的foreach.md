---
title: Java的foreach
tags:
  - Java
categories:
  - Java
abbrlink: 4c0b6fc5
date: 2019-10-31 12:31:13
---


开发中经常使用到foreach，现在是时候深入了解一下了。

<!-- more -->

# 使用

foreach是for的增强，基本语法结构：

``` java
// 这里的collection 指数组或集合
for(T t : collection) {
  ... dosomething ...
}

# 数组
int[] a = new int[]{1,2,3};
for(int i : a) {
  System.out.println(i);
}

# 集合
List<String> list = new ArrayList();
for(String str : list) {
  System.out.println(str);
}
```

# 优缺点

由上面的例子可以看到，在写法上foreach与for写法相比是简化了很多。

要注意的一点是：使用foreach能遍历的，使用for也能遍历，反之则不一定能！！！

其次还有一些缺点：

* 对于数组，在遍历过程中需要使用元素下标的，则无能为力。
* 对于集合，在遍历过程中无法对元素进行增删，否则会报异常，因为我们没法获得迭代器进行操作。

# 遍历的顺序

有时候我们可能对元素的遍历顺序有要求，那么使用foreach与for的遍历顺序是一致的吗？

答案是一样的！！

通过阅读官方的文档可以知道，对数组使用foreach写法，等同于下面的写法：

``` java
for (int i = 0; i < arr.length; i++) {

}
```

而对于集合而言，就和使用迭代器进行遍历一样：

``` java
for (Iterator iterator = collection.iterator(); iterator.hasNext(); ) {
  iterator.next do something......
}
```

# 迭代器iterator

对于集合而言，使用foreach的遍历，其实就是在使用迭代器进行遍历，这点通过Java的规范文档可以了解到，也可以通过对Java的class文件反编译观察到，如下图所示：

![foreach与迭代器.png](http://ww1.sinaimg.cn/large/e6dffef4gy1gaqphag66lj21bb0bqtjk.jpg)

但是与直接使用iterator不同，使用foreach的过程中，是无法对元素进行增删的，因为iterator是隐含的仅存在于编译后的class文件中，即开发中无法直接获取iterator，因而也就无法执行增删操作。

# 参考

[Java中的foreach遍历顺序](https://www.jianshu.com/p/81cec83541be)

[Java规范文档-看14.14.2. The enhanced for statement部分](https://docs.oracle.com/javase/specs/jls/se7/html/jls-14.html#jls-14.14.2)
