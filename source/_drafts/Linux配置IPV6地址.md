一般的服务器都是没有提供IPV6地址的，但是有时候我们又会需要使用到，例如访问谷歌经常会弹出人机验证非常讨厌，这时候使用IPV6进行访问就不需要再验证了。



# 环境

系统：CentOS 7



# 免费IPV6

这是一个免费资源，是由HE（HURRICANE ELECTRIC）提供的，通过技术手段连上HE提供的服务器，实现IPV4转IPV6，我们就间接有了IPV6访问的能力了。

具体的获取教程请参考[HE的IPV6注册](https://zhuanlan.zhihu.com/p/344450513)，目的是在HE中获取如下的配置内容：

``` shell
modprobe ipv6
ip tunnel add he-ipv6 mode sit remote 216.218.200.58 local 替换你的ipv4地址 ttl 255
ip link set he-ipv6 up
ip addr add 替换获得的ipv6地址 dev he-ipv6
ip route add ::/0 dev he-ipv6
ip -f inet6 addr
```



# 自动配置IPV6

如果直接执行上述的命令，由于这些命令都是临时生效的，每次重启服务器都需要重新执行一次。因此我们要设置为自动执行。操作过程如下

1. 首先新建一个脚本ipv6.sh，脚本示例如下

``` shell
# 当前目录是：/home/demo 文件具体路径：/home/demo/ipv6.sh
# 创建脚本ipv6.sh
vi ipv6.sh

# 脚本内容如下
#!/bin/bash

modprobe ipv6
ip tunnel add he-ipv6 mode sit remote 216.218.200.58 local 替换你的ipv4地址 ttl 255
ip link set he-ipv6 up
ip addr add 替换获得的ipv6地址 dev he-ipv6
ip route add ::/0 dev he-ipv6
ip -f inet6 addr
```

2. 保存退出后，赋予执行权限

``` shell
# 授予执行权限
chmod +x ipv6.sh
```

3. 在rc.local文件中加入执行ipv6.sh的命令，最后保存退出，等到下次重启服务器就能看到效果了。

``` shell
# 编辑rc.local脚本
vi /etc/rc.local

# 加入如下内容，记得替换成自己的路径
/home/demo/ipv6.sh
```

rc.local脚本，是Linux系统自带的脚本，每次系统启动时都会执行一遍脚本中的命令，我们就是通过这个机制来自动执行ipv6设置。



# 检查配置是否生效

想要立刻重启服务器，可以使用命令*reboot*，重启后输入网络查看命令*ifconfig*，找到*he-ipv6*这个网络接口，如果能找到就代表配置成功了。

``` shell
# 重启服务器
reboot

# 查看网络信息
ifconfig

# 找到he-ipv6这个名字，这个名字是在上面的配置中名命的，不是自动生成的
he-ipv6: flags=209<UP,POINTOPOINT,RUNNING,NOARP>  mtu 1480
        inet6 ipv6地址  prefixlen 64  scopeid 0x0<global>
        inet6 ipv6地址  prefixlen 64  scopeid 0x20<link>
        sit  txqueuelen 1000  (IPv6-in-IPv4)
        RX packets 1575  bytes 555832 (542.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1654  bytes 407621 (398.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```



# 使用IPV6访问

尝试使用IPV6访问一下谷歌

``` shell
# ping6 就是用ipv6进行访问
ping6 google.com

# 提示错误
connect: Network is unreachable
```

（这是我猜的）应该是由于缺少IPV6域名地址的解析能力，无法正确获得IPV6地址，因此访问失败。因此通过在hosts文件中加入谷歌的IPV6地址，尝试绕过解析这一步骤

``` shell
# 修改host文件
vi /etc/hosts

# 加入谷歌的IPV6地址
2404:6800:4008:c00::71 google.com
2404:6800:4008:c02::63 www.google.com
```

修改完hosts文件后要重新加载网络信息才能生效

``` shell
# 重启网络
systemctl restart network
```

由于上一步重启了网络，因此要重新配置IPV6，执行一下ipv6.sh脚本即可

``` shell
/home/demo/ipv6.sh
```

再次访问

``` shell
ping6 google.com

PING google.com(google.com (2404:6800:4008:c00::71)) 56 data bytes
64 bytes from google.com (2404:6800:4008:c00::71): icmp_seq=1 ttl=107 time=224 ms
64 bytes from google.com (2404:6800:4008:c00::71): icmp_seq=2 ttl=107 time=224 ms
64 bytes from google.com (2404:6800:4008:c00::71): icmp_seq=3 ttl=107 time=223 ms
```

这次就成功了，其中的IPV6地址就是我们在hosts文件中填的那个。



# shadowsocks使用IPV6

使用shadowsocks有个重要用途就是使用谷歌搜索，但是如果IP或者IP段被谷歌标记了的话，经常会弹出人机验证非常讨厌，通过使用IPV6的话就可以避开这个验证。

shadowsocks使用IPV6非常简单，简单的加入如下配置即可

``` shell
# 编辑配置文件
vi /etc/shadowsocks-libev/config.json

# 加入配置项
"ipv6_first":true
```

然后重启shadowsocks服务即可。

