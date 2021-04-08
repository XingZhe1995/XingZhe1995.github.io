---
title: Spring事务传播机制
tags:
  - Spring
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - Spring
date: 2021-04-08 21:57:44
---






数据库操作中事务是必不可少的，而在Spring框架中提供了7种事务传播机制。



<!-- more -->



# 事务传播

什么是事务传播？

以*@Transaction*注解的方法表示要使用事务，当多个这样的事务方法相互调用时，事务在这些方法间如何传播，一个方法有没有事务以及对事务的要求，会影响另一个事务方法的调用或者被调用的执行，这种影响是由事务传播类型决定。



# 7种事务传播机制

| 传播类型         | 解释                                                         |
| ---------------- | ------------------------------------------------------------ |
| NEVER            | 不使用事务，如果当前事务存在，则抛出异常                     |
| REQUIRED（默认） | 如果当前没有事务，则自己新建一个事务，如果当前存在事务，则加入这个事务 |
| REQUIRES_NEW     | 创建一个新事务，如果存在当前事务，则挂起该事务               |
| MANDATORY        | 当前存在事务，则加入当前事务，如果当前事务不存在，则抛出异常 |
| SUPPORTS         | 当前存在事务，则加入当前事务，如果当前没有事务，就以非事务方法执行 |
| NOT_SUPPORTED    | 始终以非事务方式执行，如果当前存在事务，则挂起当前事务       |
| NESTED           | 如果当前事务存在，则在嵌套事务中执行，否则REQUIRED的操作一样（开启一个事务） |



## NEVER

不支持事务，机制是：如果当前存在事务或者被事务操作（即被带有事务方法调用或者调用带有事务的方法），就会抛出异常。

``` java
@Service
public class DemoService {
    ......
    @Transactional
    public List methodA() {
        ......
        demoService.methodB(); //抛出异常
    }
    
    @Transactional(propagation = Propagation.NEVER)
    public List methodB() {
        ......
    }
    
    @Transactional(propagation = Propagation.NEVER)
    public List methodC() {
        ......
        demoService.methodD(); //抛出异常
    }
    
    @Transactional
    public List methodD() {
        ......
    }
}
```



## REQUIRED

支持事务，默认的事务传播机制，如果当前没有事务，则自己新建一个事务，如果当前存在事务，则加入这个事务。

即事务方法必须运行在事务中，且事务是一个整体，要么一起提交，要么一起回滚。



## REQUIRED_NEW

支持事务，机制是：创建一个新事务，如果存在当前事务，则挂起该事务。

即存在两个事务，调用方的外层事务，被调用方的内层事务。内层事务的执行与外层事务无关，内层事务如果因异常回滚，外层事务方法中需要看情况进行异常捕获处理，如果处理则外层事务不会回滚，如果没处理则内外事务一起回滚。

``` java
@Service
public class DemoService {
    ......
    @Transactional
    public List methodA() {
        ......
        demoService.methodB(); //methodA()的事务挂起，methodB()新建一个事务
    }
    
    @Transactional(propagation = Propagation.REQUIRED_NEW)
    public List methodB() {
        ......
    }
    
    public List methodC() {
        ......
        demoService.methodD();
    }
    
    @Transactional(propagation = Propagation.REQUIRED_NEW) //创建一个新事务
    public List methodD() {
        ......
    }
}
```



## MANDATORY  

强制要求事务，机制是：当前存在事务，则加入当前事务，如果当前事务不存在，则抛出异常。

即事务方法必须运行在事务中，且不会自己创建事务，只能加入已有的事务，事务是一个整体，要么一起提交，要么一起回滚。



## SUPPORTS

支持事务，机制是：当前存在事务，则加入当前事务，如果当前没有事务，就以非事务方法执行



## NOT_SUPPORTED

不支持事务，机制是：始终以非事务方式执行，如果当前存在事务，则挂起当前事务。

即使用该种事务传播机制的事务方法，永远不会使用事务，但不会影响调用方的事务。



## NETSTED

嵌套事务，机制是：如果当前事务存在，则在嵌套事务中执行，否则REQUIRED的操作一样（开启一个事务）。

即存在两个事务，调用方的外层事务，被调用方的嵌套事务（子事务），需要注意的有：

1. 外层事务会影响嵌套事务，嵌套事务结束需要等待外层事务的操作结果，外层事务回滚或提交，嵌套事务也会跟着回滚或提交。

2. 相反，外层事务不会受到嵌套事务的影响，嵌套事务是独立回滚的，外层事务不会受到影响，即嵌套事务的异常，外层事务可以选择回滚（不捕获处理）或者回滚（捕获处理）。

   注：这个嵌套事务回滚不影响外层事务的特性是有前提的，否则内外都回滚。

   使用前提：

   * JDK版本要在1.4以上，有java.sql.Savepoint。因为nested就是用savepoint来实现的。
   * 事务管理器的nestedTransactionAllowed属性为true。
   * 外层事务方法try-catch嵌套事务方法的异常。

