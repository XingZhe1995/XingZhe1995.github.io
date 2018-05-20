---
title: Spring Boot之devtools模块
tags:
  - spring
categories:
  - 框架
  - spring
  - spring boot
date: 2018-04-01 23:43:10
---


为了方便项目的开发，spring boot提供了一个相应的开发者工具：spring-boot-devtools模块，通过使用该模块就能获得相应的开发时特性。本文章讲述的就是如何配置相关的特性。

<!-- more -->

spring-boot-devtools模块，提供了一系列开发时特性，包括：热部署、livereload等。合理的使用这些特性能有助于项目的开发。

# 引入devtools模块

在项目中加入devtools模块很简单，只要在构建工具中引入相应的包即可。

值得一提的是：在Maven中将devtools依赖标记为optional，在Gradle中使用compileOnly来引用devtools依赖，是一个很好的习惯，可以有效的阻止devtools模块传递并应用到引用了该项目的项目。

## Gradle配置

gradle的配置如下所示，值得注意的是下面并没有加入版本号，这是因为使用了io.spring.dependency-managent插件，从而省略了版本号。如果没有使用该插件就需要明确的标识出版本号。

``` groovy
dependencies {
    compileOnly 'org.springframework.boot:spring-boot-devtools'
}
```

## Maven配置

maven的配置如下

``` xml
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
  </dependency>
</dependencies>
```

## 使用注意

在运行一个完整打包的项目时devtools会被**自动禁用**。

默认情况下，打包好的jar、war是不包含devtools模块的，如果需要在已打包好的jar、war使用相关的开发特性，就需要在构建工具中，显式地配置excledeDevtools属性，让构建工具在打包时不要排除devtools模块。

# 开发时特性

## 自动重启

当类路径上的文件发生变化时将会触发应用程序的自动重启，而无需自己手动重启项目，但是会自动忽略项目名为spring-boot、spring-boot-devtools、 spring-boot-autoconfigure、spring-boot-actuator和 spring-boot-starter的项目，即以上项目的改变不会触发重启。

### 原理

Spring Boot提供的重启技术通过使用两个类加载器来工作，一个基类加载器用于加载不会改变的资源（例：第三方jar包），一个重启类加载器用于加载正在开发的类，当重启时仅重新创建重启类加载器的实例，从而实现更快的重启速度。

### 资源钩子shutdown hook

devtools是依靠应用程序上下文的资源钩子（shutdown hook）在重启期间关闭它，如果禁用了资源钩子将会使得devtools无法正常工作

``` java
SpringApplication.setRegisterShutdownHook(false)
```

### 排除资源

某些资源的改变是无需触发重启的。默认情况下，改变的资源在以下路径中：/META-INF/maven, /META-INF/resources, /resources, /static, /public, /templates都不会触发重启而是会触发实时重新加载。当然我们也可以自定义排除的资源，只要修改*spring.devtools.restart.exclude*属性，例子：

``` properties
spring.devtools.restart.exclude=static/**,public/**
```

但是以上写法会覆盖默认值，所以如果在不想覆盖默认值的情况下加入自定义的排除资源，需要使用*spring.devtools.restart.additional-exclude*属性，例子：

``` properties
spring.devtools.restart.additional-exclude=static/**,public/**
```

### 监控其它路径

默认的是只有类路径下的文件发生了改变才会触发重启，如果想要不在类路径下的资源发生改变后触发重启，需要使用*spring.devtools.restart.additional-paths*属性来配置额外的路径

``` preperties
spring.devtools.restart.additional-paths = other-path/**
```

### 关闭自动重启

如果不需要使用自动重启，可以通过禁用spring.devtools.restart.enabled属性来关闭该特性（注：重启类加载器依然会被初始化，但是不会监控文件的改变）。

``` properties
spring.devtools.restart.enabled = false
```

如果想要彻底关闭devtools对于重启的支持，需要在调用SpringApplication.run()前，禁用*spring.devtools.restart.enabled*属性，如下所示：

``` java
public static void main(String[] args) {
  System.setProperty("spring.devtools.restart.enabled", "false");
  SpringApplication.run(MyApp.class, args);
}
```

## LiveReload

spring-boot-devtools包含了一个嵌入式的LiveReload服务器，允许当资源发生改变时实时刷新浏览器。

``` properties
spring.devtools.livereload.enabled=true
```

默认的，该特性是打开了的。如果想停用该特性，则设置为：

``` properties
spring.devtools.livereload.enabled=false
```

# 结束语

spring-boot-devtools模块的开发时特性并没有完整的出现在文章中，比如说远程支持，更换类加载器等，仅仅选取了平时可能会用到的特性进行记录。

# 参考

[Spring Boot官方文档 Developer Tools](https://docs.spring.io/spring-boot/docs/2.0.1.BUILD-SNAPSHOT/reference/htmlsingle/#using-boot-devtools)
