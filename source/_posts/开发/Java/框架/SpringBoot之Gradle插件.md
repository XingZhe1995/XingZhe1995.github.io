---
title: SpringBoot之Gradle插件
tags:
  - Spring Boot
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - Spring Boot
abbrlink: dcecce02
date: 2018-03-31 15:42:56
---

在Java生态中，常用Maven和Gradle来对项目进行生命周期的管理。在本篇文章中使用Gradle来对Spring Boot项目进行构建，并记录常用的插件：java、war、dependency-management。

<!-- more -->

# Gradle

关于Gradle的介绍在此省略，想了解相关知识的读者请自行搜索。

# SpringBoot插件

> Spring Boot的Gradle插件提供了Spring Boot在Gradle上的支持，允许打包可执行的Jar包、war归档文件、运行Spring Boot应用，以及使用*spring-boot-dependecies*提供的依赖管理。**注意:使用Spring Boot的Gradle插件需要Gradle4.0或以上的版本**

由官方译文可以知道，想要使用Gradle来方便管理Spring Boot项目，使用SpringBoot插件是一个极佳的选择。

## 基本配置

在Gradle中想要使用SpringBoot插件，需要如下的4个步骤：

1. 配置下载插件的仓库地址：[https://repo.spring.io/libs-snapshot](https://repo.spring.io/libs-snapshot)
2. 在依赖中引入要使用的插件，即SpringBoot的Gradle插件spring-boot-gradel-plugin
3. 指定插件的版本，最好与项目使用的Spring Boot版本一致。
4. 应用插件

整个基本配置如下所示：

``` gradle
buildscript {
    ext {
        springbootVersion = '2.0.1.BUILD-SNAPSHOT'
    }

    repositories {
        jcenter()
        maven { url 'https://repo.spring.io/snapshot' }
        maven { url 'https://repo.spring.io/milestone' }
    }

    dependencies {
        classpath "org.springframework.boot:spring-boot-gradle-plugin:${springbootVersion}"
    }
}

apply plugin: 'org.springframework.boot'

// 这里应用Java插件或者war插件
apply plugin: 'java'
apply plugin: 'io.spring.dependency-management'
```

通过上述配置，就能在整个项目中使用SpringBoot插件了。同时为了方便后续spring boot的版本更改，在配置中加入了自定义的变量*springbootVersion*，用于指代spring boot的版本。

注：在Gradle的字符串中使用变量，需要在双引号中使用（在单引号中会无法识别），并且要以*${变量名}*这种形式进行引用。

在Spring Boot项目中，除了springboot插件外，还需要Java插件或War插件，以及dependency-management插件，才能达到构建项目所需的最少插件。

## 运行应用

要在不创建归档文件（jar、war）的情况下运行应用程序，需要使用bootRun任务：

``` shell
gradle bootRun
```

### 配置主类

通过配置主类可以指定应用的运行入口。

默认情况下，bootRun任务会自动通过寻找在任务类路径下的目录中的带有*public static void main(String[])*方法的类配置为主类（main class）。

当然，可以通过bootRun任务的main属性显式地配置主类：

``` gradle
bootRun {
  main = 'com.example.ExampleApplication'
}
```

或者，可以使用Spring Boot DSL的mainClassName属性在项目范围内配置主类：

``` gradle
springBoot {
  mainClassName ='com.example.ExampleApplication'
}
```

## 依赖管理

当项目中应用io.spring.dependency-management插件时，Spring Boot的插件会自动从项目中正在使用的Spring Boot版本导入相应的spring-boot-dependencies bom。这为Maven用户提供了类似的依赖管理体验，可以像使用使用maven一样，自定义依赖的jar包版本、排除依赖等。

当然可以不使用io.spring.dependency-management插件，但是这会为项目的管理徒然增加难度。

### 自定义版本号

通过使用依赖管理插件，可以使用属性来控制依赖关系的版本，参考[bom](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-dependencies/pom.xml)，例子：

``` java
ext['slf4j.version'] = '1.7.20'
```

> Spring Boot的每个发布版本都是针对特定的第三方依赖来进行设计和测试。覆盖版本可能会导致兼容性问题，应该要小心处理。

## 插件

单独的使用SpringBoot插件对项目几乎没有改变，但是当使用了某些插件时，SpringBoot插件会检测出该插件并对项目配置做出相应的改变。以下列出几个常用的插件：

### Java插件

``` gradle
apply plugin: 'java'
```

当项目中使用了Java插件，Gradle会做出以下改变：

1. 创建一个名为bootJar的BootJar任务：为项目创建一个可执行的jar。该jar将包含项目目录结构中main文件集中运行时类路径中的所有内容;类被放置在BOOT-INF / classes中，而jar被放置在BOOT-INF / lib中
2. 将assemble任务配置为依赖于bootJar任务
3. 禁用jar任务
4. 创建了一个名为bootRun的BootRun任务，用于运行该项目
5. 创建一个名为bootArchives的配置，其中包含由bootJar任务生成的组件
6. 配置任何没有配置编码的JavaCompile任务以使用UTF-8编码
7. 配置任何JavaCompile任务以使用-parameters编译器参数

### War插件

``` gradle
apply plugin: 'war'
```

当项目中使用了war插件，Gradle会做出以下改变：

1. 创建一个名为bootWar的BootWar任务，该任务将为该项目创建一个可执行的war。除了标准打包之外，providedRuntime配置中的所有内容都将打包在WEB-INF/lib-provided的文件夹中
2. 将assemble任务配置为依赖于bootWar任务
3. 禁用war任务
4. 配置bootArchives的配置以包含由bootWar任务生成的组件

注意：使用了war插件就不需要再使用Java插件了。

### dependency-management插件

``` gradle
apply plugin: 'io.spring.dependency-management'
```

当项目应用*io.spring.dependency-management*插件时，SpringBoot插件将会自动从项目中正在使用的Spring Boot版本中导入spring-boot-dependencies bom。为Maven用户提供了类似的依赖管理体验。

注意：当使用了该插件时，可以在引用spring-boot-starter-**项目时省略相应的版本号，默认使用与项目一致的版本号。

## 打包

SpringBoot插件可以创建可执行的归档文件（jar、war），其中包含了一个应用所需的全部依赖，并且能够使用*java -jar*命令来运行。

### 打包可执行的jar

``` shell
gradle bootJar
```

可执行jar可以使用bootJar任务来构建。该任务在应用java插件时自动创建，并且是BootJar的一个实例。assemble任务自动配置为依赖于bootJar任务，因此运行assemble（或built）也将运行bootJar任务

### 打包可执行的war

``` shell
gradle bootWar
```

可执行war可以使用bootWar任务来构建。该任务在应用war插件时自动创建，并且是BootWar的一个实例。assemble任务自动配置为依赖于bootWar任务，因此运行assemble（或built）也将运行bootWar任务

一个war文件可以被打包，以便它可以使用java -jar执行并被部署到一个外部容器。为此，应该将嵌入式servlet容器依赖项添加到providedRuntime配置中，列如：

``` groovy
dependencies {
  compile 'org.springframework.boot:spring-boot-starter-web'
  providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
}
```

这可以确保它们被封装在war文件的WEB-INF/lib提供的目录中，在哪里它们不会与外部容器的类冲突

### 同时打包可执行的和常规的归档文件

默认情况下，当配置bootJar或bootWar任务时，jar或war任务被禁用。通过启用jar或war任务，可以将项目配置为同时构建可执行归档文件和常规归档文件：

``` grovvy
jar {
  enabled = true
}
```

为避免可执行文件和常规归档被写入同一位置，应将其中一个或另一个配置为使用不同的位置。其中一种方式是通过配置分类器：

``` grovvy
bootJar {
  classifier = 'boot'
}
```

### 排除Devtools

默认情况下，Spring Boot的Devtools模块org.springframework.boot：spring-boot-devtools将被排除在可执行jar或war之外。如果您想在您的归档文件中包含Devtools，可以将将excludeDevtools属性设置为false，例子如下：

``` gradle
bootWar {
  excludeDevtools = false
}
```

### 配置需要解包的库

当嵌套在可执行归档文件中时，大多数库可以直接使用，但某些库可能有问题。要处理任何有问题的库，可配置为在运行可执行归档文件时将特定的嵌套jar包解压到临时文件夹，可以将库标识为需要使用与源代码文件的绝对路径相匹配的Ant样式模式进行解包

``` grovvy
bootJar {
  requiresUnpack '**/jruby-complete-*.jar'
}
```

为了更多的控制，也可以使用闭合。闭包传递一个FileTreeElement，并返回一个布尔值，指示是否需要拆箱

# 结束语

本文大部分内容都摘录自官方文档，但通过阅读文档依然收获良多，并记录下自己常用或者可能会用到的知识点。

# 参考

[Spring Boot的Gradle插件](https://docs.spring.io/spring-boot/docs/2.0.1.BUILD-SNAPSHOT/gradle-plugin/reference/html/)
