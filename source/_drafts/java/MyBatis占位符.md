---
title: 'MyBatis占位符'
tags:
  - MyBatis
categories:
  - 框架
  - ORM
  - MyBatis
abbrlink: 365bf8ac
---

![MyBatis思维导图](../images/MyBatis占位符.png)

<!-- more -->

MyBatis共有两个占位符：**#{ }** 和 **${ }**。

# 用法

要在映射文件中使用占位符，首先需要用注解*@Param*指定可用的参数及其参数名，例：

``` Java
User findById(@Param("id") long id);

List<User> find(@Param("query") Query query);
```

**#{ }** 和 **${ }**均为通过参数名来获得对应的值，但是参数类型是基本类型或。

## 参数是基本类型

**#{ }**中的变量名可以是value或者其它名称，但**${ }**的变量名必须为value

## 参数是POJO类型

**#{ }** 和 **${ }**的使用方式一致，变量名是POJO中的属性.属性.属性....

# 应用场景

在实际开发中，能用**#{ }**的就用**#{ }**，原因如下：

* 性能方面：**#{ }**是预编译的，而相同的预编译SQL是可以重复利用的
* 安全方面：使用**${ }**有SQL注入的风险，而**#{ }**没有这方面的顾虑

# 深入理解

# SQL注入

# MyBatis执行顺序

1. 对SQL语句进行动态解析，主要包括：

  * 占位符的处理
  * 动态SQL的处理
  * 参数类型校验

2. SQL预编译

# SQL预编译

> sql 预编译指的是数据库驱动在发送 sql 语句和参数给 DBMS 之前对 sql 语句进行编译，这样 DBMS 执行 sql 时，就不需要重新编译。

JDBC 中使用对象 PreparedStatement 来抽象预编译语句，使用预编译

预编译阶段可以优化 sql 的执行。
预编译之后的 sql 多数情况下可以直接执行，DBMS 不需要再次编译，越复杂的sql，编译的复杂度将越大，预编译阶段可以合并多次操作为一个操作。

预编译语句对象可以重复利用。
把一个 sql 预编译后产生的 PreparedStatement 对象缓存下来，下次对于同一个sql，可以直接使用这个缓存的 PreparedState 对象。

mybatis 默认情况下，将对所有的 sql 进行预编译。

> #{}: 告诉 MyBatis 创建一个预处理语句（PreparedStatement）参数，在 JDBC 中，这样的一个参数在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中。

例子：
```
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>

String selectPerson = "SELECT * FROM PERSON WHERE ID=?";
PreparedStatement ps = conn.prepareStatement(selectPerson);
ps.setInt(1,id);
```

> ${}: 用于在SQL语句中插入一个不转义的字符串。MyBatis 不会修改或转义字符串。当 SQL 语句中的元数据（如表名或列名）是动态生成的时候，字符串替换将会非常有用。用这种方式接受用户的输入，并将其用于语句中的参数是不安全的，会导致潜在的 SQL 注入攻击，因此要么不允许用户输入这些字段，要么自行转义并检验

# 思维导图

![MyBatis思维导图](../images/MyBatis占位符.png)

# 总结

**#{ }**是预编译处理，**${ }**是字符串替换

使用${ }有SQL注入风险，要么不允许用户输入这些字段，要么自行转义并检验

# 参考

https://segmentfault.com/a/1190000004617028

https://www.cnblogs.com/huanyou/p/7201480.html
