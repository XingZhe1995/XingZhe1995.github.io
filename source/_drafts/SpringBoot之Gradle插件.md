---
title: SpringBoot之Gradle插件
tags:
  - java
  - spring
  - spring boot
categories:
  - 框架
  - spring
  - spring boot
---

在Java生态中，常用Maven和Gradle来对项目进行生命周期的管理。在这篇文章中使用Gradle来对Spring Boot项目进行构建，并记录常用的插件：java、war等。

<!-- more -->

# Gradle

关于Gradle的介绍在此省略，想了解相关知识的可自行搜索。

# SpringBoot插件

> Spring Boot的Gradle插件提供了Spring Boot在Gradle上的支持，允许打包可执行的Jar包、war归档文件、运行Spring Boot应用，以及使用*spring-boot-dependecies*提供的依赖管理。  
**注意:使用Spring Boot的Gradle插件需要Gradle4.0或以上的版本**

基本的Gradle脚本

``` gradle
buildscript {
    ext {
        springbootVersion = '2.0.1.BUILD-SNAPSHOT'
    }

    repositories {
        maven { url 'https://repo.spring.io/libs-snapshot' }
    }

    dependencies {
        classpath "org.springframework.boot:spring-boot-gradle-plugin:${springbootVersion}"
    }
}

apply plugin: 'org.springframework.boot'
```

在自定义变量springbootVersion中指定当前项目的spring boot版本

注：在Gradle的字符串中使用变量，需要在双引号中（在单引号中会无法识别），并且要以*${变量名}*这种形式进行引用。

以上就是最基本的脚本。

# 常用插件

当使用了其它插件时，SpringBoot插件会自动对项目的配置做出改变。

## java插件

## war插件

