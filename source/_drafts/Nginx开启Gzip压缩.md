有些资源文件的体积比较大，开启nginx的gzip压缩，能够极大的提升传输的效率。



# 环境

系统：CentOS 7

Nginx版本：1.16.1



# 配置方式

nginx配置为文件服务器的方式非常简单，打开nginx的配置文件，一般在这个路径下

``` shell
vi /etc/nginx/nginx.conf
```

然后在http、server、location这三个任一代码块（只是影响了生效的范围）中加入如下关键指令即可

``` shell
#开启gzip压缩 默认关闭off
gzip on;
#IE版本1-6不支持gzip压缩，关闭
gzip_disable 'MSIE[1-6].';
#需要压缩的文件格式 text/html默认会压缩，不用添加
gzip_types text/css text/javascript application/javascript image/jpeg image/png image/gif; 
#给响应头加个vary，告知客户端能否缓存
gzip_vary on; 
# 默认值是 4 4k/8k，设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。 例如 4 4k 代表以4k为单位，按照原始数据大小以4k为单位的4倍申请内存。 4 8k 代表以8k为单位，按照原始数据大小以8k为单位的4倍申请内存。
gzip_buffers 4 8k;
# 压缩等级1~9，默认值1，1级别最低压缩速度最快但是压缩比最小，9级别最高压缩速度最慢但压缩比最大，建议根据自己情况测试一下那个级别适合自己，偷懒的话网上看的大多数都是设成1或者4
gzip_comp_level 1; 
# 默认值0 文件大小大于该值时才进行压缩，单位为Byte，写1k即1024B，当值为0时所有页面都进行压缩，建议设置成大于1k的字节数
gzip_min_length 1k; 

# 这个不熟悉
gzip_proxied # 默认关闭off 用于反向代理的时候是否开启压缩，具体参数建议看下文的参考文章
```

具体示例

``` shell
http {
  ......
  server {
    ......
    location / {
      gzip on;
      gzip_disable 'MSIE[1-6].';
      gzip_types text/css text/javascript application/javascript image/jpeg image/png image/gif; 
      gzip_buffers 4 4k;
      gzip_comp_level 1;
      gzip_min_length 20;
      gzip_vary on; 
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



# 参考

[Nginx Gzip模块中文参考](https://www.nginx.cn/doc/standard/httpgzip.html)

