---
title: Spring事务失效
tags:
  - Spring
categories:
  - Java
  - 框架
  - Spring
---

事务是在数据库操作中非常重要的一环，但是在Spring项目下却会遇到事务失效的情况。



<!-- more -->



事务失效的原因可以从数据库、框架和代码三个层面来分析。



# 数据库引擎不支持事务

从数据库层面看，是由数据库引擎提供事务支持的，因此如果选用的引擎不支持事务，那么系统的调用和配置再正确，也无法正常的使用事务。MySQL的MySIAM引擎和InnoDB引擎就是很好的例子，MySIAM引擎是不支持事务的，而InnoDB引擎则支持事务。



# Spring框架层面

想要Spring框架正常的支持和使用事务，是需要正确配置的。



## 数据源没有配置事务管理器

Spring项目中是需要自己配置*事务管理器*的，如果没有配置，那就无法使用事务。如果是Spring Boot项目的话，已经自动帮我们配置好事务管理器了。



## 没有被 Spring 管理

没有把类注册成Spring的话，Spring就不会管理这个类，自然就没有Spring的事务支持了。



# 方法不是PUBLIC





# 捕获了异常

Spring是通过异常来判断是否来进行事务回滚的，如果已经在方法内部处理了抛出的异常，那么事务就不会被出发。



# 异常类型错误

Spring中触发事务的异常类型是：RuntimeException及其子类，因此抛出非RuntimeException及其派生类的异常是不会触发的，

这个可以通过*rollbackFor*来配置

``` java
@Transactional(rollbackFor = Exception.class)
```





## 事务传播机制为不支持事务




异常不是RuntimeException


自身调用







# 参考