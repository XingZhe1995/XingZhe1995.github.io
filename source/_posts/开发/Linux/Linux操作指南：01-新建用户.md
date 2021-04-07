---
title: Linux操作指南：01-新建用户
tags:
  - Linux
categories:
  - 开发
  - Linux
abbrlink: 1eb05570
date: 2020-01-10 22:43:20
---


在Linux上root用户拥有最高的权限，但是多人共享root账户或者直接使用root账户都是一件危险的事情，因此新建普通用户给其它操作员是一件顺理成章的事情。

<!-- more -->

**注：演示使用的Linux操作系统是Centos 7**

# 用户操作

## 新增用户

全新的Linux系统默认都是只有root账户，因此通过root用户来创建用户，当然也可以使用具备新建用户的权限的用户来创建用户

``` shell
useradd test
```

此时通过查看命令即可看到home目录下多了个test目录，即代表用户test已经成功创建

``` shell
# ls -l 查看命令
ls -l /home
drwx------  2 test   test   4096 Jan 10 21:32 test
```

# 修改用户密码

使用*useradd*命令创建用户后，此时由于没有设置密码，该新建的账户是无法登陆使用的，因此需要给该新账户添加密码（注：修改密码使用的是同一个命令）

``` shell
# passwd 修改密码命令
passwd test
New password: 
```

此时即可输入新密码了，但是会发现好像一直按键盘都是空的，这就是Linux的安全策略，光给密码打码还不够安全，连长度都不知道才是真的安全，输入完密码按回车键即可

``` shell
passwd test
New password: 
Retype new password: 
```

输入完第一次后还会要求你再重复输入一次，以防你输入错误的密码

``` shell
passwd test
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

两次输入密码成功后会出现成功的提示信息，此时新建账户已经可以使用了。

但是有时候可不是这么顺利，比如说输入的密码太短：

``` shell
passwd test
New password: 
BAD PASSWORD: The password is shorter than 8 characters
```

或者输入的密码中包含用户名

``` shell
passwd test
New password:
BAD PASSWORD: The password contains the user name in some form
```

又或者输入的密码太简单

``` shell
passwd test
New password:
BAD PASSWORD: The password fails the dictionary check - it is too simplistic/systematic
```

以上的几种情况都会导致输入的密码无效，要重新输入新的密码。

## 删除用户

新建的用户在使用一段时间后可能会面临删账户的情况，使用如下命令:

``` shell
userdel test
```

但是这样只是仅仅删除了账户，而home目录下的账户目录是没有被删除的，因此可以选择加入参数*-R*在删除账户的同时删除对应的用户目录。

``` shell
userdel -R test
```

# 总结

以上就是在Linux中关于账户的一些常用操作。
