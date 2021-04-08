---
title: Spring事务失效
tags:
  - Spring
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - Spring
abbrlink: ba4518b0
date: 2021-04-07 22:07:38
---




事务是在数据库操作中非常重要的一环，但是在Spring项目下却会遇到事务失效的情况。



<!-- more -->



# 8种失效原因

Spring项目中，事务失效主要有8种原因：

1. 数据库引擎不支持事务
2. 数据源没有配置事务管理器
3. 事务方法所在类没有被Spring管理
4. 事务方法不是PUBLIC方法
5. 事务方法内捕获了异常
6. 事务方法内抛出的异常类型错误
7. 事务传播机制设置为NOT_SUPPORT
8. 事务方法的调用在类内部进行



## 数据库引擎不支持事务

从数据库层面看，事务是由数据库引擎提供支持的，因此如果选用的引擎不支持事务，那么系统的调用和配置再正确，也无法正常的使用事务。

例子：MySQL的MySIAM引擎和InnoDB引擎，MySIAM引擎是不支持事务的，而InnoDB引擎则支持事务。



## 数据源没有配置事务管理器

Spring项目中是需要自己配置*事务管理器*的，如果没有配置，那就无法使用事务。如果是Spring Boot项目的话，已经自动帮我们配置好一个默认的事务管理器了。



## 事务方法所在类没有被Spring管理

如果没有把类注册成Bean的话，Spring就不会管理这个类，自然就没有Spring的事务支持了。

``` java
//@Service 这里没有相应的注解：@Component、@Service之类的
public class demo {
    @Transactional
    public List findById(Long id) {
        .....
    }
}
```



## 事务方法不是PUBLIC方法

根据官方文档，*@Transactional*注解只有用在*PUBLIC*修饰的方法上事务才会生效，如果要用在非*PUBLIC*方法上，就需要使用AspectJ代理模式。（这应该是由Spring默认的代理模式所形成的限制）

官方原文如下：

> When using proxies, you should apply the @Transactional annotation only to methods with public visibility. If you do annotate protected, private or package-visible methods with the @Transactional annotation, no error is raised, but the annotated method does not exhibit the configured transactional settings. Consider the use of AspectJ (see below) if you need to annotate non-public methods.



## 事务方法内捕获了异常

``` java
@Transactional
public List demo() {
    try {
        ......
    } catch (Exception e) {
        ......
    }
}
```

像这种已经在方法内部处理了异常的情况，也是无法触发事务的，Spring是通过AOP的方式来提供事务支持的，因此在代理类中没有捕获到异常，也就不会触发事务回滚了。



## 事务方法内抛出的异常类型错误

Spring中默认的触发事务的异常类型是：RuntimeException及其子类，因此抛出非RuntimeException及其派生类的异常是不会触发事务的，

这个异常类型可以通过*rollbackFor*来配置，但仅限于 `Throwable` 异常类及其子类

``` java
@Transactional(rollbackFor = Exception.class)
```



## 事务传播机制设置为NOT_SUPPORTED

``` java 
@Transactional(propagation = Propagation.NOT_SUPPORTED)
public List demo1 {
    ......
}
```

事务传播机制**Propagation.NOT_SUPPORTED：** 始终以非事务方式执行,如果当前存在事务，则挂起当前事务。

因此配置了该传播机制的也就没有事务，因而也就不会触发事务了。



## 事务方法的调用在类内部进行

``` java
@Service
public class Demo {
    @Transactional
    public List demo1() {
        ......
        demo2();
    }
    
    @Transactional
    public List demo2() {
        ......
    }
}
```

像这种在同一个类内然后直接调用方法的，事务方法demo2是不会生效的。

Spring的事务是通过AOP生效的，如果直接在类中调用事务方法，就会没有经过代理类，这样就无法提供事务支持了。

有两种解决方法

方法一：在类中注入自己，然后再通过这个注入的类调用另一个方法，不过这个方法不太优雅。

``` java
@Service
public class DemoService {
    
    @Autowired
    private DemoService demoService;
    
    @Transactional
    public List demo1() {
        ......
        demoService.demo2();
    }
    
    @Transactional
    public List demo2() {
        ......
    }
}
```

方法二：Spring框架配置公开代理，Spring的AOP框架默认情况下*不公开代理*，因为这样做会降低性能。

开启方法

``` java
@EnableAspectJAutoProxy(exposeProxy = true)
```

调用方法

``` java
@Service
public class DemoService {
    
    @Autowired
    private DemoService demoService;
    
    @Transactional
    public List demo1() {
        ......
        ((DemoService)AopContext.currentProxy()).demo2();
    }
    
    @Transactional
    public List demo2() {
        ......
    }
}
```





