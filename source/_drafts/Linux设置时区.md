如果服务器在国外且所在时区不一致，那么使用`date`命令查看时间的话，会看到另外一个时区的时间，为了符合生活习惯需要设置新的时区。

假设一个场景：凌晨3点人少的时候重启服务器。这样写crontab定时任务就有点麻烦了，因为要以自己的时间计算服务器所在时区的时间，因此把服务器时区设置为自己生活所在的时区还是挺有必要的。

设置方法如下

``` shell
# 选择时区，在这里选了中国时区Asia/Shanghai
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 会提示
cp: overwrite ‘/etc/localtime’?

# 输入确认y
cp: overwrite ‘/etc/localtime’? y
```

PS1：这里只是使用了其中一种方法，其它设置方法可以参考[Linux如何设置时区、时间](https://blog.csdn.net/gezilan/article/details/79422864)。

PS2：如果时区对上了，但是具体的时间对不上，可能是没有使用ntp同步互联网时间。