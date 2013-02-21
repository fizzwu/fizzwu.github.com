---
layout: post
title: "How to fix RMagick no such file error"
description: ""
category: 
tags: [Deploy, Linux]
---
{% include JB/setup %}

前文说了我用源代码编译安装了ImageMagick，用bundle安装了rmagick以后，运行thin的时候提示这样一个错误：

`libMagickCore-Q16.so.7: cannot open shared object file: No such file or directory - /home/zhufei/.rvm/gems/ruby-1.9.2-p320@rails3020/gems/rmagick-2.7.2/lib/RMagick2.so`

Google一圈以后我用这个命令解决了它 `ldconfig /usr/local/lib`

关于ldconfig命令有一些解释：

ldconfig命令用于配置动态链接库运行时绑定。该命令会在所信任的目录（/lib和/usr/lib目录）、/etc/ld.so.conf文件所指定的目录和命令行参数所指定的目录中搜索最新的共享库，进而创建必要的链接并且进行缓存。默认的缓存文件为/etc/ld.so.cache，该文件中保存已排序的共享库名字的列表。
    
ldconfig通常在系统启动时运行，如果用户安装了一个新的动态链接库，那么便需要手动运行该命令。
