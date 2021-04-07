---
title: Nginx配置为文件服务器
tags:
  - Nginx
categories:
  - 开发
  - 服务器
  - Nginx
abbrlink: 4eef1a76
date: 2021-03-27 21:11:43
---


我们经常会遇到从远程服务器下载文件的情况，如果直接使用ftp或者sftp的进行下载的话，下载速度总是不甚满意，网络差的话简直让人抓狂要砸键盘了。这情况可以考虑用Nginx做文件服务器，然后使用IDM（设置为32个线程同时工作）下载，那下载速度可是杠杠的带宽都要跑满了。

<!-- more -->

# 环境

系统：CentOS 7

Nginx版本：1.16.1



# 配置方式

nginx配置为文件服务器的方式非常简单，打开nginx的配置文件，一般在这个路径下

``` shell
vi /etc/nginx/nginx.conf
```

然后在location块中加入如下关键指令即可

``` shell
语法:	autoindex on | off;
描述：开启或禁用目录浏览功能，默认是禁用

语法：autoindex_exact_size off|on;
描述：默认为on，显示出文件的确切大小，单位是bytes。一般会改为off，显示出文件的大概大小，单位是kB或者MB或者GB

语法：autoindex_format html | xml | json | jsonp
描述：设置目录列表的格式，默认是html

语法：autoindex_localtime on|off;  
描述：on显示文件的本地时间（服务器时间），否则显示文件的GMT时间（如果不是在0时区，则会有对不上时间的烦恼），默认是off
```

具体示例

``` shell
http {
  ......
  server {
    ......
    location / {
      root /home/demo/download; # 可下载文件的路径
      autoindex on; # 开启目录浏览功能 最关键的一个一定要填
      autoindex_exact_size off; # 不显示文件的大小
      autoindex_format html; # 目录列表的格式是html
      autoindex_localtime on; # 使用本地时间
  }
}
```

写完配置文件后检查一下有没有写错（可省略）

``` shell
nginx -t
```

最后就是让nginx重新加载让配置生效

``` shell
nginx -s reload
```



