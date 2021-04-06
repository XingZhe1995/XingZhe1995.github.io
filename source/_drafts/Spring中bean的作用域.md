---
title: Spring中bean的作用域
tags:
  - Spring
categories:
  - Java
  - 框架
  - Spring
---



<!-- more -->



| 作用域            | 解释                                                         |
| ----------------- | ------------------------------------------------------------ |
| Singleton（默认） | 单例，在spring容器中仅存在一个bean实例                       |
| Prototype         | 多例，每次从spring容器中调用bean时，都会返回一个新的实例，即每次调用getBean时，都相当于new一个实例 |
| Request           | 每次http请求就会创建一个新的bean                             |
| Session           | 同一个http session共享一个bean，不同session使用不同的bean    |
| GlobalSession     | 用于portlet应用环境，只有portlet才定义了globalsession        |

注：Request、Session和Global Session都只能用于webapplicationcontext环境中。