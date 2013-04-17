---
layout: post
title: "Nginx Log Rotation With Cronolog"
description: ""
category: 
tags: [Linux, Nginx]
---
{% include JB/setup %}

Nginx的日志默认只打印到一个文件中，为了方便分析和定期清理日志，需要对这个日志文件按时间进行分割，我用的是[cronolog](http://cronolog.org/)这个工具。

简单记录一下我的配置过程，

配置nginx日志

`access_log /home/logs/xx/access.log.pipe main;`

创建日志管道

`mkfifo /home/logs/xx/access.log.pipe`

配置cronolog，按天分割日志

`nohup cat /home/logs/xx/access.log.pipe | cronolog /home/logs/xx/xx_pv_log_%Y%m%d > /dev/null 2>&1 &`

启动nginx