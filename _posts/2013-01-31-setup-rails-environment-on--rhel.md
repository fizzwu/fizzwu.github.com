---
layout: post
title: "Setup Rails Environment on  RHEL"
description: ""
category: 
tags: [Linux, Deploy]
---
{% include JB/setup %}

这次应用搬家公司给的机器是RHEL，从来没在这上面装过Rails环境，和Mac，Ubuntu有一些不同，遇到的坑记一下。

### RVM

rvm有很多package，openssl, libyaml这种，如果没装这些东西直接装Ruby的话，后面要装别的依赖这些包的东西的话，要重新装Ruby，麻烦的很，所以先装掉
{% highlight bash %}
$ rvm get head
$ rvm pkg remove
$ rvm requirements run
$ rvm pkg install openssl libyaml
{% endhighlight %}

### Errors:

`gem install libxml-ruby` 出错，需要安装libxml2-devel, `sudo yum install libxml2-devel`

`gem install mysql2` 出错，安装mysql-devel, `sudo yum install mysql-devel`

### Installation

#### Install rmagick

先装ImageMagick，公司的RHEL版本太老，yum源也是老的不行，直接`yum install ImageMagick-devel`的版本太老了，只能直接从源代码编译
安装，原文在(这里)[http://rmagick.rubyforge.org/install2-linux.html]

ImageMagick依赖一些库，可以先用yum装掉

`sudo yum install libpng-devel`

`sudo yum install libjpeg-devel`

然后下载源码包，解压，一路configure, make, make install下去

{% highlight bash %}
$ wget http://xxx/ImageMagick.tar.gz 
$ tar xvzf ImageMagick.tar.gz
$ cd ImageMagick-x.y.z
$ ./configure
$ make
$ make install
{% endhighlight %}

到这里ImageMagick就装好了，然后我装Rmagick的时候提示Path有问题，google了一下这么解决

将`export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig"`这行加到`.bashrc`中，然后敲这两行命令：
{% highlight bash %}
$ ln -s /usr/local/include/ImageMagick/wand /usr/local/include/wand
$ ln -s /usr/local/include/ImageMagick/magick /usr/local/include/magick
{% endhighlight %}

后面遇到问题再补充...
