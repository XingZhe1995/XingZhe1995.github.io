---
title: AQS
tags:
  - Java
categories:
  - 开发
  - 编程语言
  - Java
abbrlink: dfdb52be
date: 2021-04-07 14:53:45
---




AQS即同步器，用于构建锁和同步器的抽象类，使用AQS能简单且高效地构建出应用广泛的大量的同步器。



<!-- more -->



# 核心思想

AQS核心思想：如果被请求的共享资源空闲，则把当前请求的线程设置为有效的线程，并且把共享资源的状态设置为锁定。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制是用CLH队列锁实现的，即将暂时获取不到锁的线程加入到队列中。



## 关键成员变量state

``` java
private volatile int state; //共享变量，使用volatile修饰保证线程可见性
```

AQS通过volatile int成员变量**state**来标识同步状态，使用CAS对该同步状态进行原子操作实现值的修改。通过FIFO队列来完成获取资源线程的排队工作



## CLH队列

CLH（Craig,Landin,and Hagersten）队列是一个虚拟的双向队列，即不存在队列实例，仅存在节点间的关联关系。AQS把每条请求共享资源的线程封装成一个CLH锁队列的一个节点来实现锁的分配。



# 独占和共享

AQS定义了两种资源共享的方式：独占（只有一个线程能够执行）、共享（多个线程可以同时执行）。

独占可以分为公平锁和非公平锁

* 公平锁：按照线程在队列中的顺序，先到者先拿到锁；
* 非公平锁：无视队列顺序，谁抢到就是谁的。



## 两对方法

使用AQS实现自己的锁，依据资源的共享方式，需要实现不同的方法。

资源共享的方式为**独占**，则需要实现的方法如下

``` java
tryAcquire(int)
```

尝试获取资源，成功则返回true，失败则返回false。

``` java
 tryRelease(int)
```

尝试释放资源，成功则返回true，失败则返回false。

资源共享的方式为**共享**，需要实现的方法如下

``` java
 tryAcquireShared(int)
```

尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。

``` java
tryReleaseShared(int)
```

尝试释放资源，成功则返回true，失败则返回false。


**总结：独占模式下主要实现：*tryAcquire()*和*tryRelease()*方法，共享模式下主要实现*tryAcquireAndShared()*和*tryReleaseShared()*方法，这些方法一般都是成对使用，但也有同时实现使用情形，例如：ReentrantReadWriteLock。**

