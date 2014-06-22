---
layout: post
title: "我的Octopress博客"
date: 2014-06-15 06:35:12 -0400
comments: true
categories: 
---
很早之前就想自己搭一个博客了，但由于各种原因总是没有成功。在学长的推荐下，我发现了一款强大的博客系统——Octopress。它是一款基于Jekyll的内容管理工具，可以方便的搭建站点和发布静态页面。号称是hacker专属的一个博客系统(A blogging framework for hackers)。亲自体验后，我发现它可以让你**像写代码一样写博客**。

<!-- more -->

好了，废话少说，我就记录下我在*fedora20*环境下搭建Octopress的过程吧。

1. 安装Ruby
---

Octopress需要Ruby环境，RVM(Ruby Version Manager)负责安装和管理Ruby的环境。所以我们先在终端输入如下命令，来安装RVM：

    $ curl -L https://get.rvm.io | bash -s stable --ruby
然后根据提示运行

    $ source /etc/profile.d/rvm.sh
即可完成安装。这时运行

    $ ruby --version
发现显示

`ruby 2.1.2p95 (2014-05-08 revision 45877) [x86_64-linux]`

证明Ruby安装成功

  - 如果Ruby版本小于1.9.3，则需用RVM安装Ruby1.9.3



2. 安装Octopress
---

Ruby安装完成后，我们可以开始安装Octopress了。
首先，把Octopress项目clone到本地并进入Octopress目录。

    $ git clone git://github.com/imathis/octopress.git octopress
    $ cd octopress 
之后我们需要安装依赖

    $ gem install bundler
    $ bundle install 
最后安装Octopress默认主题

    $ rake install
- gem安装bundler的过程会比较慢，因为gem的默认安装源在国外。我们可以耐心等一等，如果想换源请自行google
- 如果使用`yum install ruby`命令安装ruby，在运行` gem install bundler `时会报错，这是由于yum安装Ruby并未将gem所需的所有依赖安装完整。因此请尽量使用RVM安装



3. 将博客部署到github
---

首先，我们需要在github上创建一个个人blog的仓库（相关操作请自行google）
创建好仓库后我们可以利用octopress的一个配置rake任务来自动配置

    $ rake  setup_github_pages
上面的命令会做一些事情，其中最主要的就是创建一个_deploy目录，目录用来存放部署到master分支的内容。期间会要求你输入仓库的url，根据提示，进行输入即可。
之后执行

    $ rake generate
    $ rake deploy 
这两行命令首先生成博客文件，并将生成的博客文件拷贝到_deploy/目录下，然后将这些内容添加到git中，并commit和push到仓库的master分支。至此，博客的部署基本完成，不过博客的source需要单独提交，执行如下命令就可以将source提交到仓库的source分支下。
    
    $ git add .
    $ git commit -m 'Initial source commit'
    $ git push origin source 
之后就是博客的配置工作了。Octopress有很多第三方主题，我使用的是一个极简风格的主题dotmin。博文支持现在很流行的markdown语法，这里推荐一款在线编辑markdown文件的编辑器[dillinger](http://dillinger.io)
![dillinger](http://img.my.csdn.net/uploads/201302/27/1361970185_6834.png)
它支持即时预览且预览速度较快，不过只能联网编辑，linux下的markdown编辑器我推荐[haroopad](http://pad.haroopress.com/user.html)
![haroopad](http://pad.haroopress.com/assets/images/intro/2.png)

它支持 Windows、Mac OS X 和 Linux。 主题样式丰富，语法标亮支持 54 种编程语言。其邮件导出功能可以将文档发送到 Tumblr 和 Evernote。这款产品由一位韩国人开发的，网站上的一部份帮助文档也是韩文。

4. 接下来会写什么
---

- ACM解题报告
- 学习记录
- 其他感兴趣的

总之，一个博客搭好只是一个开始，重要的是一直坚持写下去。加油。


