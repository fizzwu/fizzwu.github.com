---
layout: post
title: "Rails environment with Nginx, HAProxy and Thin"
description: ""
category: 
tags: [Linux, Deploy, Nginx]
---
{% include JB/setup %}

继续迁移的话题，上面给了我们5台机器（banner1 ~ banner5)，装的是Red Hat Linux，准备拿其中两台做[Nginx](http://wiki.nginx.org/Main)反向代理，用[HAProxy](http://haproxy.1wt.eu/)做负载均衡和监控，
后面的Rails web server用的是[thin](http://code.macournoyer.com/thin/)，而且希望有一些特殊的action（处理图片，生成文件的请求）能指向到banner4和banner5跑的thin上。

计划就是这样，下面记录一下我的操作和配置。

### Nginx

下载nginx源码，configure, make, make install不提，有一个坑要提一下，如果你的nginx配置里用了`stub_status`，编译nginx的时候要加上参数

`./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module`


### HAProxy

安装： `sudo yum install haproxy`

//TODO


### Thin

//TODO

### Memcached

从源码编译的文档在[这里](https://code.google.com/p/memcached/wiki/NewInstallFromSource)

{% highlight bash %}
$ yum install libevent-devel
$ wget http://memcached.org/latest
$ tar -zxvf memcached-1.x.x.tar.gz
$ cd memcached-1.x.x
$ ./configure --prefix=/usr/local/memcached
$ make
$ sudo make install
{% endhighlight %}

usage: `/usr/local/memcached/bin/memcached -d -m 1024 -l 0.0.0.0 -p 11211 -u root`