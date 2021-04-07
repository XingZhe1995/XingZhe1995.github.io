---
title: Hexo博客搭建
tags:
  - 其它
categories:
  - 开发
  - 其它
abbrlink: b8f4bd70
date: 2018-03-04 23:08:15
---


本篇文章主要讲述的是搭建**Hexo + Github Page + Travis CI**的博客环境。

目前有许多搭建个人博客的方案，但是使用Github Page来展示自己的个人博客，无疑是一个较好的方案，无需额外的服务器、免费、自带CDN、可靠并且自带网站域名，这种种的优势足以让人做出选择，然后加上美观的主题，就可以搭建出一个完整的博客系统了，当然要是有点懒的话，加上Travis CI进行自动部署，就可以专注于博客文章的编写了。

<!-- more -->

*开始前的题外话：在这里我已假设你拥有git的使用知识。*

# 什么是Hexo

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

由上述Hexo官网引用可以知道，Hexo是一个博客框架，通过解析Markdown文件，把Markdown文件中的文章内容进行渲染并生成我们所看的网页，即我们通常看到的博客文章。

使用Hexo之前![使用Hexo之前](https://ws1.sinaimg.cn/large/e6dffef4gy1g18iaybxqnj20vc0buweu.jpg)

使用Hexo之后![使用Hexo之后](https://ws1.sinaimg.cn/large/e6dffef4gy1g18ie323nyj20os0ef3zl.jpg)

通过以上的配图可以看到，使用Hexo的前后的变化是很大的。

# GitHub配置

> [Github Pages](https://pages.github.com/)主要是用于个人、组织、或者项目的网站，直接把网站代码托管在GitHub仓库上，别人就能访问到该网站，你只需要编辑并推送代码，你对网站的修改就能生效。  
*注意:GitHub Pages仅支持静态网站。*

## 注册GitHub账号

使用**GitHub Pages**来部署博客代码，需用到GitHub仓库，因而拥有一个GitHub的账号是必须的。[>>>传送门<<<](https://github.com/join)

## 创建博客代码仓库

根据GitHub Pages的官网介绍，GitHub Pages主要用于个人网站、组织网站和项目网站，它们的配置如下：

|网站类型|网站域名与仓库名|分支名|
|:---|:---|:---|
|个人网站|username.github.io|master|
|组织网站|orgname.github.io|master|
|个人所拥有的项目|username.github.io/projectname|master、gh-pages或者master分支下的docs文件夹下|
|组织所拥有的项目|orgname.github.io/projectname|master、gh-pages或者master分支下的docs文件夹下|

由上面的表格可以看到，对不同类型的网站，在仓库的命名和推送的目标分支仅有略微的差别，而对于要搭建个人博客的我们，关注点就在**第一行**上*（当然，聪明的你完全可以依照本篇文章的方法为自己的项目搭建一个网站出来）*。

参照上面的配置表格，根据自己GitHub上的用户名*username*，创建一个仓库并命名为：**username.github.io**  
与此同时，你的个人网站也拥有了相应的域名也就是仓库名*username.github.io*，以后你或者其他人就能通过该域名访问到你的网站。

## master分支与hexo分支

由上面的表格可以了解到，对于个人网站而言，网站代码必须存放于master分支上，GitHub Pages才能生效，而对于我们生成博客网站前的博客代码，我则存放于hexo分支上（读者可以放到其它地方或者其它分支上，只要最后博客网站代码正确推送到username.github.io仓库的master分支上即可）。  
**master分支：存放最终生成的博客网站代码；**  
**hexo分支：存放博客源代码；**

# 博客配置

在这里我选择了Hexo做为博客框架，当然如果感兴趣的读者也可以研究一下其它的博客框架。

## Hexo博客框架

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

Hexo博客框架依赖于node.js来运行并生成静态页面。

### 安装node.js

在这里建议下载稳定版LTS（[>>>下载传送门<<<](https://nodejs.org/en/)），以提供一个相对稳定的博客编写环境。下载并安装后，打开命令行界面，输入命令*node -v*，如果安装成功将会返回版本信息，如下图所示：

``` shell
node -v
8.9.4
```

### 安装Hexo博客框架

打开命令行界面，输入如下命令，安装全局的Hexo命令，方便后续的博客编写操作：

``` shell
npm install -g hexo-cli
```

然后输入如下命令，安装hexo博客框架代码：

``` shell
hexo init 这里写存放博客代码的路径
```

进入博客代码文件夹内，安装Hexo博客框架运行所需的依赖：

``` shell
npm install
```

至此一个可运行的Hexo博客框架就搭建完成了，如果需要了解更多关于Hexo的使用方法，可点击[>>这里<<](https://hexo.io/zh-cn/docs/setup.html)。

## Next博客主题

博客主题对于博客来说是极其重要的，在这里我选择了使用Next主题，Next主题拥有众多的开发者，也被众多的博客所使用，因而能够保持着足够的活力，同时也能够在遇到问题的时候寻求到帮助。

为了方便余下篇幅的描述，作出以下约定：

1. 站点配置文件：Hexo博客根目录下一个名为*_config.yml*的文件，里面存放着整个博客站点的配置信息；
2. 主题配置文件：在Hexo博客所引用的主题文件里也存在着一个名为*_config.yml*的文件，但与站点配置文件不同的是里面存放的是该主题的配置信息；
3. 主题文件夹：Hexo博客根目录下有一个thems文件夹，里面存放着博客的主题资源；

### 主题安装基本步骤

#### 下载主题

关于主题的具体下载方式，在这里暂且跳过，后续由专门的章节来进行讲解。  
在这里假设我们已经下载好主题文件，并把主题文件夹命名为*next*（这里你可以命名为任何合法的名字，只需要在下一个步骤里填入相同的名字就可以了），把主题文件放置在*主题文件夹*内。

#### 切换主题

Hexo博客在博客创建的时候就已经自带了一个主题，而要切换为自带的博客主题，就需要修改*站点配置文件*，如下所示：

``` yaml
theme: next
```

到这里博客就能够使用新的博客主题了。

### 主题下载方式

主题的下载方式决定了后续过程中对该主题进行个性化定制后升级的难易程度。

#### 直接下载

直接下载，顾名思义就是直接下载整个主题，使用这种方式有一个弊端：在主题升级或者修复bug之后，主题的升级都需要去主题站点重新下载整个主题并替换掉旧的主题，费时费力；其次对于高度个性化定制主题之后，更是难以进行升级操作。因而**强烈不推荐这种方式**。

#### git clone下载

使用*git clone*方式下载主题至主题文件夹内，是主题发布方的推荐方式，该种方式能够及时且方便的获取主题的最新版本，并且能够在高度个性化定制主题后能够轻松的保留更改的同时升级主题。

主题下载：进入博客根目录，打开命令行界面（*注意路径为博客根目录*），执行如下命令

``` shell
git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```

执行结束后主题文件夹下就多了一个叫next的文件夹，同时里面也存放有主题相关的资源文件。

主题更新：进入Next主题文件夹，打开命令行界面（*注意路径为博客根目录*），执行如下命令

``` shell
git pull origin master
```

如果更新遇到问题，可以参考[>>>这里<<<](https://github.com/theme-next/hexo-theme-next)

#### git子模块下载

使用*git submodule*方式下载主题，是无法对主题内容进行高度个性定制化的，仅仅能够在Next主题允许的范围内对主题进行个性定制，优点就是在保留一定程度的定制化的同时能够轻松对主题进行升级（比*git clone*方法还要方便）。

主题下载：进入博客根目录，打开命令行界面（*注意路径为博客根目录*），执行如下命令

``` shell
git submodule add https://github.com/theme-next/hexo-theme-next.git themes/next
```

执行结束后主题文件夹下就多了一个叫next的文件夹，同时里面也存放有主题相关的资源文件。

主题更新：进入博客根目录，打开命令行界面（*注意路径为博客根目录*），执行如下命令

``` shell
git submodule update --remote
```

使用该种方式对主题的定制化，是通过在站定配置文件追加*theme_config*字段实现的，只要是主题配置文件中存在的字段就能够在*theme_config*字段下使用(*注意缩进*)，并且作用到主题中。例子：

``` yaml
theme_config:
  # 主题
  scheme: Mist
  # 头像
  avatar: /images/avatar.png
  # 菜单
  menu:
    home: / || home
    tags: /tags/ || tags
    categories: /categories/ || th
    archives: /archives/ || archive
```

### 数学公式渲染

写博客的过程中可能会遇到需要编写数学公式的情景，要是使用图片来代替的话就不太优雅了，这时候可以考虑使用数学渲染引擎来直接渲染出数学公式了，幸运的是Next主题已经替我们准备好了。Next主题提供了两个渲染引擎：mathjax和katex，mathjax渲染速度较慢，但是支持的范围较广，katex渲染速度快，但是支持的范围较窄，因此博主选择了mathjax，以下内容也以mathjax为例，如果有需要的读者可自行查询文档：[katex安装](https://github.com/theme-next/hexo-theme-next/blob/master/docs/MATH.md)

首先卸载hexo自带的渲染工具
``` shell
npm un hexo-renderer-marked --save
```

然后安装新的渲染工具，根据官方文档说，mathjax只能在hexo-renderer-kramed和hexo-renderer-pandoc中二选一，在这里博主选择了hexo-renderer-kramed
``` shell
npm i hexo-renderer-kramed --save # or hexo-renderer-pandoc
```

在站点配置文件中，写入如下配置
``` yaml
theme_config:
  math:
    enable: true
    engine: mathjax
    mathjax:
      cdn: //cdn.jsdelivr.net/npm/mathjax@2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML
```
即可使博客网站具备渲染数学公式的能力，更多的配置可在主题配置文件（Next/_config.yml）的math部分查看。

然后在需要进行数学公式渲染的markdown文件中，开头部分加入mathjax: true，即可让数学渲染引擎对该文件进行渲染，如果不需要进行数学渲染的切记不要添加，因为会有损性能。
``` markdown
---
title: 
date: 
tags:
categories:
mathjax: true
---
```
如果成功了的话，就能看到你现在所看到的数学公式了
$$
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
$$

# 自动部署

一篇新的博客文章需要经历三个步骤才能出现在博客中：

1. 写文章
2. 生成GitHub Pages用的博客代码（hexo generate）
3. 提交上述的生成的博客代码（git push）

要是每次写一遍新的文章都要经历如上三个步骤多繁琐呀！要是能把第二个步省略掉，自己只专注于写博客，然后提交新的文章，文章就能自动出现在博客中那该多好呀，使用Travis CI就能达到这个目的，替我们完成这与创作无关的步骤。

## GitHub权限配置

使用Travis CI来自动部署网站代码，必须对代码仓库进行设置，同时需要授予Travis CI相应的权限，而GitHub已经提供了相应的开发者接口，并且有一套完善安全的接口调用方法。

### 获取Access Token

1. 登陆GitHub
2. 进入个人设置页面（点击自己的头像图标->Settings）
3. 点击左侧栏下的Developer settings（Personal settings->Developer settings）
4. 点击Personal access tokens
5. 这个时候就能看到如下图所示的界面，点击Generate new token按钮![Generate new token界面](/images/blog/generatenewtoken.png)
6. 填写Token description（凭据描述,方便以后自己的辨别），选中repo下的publish_repo选项（授予Travis CI的权限），如下图所示：![New Personale Access Token](/images/blog/newpersonalaccesstoken.png)然后点击下方的Generate token（保存设置）
7. 点击保存后回到步骤5的界面，这时候会显示一串字符，这就是Travis CI所需要的凭据**（注意：立刻保存好这串字符，因为该字符串仅在第一次显示，并且为了自己的代码安全不要告诉他人这串字符的任何信息）**![Access Token](/images/blog/accesstoken.png)

### GitHub仓库配置

1. 登陆GitHub
2. 进入*username.github.io*仓库
3. 点击仓库上方的Settings按钮![Settings](/images/blog/reposetting.png)
4. 点击左侧栏的Integrations & services->点击右侧的Add services按钮->输入并选中Travis CI，如下图所示![Add Services](/images/blog/addservice.png)
5. 点击下方的绿色Add service

至此，在GitHub方面的配置就完成了。

## Travis CI

> Travis CI是一个提供持续集成的服务，能够与GitHub很好的结合起来。Travis CI对于GitHub的开源仓库是免费的，对于私有仓库是需要收费的。

### Travis CI注册

要使用Travis CI当然要先注册一个账号[>>>传送门<<<](https://www.travis-ci.org/)，而Travis CI允许使用GitHub的账号作为第三方授权登陆。

### Travis CI配置

当使用GiHub授权登陆Travis CI的时候，Travis CI就能感知到我们仓库的状态，这时候就要设置究竟是那个仓库要进行持续集成了。

1. 点击头像图标进入如下界面：![GitHub仓库列表](/images/blog/tciselectrepo.png)
2. 选中username.github.io仓库（按钮变为绿色）
3. 点击旁边的小齿轮，进入仓库的具体设置页面
4. 点击右侧的more options->选择Settings![Travis CI Setting header](/images/blog/tcisettingheader.png)
5. 按下图选中对应的选项![Travis CI Setting Content](/images/blog/tcisettingcontent.png)
6. 在Environment Variables部分中，写入环境变量GITHUB_TOKEN，值为上述提到的Token，然后点击add按钮添加

至此在Travis CI的配置就完成了。

## 博客源代码配置

Travis CI服务的使用，除了需要在Travis CI和GitHub中进行配置外，还需要在博客根目录下添加一个名为.travis.yml的文件，这个配置文件告诉了Travis CI如何工作，配置文件如下：

``` yaml
# hexo博客框架是依赖于node.js环境来运行的
language: node_js
# 指定node.js的版本
node_js: stable
# 缓存依赖文件，提高构建的速度
cache:
  directories:
    - "node_modules"
# 指定对什么分支才进行构建
branches:
  only:
  # 这里替换成自己的分支
    - your-github-branch
# 安装hexo所需要的依赖
install:
  - npm install
# 构建时执行的动作，这里是生成博客的网站代码
script:
  - hexo g
# 推送网站代码到github
deploy:
  provider: pages
  skip-cleanup: true
  # 这里引用上一步中所填入的Token
  github-token: $GITHUB_TOKEN
  keep-history: true
  # 将要推送到github的代码位置，这里是指向Hexo生成的博客网站代码的文件夹
  local-dir: ./public
  # 推送的目标分支
  target-branch: master
  # 构建结果的通知对象
  email: your-email
  # 提交者
  name: commiter
  # 推送前的条件判断
  on:
    # 本次构建来自指定分支才进行推送，预防有多分支时造成的误推送（这里与前面都点重复）
    branch:
      - your-github-branch
# 设置邮件通知，构建成功或失败都进行通知
notifications:
  email:
    on_success: always
    on_failure: always
```

### 使用流程

经过上述的三个配置步骤，博客的自动部署就设置完成了，现在我们写一篇博客文章只需要：

1. 写博客文章
2. 推送文章内容到GitHub仓库

然后剩下的就交给Travis CI来完成了，它将会代替我们生成网站代码并部署到GitHub仓库上，我们只要耐心的等待一下，就能在网站上看到效果了。

# 结束语

网络上关于Hexo博客搭建的文章很多，如果有人能读到本篇文章，希望能给你带来一点帮助！

# 参考文档

[GitHub Pages帮助文档](https://help.github.com/categories/github-pages-basics/)  
[Hexo帮助文档](https://hexo.io/zh-cn/docs/setup.html)  
[Next主题](http://theme-next.iissnan.com/)   
[Travis CI帮助文档](https://docs.travis-ci.com/)
