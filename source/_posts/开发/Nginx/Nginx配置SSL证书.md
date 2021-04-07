---
title: Nginx配置SSL证书
tags:
  - Nginx
categories:
  - 开发
  - 服务器
  - Nginx
abbrlink: ecc40a95
date: 2021-03-27 21:11:53
---


HTTP连接是不安全的，数据是明文传输的，如果有敏感数据这就直接暴露在互联网环境下是很危险的行为，因此为Nginx配置SSL证书，使用安全的HTTPS进行访问是非常有必要的。

<!-- more -->

# 环境

系统：CentOS 7

Nginx版本：1.16.1



# SSL证书

Nginx需要使用到的SSL证书分为了两部分：key和pem，这个证书是唯一的。

具体的获得方式要自己去域名服务提供商里找了，至于SSL是啥，我自己也很难说清楚，请自行百度吧。



# SSL配置方式

nginx配置启用SSL的方式非常简单，打开nginx的配置文件，一般在这个路径下

``` shell
vi /etc/nginx/nginx.conf
```

然后在server块中加入如下关键指令即可

``` shell
listen 443 ssl; # 443是默认的HTTPS端口 ssl指的启用SSL安全配置的意思，在较旧的版本中是单独写成ssl on形式
# 证书放在了/etc/nginx/cert目录下，由于cert目录与nginx.conf配置文件处于同级目录下，因此可以写成cert/，
# 如果是其它地方建议直接写成绝对路径 例如：/home/nginx/cert/cert-file-name.pem
ssl_certificate cert/cert-file-name.pem;  # ssl证书 需要将cert-file-name.pem替换成已上传的证书文件的名称。
ssl_certificate_key cert/cert-file-name.key; # ssl证书需要将cert-file-name.key替换成已上传的证书密钥文件的名称。

ssl_session_timeout 5m; # 会话的超时时间，m指分钟
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; # 加密算法
#表示使用的加密套件的类型。
ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # 表示使用的TLS协议的类型。
ssl_prefer_server_ciphers on; # 设置协商加密算法时，优先使用我们服务端的加密套件，而不是客户端浏览器的加密套件

```

具体示例

``` shell
http {
  ......
  server {
    listen 443 ssl;
    ssl_certificate cert/cert-file-name.pem; 
    ssl_certificate_key cert/cert-file-name.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    
    ......
    
    location / {
      ......
    }
  }
}
```



# 重定向

既然已经开启了SSL实现了HTTPS，那么普通的HTTP传输就不再需要了，但是每次输入网址都要刻意输入*https*还是很繁琐的，因此加入重定向配置

``` shell
http {
  ......
  server {
    listen 80; # http访问的端口
    ......
    rewrite ^(.*)$ https://$host$1; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
    ......
  }
  
  # 无效了。已经重定向永远到不了了
  server {
    listen 80;
    ......
  }
  
  # 有效
  server {
    listen 443 ssl;
    ......
  }
}
```

这样写后，所有的HTTP访问最终都会重定向到HTTPS，需要注意：这样配置后其它的监听80端口的配置就都失效了，因为都重定向了，需要更新相应的配置了。



写完配置文件后检查一下有没有写错（可省略）

``` shell
nginx -t
```

最后就是让nginx重新加载让配置生效

``` shell
nginx -s reload
```
