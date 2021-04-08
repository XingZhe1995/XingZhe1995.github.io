---
title: Spring中Bean的作用域
tags:
  - Spring
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - Spring
abbrlink: fcf7a352
date: 2021-04-07 15:37:12
---






Spring中把每个组件都看作一个Bean进行管理，使用时多次请求同一个Bean，这个Bean对象究竟是同一个还是每次请求都会生成一个新的呢？这是由Bean的作用域进行控制的。



<!-- more -->



# 5种作用域

Spring中的Bean共有5种作用域，如下图所示

| 作用域            | 解释                                                         |
| ----------------- | ------------------------------------------------------------ |
| Singleton（默认） | 单例，在spring容器中仅存在一个bean实例                       |
| Prototype         | 多例，每次从spring容器中调用bean时，都会返回一个新的实例，即每次调用getBean时，都相当于new一个实例 |
| Request           | 每次http请求就会创建一个新的bean                             |
| Session           | 同一个http session共享一个bean，不同session使用不同的bean    |
| Global Session    | 用于portlet应用环境，只有portlet才定义了globalsession        |

注：Request和Session都只能用于webapplicationcontext环境中（就是常见的web开发环境）；而Global Session只能用于portlet context环境中（这个环境没见过，不太懂，请自行搜索相关资料）。



# 设置作用域

在Spring中设置Bean的作用域，是通过使用*@Scope*注解来实现的，具体使用方式如下

``` java
@Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
public class demo {
    ......
}
```

具体的参数如下所示，分别对应了4种作用域

``` java
ConfigurableBeanFactory#SCOPE_PROTOTYPE
ConfigurableBeanFactory#SCOPE_SINGLETON
org.springframework.web.context.WebApplicationContext#SCOPE_REQUEST
org.springframework.web.context.WebApplicationContext#SCOPE_SESSION
```

*注意：Global Session这个作用域没有找到，不知道放在哪里！*