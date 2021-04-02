Git提交代码到远程的时候显示异常信息“ServicePointManager 不支持具有 socks5 方案的代理”，虽然代码是正常提交了，但这是怎么回事？



<!-- more -->



# 问题分析

异常信息如下

``` shell
fatal: NotSupportedException encountered.
   ServicePointManager 不支持具有 socks5 方案的代理。
```

提取关键词：*socks5*和*代理*，这两个词有点熟悉，不就是科学上网时经常遇到的字眼吗！

再看看git配置

``` shell
git config -l

# 显示信息如下
......
http.proxy=socks5://127.0.0.1:1080
```

看来问题就是这里了，



# 前置问题：Connection refused

在提交代码的时候由于我之前配置了



fatal: unable to access 'https://github.com/izhixing/izhixing.github.io.git/': Failed to connect to 127.0.0.1 port 1080: Connection refused




Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 3.86 KiB | 988.00 KiB/s, done.
Total 6 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.