---
title: Interceptor和filter区别
tags:
  - Spring
categories:
  - Java
  - 框架
  - Spring
abbrlink: 7c2ed01f
date: 2021-04-07 15:54:49
---






Spring种filter和Interceptor傻傻分不清，因此简单的做个记录



<!-- more -->

*filter*即过滤器，是属于Java的。

*Interceptor*即拦截器，是属于Spring的，是用于面向切面编程的（指定用于web开发中）。



在执行上，*filter*的执行靠server（例如：tomcat），*Interceptor*的执行靠Spring容器，且执行顺序方面：*filter*要早于*Interceptor*执行。

在能力上，Spring项目中*Interceptor*能做到*filter*能做的且能力强于filter，因此Spring项目种要优先使用*filter*。

