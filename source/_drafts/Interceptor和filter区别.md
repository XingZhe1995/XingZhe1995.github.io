---
title: Interceptor和filter区别
tags:
  - Spring
categories:
  - Java
  - 框架
  - Spring
---

filter即过滤器，interceptor即拦截器。
filter是属于Java的，interceptor是属于spring的。
在spring项目中interceptor能做到filter能做的且能力强于filter，因此要优先使用interceptor。
Interceptor是用于面向切面编程的（指定用于web中），filter（靠server执行，例如：tomcat）的执行顺序要早于interceptor（靠spring容器执行）。