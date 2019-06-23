---
title: Conda
categories:
  - 工具
abbrlink: c3592b96
date: 2018-05-20 10:34:06
tags:
---

Conda是一个包管理、依赖管理和*环境管理*工具，虽然由Python程序创建，但它可以打包并分发任何语言的软件，例如：Python, R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, FORTRAN。

<!--more-->

# Conda简介

> Conda是一个开源的包管理、依赖管理和环境管理工具，能运行在Windows、macOS和Linux上，具备快速安装、运行和更新包和依赖的能力，能够轻松的创建、保存、加载和切换不同的开发环境。虽然Conda由Python程序创建，但它可以打包并分发任何语言的软件。

通过以上摘录自官网的译文可以知道，Conda对于经常要切换不同的开发环境的开发者来说，是一个极其合适的工具。

# 下载

Conda作为一个管理工具，有两个常见的发行版本：Anaconda和Miniconda，其区别如下：

Miniconda：仅仅拥有python，conda和一些必须的依赖包,除此之外没有任何附带的东西，操作时需要使用命令行的方式进行操作，占用极小的硬盘空间，但使用的时候需要自己安装所需要的包。[>>>下载<<<](https://conda.io/miniconda.html)

Anaconda：拥有Miniconda所拥有的之外，还自带超过720个的开源包，并且具有图形化的操作界面，安装后即可使用，但占用较大的空间，以及会安装一些自己使用不到的包。[>>>下载<<<](https://www.anaconda.com/download/)

综上来看：两个发行版该有的基本功能一样不缺，但相对而言，Miniconda更适合一些简约的人使用，如果需要省心的话Anaconda就是一个极好的选择，毕竟多占的一些硬盘空间对现在的硬盘来说不算什么，读者可以根据自身情况进行选择。

博主比较喜欢简约因此使用的是Miniconda，同时为了更好的熟悉conda，下面将会直接使用命令行来进行操作演示。

# 环境变量

在安装conda的过程中，其中有一个选项就是把conda加入系统环境变量中，官方提示是不建议勾选，因为重装之类的可能会导致找不到相应目录之类的，建议直接使用官方提供的命令行工具，在命令行下找到一个名为*Anaconda Prompt*的应用，如下所示：
![Anaconda Prompt](/images/conda/conda_prompt.png)
运行后的效果如下所示：
![Anaconda Prompt CMD](/images/conda/conda_cmd.png)
但是经过一段时间的使用后发现还是加入到系统的环境变量中更方便，这样就可以随时随地在任何环境中使用了，如果已经安装了程序但又没有加入系统环境变量的，可以按照如下步骤重新加入：
1. 找到程序的安装根目录，要是安装时没有指定安装目录的话，一般都是安装在安装时使用的用户的用户目录之下，这里假设路径是conda_rootdir
![conda安装根目录](/images/conda/conda_rootdir.png)
2. 打开系统环境变量的设置窗口：我的电脑点击鼠标右键->属性->高级系统设置->环境变量
![系统环境变量](/images/conda/system_path.jpg)
3. 在系统变量部分，找到变量名为Path的条目，选中并点击下方的编辑按钮，就能打开编辑界面
![系统环境变量编辑窗口](/images/conda/system_variable_edit.png)
4. 把如下的路径添加到系统环境变量中，记得把conda_rootdir替换为自己电脑下conda的安装根目录
![conda路径](/images/conda/conda_path.png)
5. 点击确定，保存编辑的内容，然后重新打开命令行窗口，然后输入命令*conda info*，并显示相应内容的话，就表示配置成功，配置失败的请重新配置
![conda信息](/images/conda/conda_info.png)

# conda配置文件

conda安装后会在用户目录下生成一个配置文件*.condarc*，如果没有的话可以自己创建一个，内容如下：
``` yaml
channels:
  - defaults
```
配置文件中的channels属性指的是安装时的下载来源，defaults指的是默认的下载来源，如果想添加下载源的话只要按照格式添加就可以了。默认源在国内下载速度较慢，所以国内有清华镜像源即[Anaconda 镜像](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)，使用后速度会有明显的提升，但是在这里依然不建议使用国内的清华源，因为使用一段时间后发现清华源的版本比较旧而且还不全，直接使用默认源的话使用上是基本没有问题，下载时间也就相对清华源慢一些而已。

默认情况下环境文件以及下载下来的包都是安装在软件根目录下的envs和pkgs文件夹下，如果想要更改安装路径的话，可以加入如下配置
``` yaml
# 环境目录
envs_dirs:
  - E:\Documents\Code\alpha\conda\envs

# 包目录
pkgs_dirs:
  - E:\Documents\Code\alpha\conda\pkgs
```

如果有想了解更多的可以直接访问官网了解详细的配置[>>>传送门<<<](https://conda.io/docs/user-guide/configuration/use-condarc.html)

# Conda信息

如果想要查看Conda的信息的话，可以输入如下命令
``` shell
conda info
```
![conda信息](/images/conda/conda_info.png)
可以看到上一部分的配置信息也出现在内容展示中，说明配置是有效的。

如果想要查看已有的开发环境，输入如下命令
``` shell
conda info --env
```
![conda环境信息](/images/conda/conda_info_env.png)
可以看到里面现在有两个环境，分别是base和tensorflow，其中base就是安装时默认的环境，而tensorflow就是博主自己创建的环境。

在上述图片中还有个要注意的地方就是base环境前面是有个\*号的，说明base环境处于激活的状态，即判断当前所处环境的方法就是查看\*号出现在那个环境前。
![激活了tensorflow环境](/images/conda/activate_tensorflow.png)
而上面这张则是激活了tensorflow环境的，而这张还有一个特别地方就是前面直接写着名为tensorflow的环境名称，因此也可以作为当前处于那个激活环境的判断。

那么到底怎么创建一个新的环境呢？

# 环境创建、克隆、激活和删除

想要创建环境，只要输入如下命令：
``` shell
# 不指定python版本
conda create --name env-name

# 指定python版本
conda create --name env-name python=2.7
```
其中env-name是该新建环境的名称；python是该环境中的python版本，默认情况下如果不写该参数，将会延用base环境中的python版本，而base环境中的python版本就是下载部分选择conda安装包时的python版本。
![conda创建环境](/images/conda/conda_create.png)
创建成功后就能看到如上的显示，想要激活环境，输入：
``` shell
conda activate env-name
```
想要取消激活，输入：
``` shell
conda deactivate
```
当然，也可以直接关闭命令窗口来退出已激活的环境。

注意：在没有激活任何环境的情况下都是处于base环境之下的，就算激活了其它环境也只在激活的命令窗口下是激活环境，在激活环境下的任何操作，都不会对其它环境产生影响，当然这也就是conda的意义所在。

在某些情况下如果想要复制已有的环境，可以输入：
``` shell
conda create --name new-env-name --clone env-name
```

如果想删除现有的环境，必须在先取消激活目标环境，然后执行如下命令：
``` shell
conda remove --name env-name --all
```

# 包的查看、搜索、安装、更新和卸载

注意：以下的一切命令请先激活环境后再执行，否则都将会作用于基本环境，即操作的对象是基本环境。

创建好环境后，想要使用当然还需要安装自己所需要的包，这里使用tensorflow做例子，输入如下命令，查看当前已安装的包：
``` shell
conda list
```
![查看已安装的包](/images/conda/conda_list.png)

搜索可用的包
``` shell
conda search tensorflow
```
![搜索可用的包](/images/conda/conda_search.png)

安装包
``` shell
conda install tensorflow
```

更新包
``` shell
conda update tensorflow
```

卸载已安装的包
``` shell
conda uninstall tensorflow
```

# 环境配置的导入和导出

在某些情况下，如果想要分享自己的开发环境，可以通过导出目标环境的环境配置来完成分享。
``` shell
conda activate tensorflow

conda env export > path-to/environment.yml
```
注意：要先激活想要的分享目标环境，然后再执行导出命令

导出命令的path-to是指定目标路径，environment.yml则是导出的环境配置文件，如果不写路径则将会导出到命令行当前的路径之下。

别人拿到了你分享出来的环境配置文件又该如何使用呢，输入：
``` shell
conda env create -f path-to/environment.yml
```
即可创建一个和环境配置文件描述的一摸一样的开发环境了。

# 结束语

至此，conda的基本用法已经讲述完，如过想要更深入了解的话可以在官网上查阅更详细的资料。

# 参考文献

[conda官网](https://conda.io/docs/index.html)
