---
title: Nginx配置为代理服务器
tags:
  - 服务器
  - Nginx
categories:
  - Nginx
abbrlink: 55e0da5c
date: 2021-03-27 21:11:43
---

Nginx作为代理服务器是一个很常见的用途，例如内部有一个tomcat服务但是又不想开放8080端口，通过配置代理可以在只开放80端口的情况下访问到内部的tomcat服务了。

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

```shell
# proxy_set_header作用是设置请求头，代理会使部分信息丢失，因此需要重新设置方便获得真实访问信息
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

# 关键 proxy_pass 代理转发需要到达的目的地，可以是其它任何想要访问的地址
proxy_pass http://127.0.0.1:3000/;
```

具体示例

```shell
http {
  ......
  server {
    ......
    location / {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://127.0.0.1:8080/;
  }
}
```

写完配置后检查一下有没有写错（可省略）

```shell
nginx -t
```

最后就是让nginx重新加载让配置生效

```shell
nginx -s reload
```