---
title: 注解@Autowired和@Resource区别
tags:
  - Spring
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - Spring
abbrlink: f6f32af4
date: 2021-04-06 22:14:18
---



Spring提供了注解@Autowired用于依赖注入，同时也支持使用Java本身提供的注解@Resource用于依赖注入，这两个注解有什么异同呢？



<!-- more -->



# 相同点

*@Autowired*和*@Resource*都是用来装配bean的，都可以注解在字段上和setter方法上



# 区别

* 归属上：*@Autowired*是属于Spring的，*@Resource*是属于Java的。
* 装配方式：
  * *@Autowired*默认按照类型装配（默认情况下依赖对象必须存在，如果允许null，可以设置required属性为false），如果需要按照名称匹配需要结合@Qualifier使用。要注意只能有一个实现类否则存在多个候选对象会报错。
  * @Resource默认按照名称装配，名称通过name属性指定（不指定时取字段名或者setter方法上的属性名），当根据名称找不到时会按照类型进行装配，如果name一旦指定就只会按照名称进行装配。使用@Resource能较少代码耦合。

注：速度上，这两种方式都是直接指定name是最快的。