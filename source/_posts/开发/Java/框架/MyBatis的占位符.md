---
title: MyBatis的占位符
tags:
  - MyBatis
categories:
  - 开发
  - 编程语言
  - Java
  - 框架
  - MyBatis
abbrlink: daf2aad3
date: 2021-04-03 22:32:18
---





代码中访问数据库的时候，经常需要传输参数用作语句条件，这就不得不用到MyBatis提供的占位符。



<!-- more -->



# 占位符：#{ } 和 ${ }

MyBatis中有两种占位符：**#{ }** 和 **${ }**，MyBatis对它们的处理方式是不同的：

* #{}是预编译处理：告诉 MyBatis 创建一个预处理语句（PreparedStatement）参数，在 JDBC 中，这样的一个参数在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中。
* ${}是字符串替换：用于在SQL语句中插入一个不转义的字符串。MyBatis 不会修改或转义字符串。当 SQL 语句中的元数据（如表名或列名）是动态生成的时候，字符串替换将会非常有用。用这种方式接受用户的输入，并将其用于语句中的参数是不安全的，会导致潜在的 SQL 注入攻击，因此要么不允许用户输入这些字段，要么自行转义并检验



## 用法

两种占位符的用法基本上是一致的，在Java代码中使用注解*@Param*，通过注解定指可用的参数及其参数名，例：

``` Java
// 参数是基本类型
User findById(@Param("demoId") long id);

// 参数是对象
List<User> find(@Param("demoQuery") Query query);
```

然后在xml配置文件中通过占位符传入参数，对于参数是基本数据类型获取方式如下

``` xml
select * from test where id = #{demoId};
select * from test where id = ${demoId};
```

对于参数是对象，需要显式指定需要传入的对象属性，例如

``` xml
select * from test where id = #{demoQuery.id};
select * from test where id = ${demoQuery.id};
```

稍微要提一下的是，虽然**#{ }** 和 **${ }**均为通过参数名来获得对应的值，但使用上还是有细微差别的。

对于参数是基本数据类型，**${ }**的变量名必须为@Param注解中指定的参数名，**#{ }**中的变量名可以是@Param注解中指定的参数名或者方法中的参数名，例如

``` java
// 参数是基本类型
User findById(@Param("demoId") long id);
```

``` xml
<!-- #{}中的变量名可以是@Param注解中指定的参数名或者方法中的参数名 -->
select * from test where id = #{id};
select * from test where id = #{demoId};

<!-- ${}的变量名必须为@Param注解中指定的参数名 -->
select * from test where id = ${demoId};
```



## 性能和安全

* 性能方面：**${ }**是字符串替换，**#{ }**是预编译的，而相同的预编译SQL是可以重复利用的，因此**#{}**性能更高。
* 安全方面：由于**${ }**是字符串替换，参数传入什么就使用的是什么，如果没有经过检查和转义的话会有SQL注入的风险，而预编译的**#{ }**没有这方面的顾虑

因此在实际开发中，能用**#{ }**的就用**#{ }**。



# 拓展



## MyBatis中SQL执行顺序

1. 对SQL语句进行动态解析，主要包括：
   * 占位符的处理
   * 动态SQL的处理
   * 参数类型校验

2. SQL预编译



## SQL预编译

MyBatis默认情况下，将对所有的 sql 进行预编译。

SQL预编译指的是数据库驱动在发送SQL 语句和参数给 DBMS 之前对 SQL语句进行编译，这样 DBMS 执行 SQL时，就不需要重新编译。

在Java中通过JDBC中的PreparedStatement实现预编译

``` java
String selectPerson = "SELECT * FROM PERSON WHERE ID=?";
PreparedStatement ps = conn.prepareStatement(selectPerson);
ps.setInt(1,id);
```

预编译有两个好处：

1. 优化SQl的执行：预编译之后的SQL多数情况下可以直接执行，DBMS 不需要再次编译，越复杂的SQL，编译的复杂度将越大，预编译阶段可以合并多次操作为一个操作。
2. 预编译语句对象可以重复利用：把一个SQL预编译后产生的 PreparedStatement 对象缓存下来，下次对于同一个SQL，可以直接使用这个缓存的 PreparedState 对象。



# 参考

https://segmentfault.com/a/1190000004617028

https://www.cnblogs.com/huanyou/p/7201480.html
