---
title: AarrayList和LinkedList区别
tags:
---


ArrayList和Linked List都不是线程安全的。
ArrayList的数据结构是Object[]数组，Linked List的数据结构是双向链表。
ArrayList的空间占用主要体验在会额外预留一定的容量空间，Linked List的空间占用则体现在要额外存放前后驱元素的引用。
读取上，ArrayList支持随机读取，Linked List则不支持只能逐个遍历。修改上（增、删），ArrayList插入末尾只有O（1），插入/删除指定位置要O（n-i）；Linked List的插入、修改（增、删）近似O（1），指定位置则近似O（n）。总结就是：ArrayList适合读多写少的场景，Linked List适合写多读少的场景。