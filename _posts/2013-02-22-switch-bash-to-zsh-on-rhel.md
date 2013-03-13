---
layout: post
title: "Switch bash to zsh on RHEL"
description: ""
category: 
tags: [Linux, shell]
---
{% include JB/setup %}

公司的服务器上的shell默认是bash，自己本地用习惯[zsh](http://www.zsh.org/)以后，觉得有必要把两边体验统一起来，于是稍微折腾了一下。

### 安装zsh

服务器上自带了一个zsh，但是版本太老了，于是我下载源代码自己重新装了一个：

{% highlight bash %}
$ wget http://www.zsh.org/pub/old/zsh-4.3.17.tar.gz
$ tar xvzf zsh-4.3.17.tar.gz
$ cd zsh-4.3.17
$ ./configure
$ make
$ sudo make install
$ chsh -s /usr/local/bin/zsh
{% endhighlight %}

### 安装oh-my-zsh

[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)是一个用来管理你的zsh配置的框架，里面自带了很多插件和主题，
详细安装方式可以看前面链接里的readme，脚本安装方式失败的话推荐自己手动安装一下。

### 选择主题

其实我换到zsh的主要目的就是为了好看。。。所以必须要挑个喜欢的主题了，其实所有的主题都在`~/.oh-my-zsh/themes`目录下面，当然项目wiki
的页面里也有主题列表，还有截图，戳[这里](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)，直接在`~/.zshrc`里面设置下
`ZSH_THEME = "xxxx"`就可以了。

