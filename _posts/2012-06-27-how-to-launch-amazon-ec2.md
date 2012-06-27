---
layout: post
title: "How to launch Amazon EC2"
description: ""
category: 
tags: [EC2, Linux]
---
{% include JB/setup %}

之前经常听人推荐[Amazon EC2](http://aws.amazon.com/ec2/)，因为第一年免费。。。于是我也折腾了一下方便以后当成VPS练手，简单记录一下。

注册[AWS](http://aws.amazon.com/about-aws/)需要一张国际信用卡，测试又刷掉1美元，认证的时候会打电话过来跟你确认PIN码，挺新鲜的，第一次见。

免费的EC2提供的配置和服务请移步[http://aws.amazon.com/free/](http://aws.amazon.com/free/)。

选择机器的时候一定要选择micro，不然就等着收账单吧。

security group是EC2的安全策略，至少把ssh和http端口开起来吧，0.0.0.0/0意思就是任何ip都可以使用这个服务。

后面下载key.pem，高级设置，tag，都默认就好，然后launch就可以了。

登上去就按照官方指导来（右键connect），key.pem可能需要chmod 400一下，之后遇到一个问题，官方指导说用

`ssh -i key.pem root@ec2-xx-xx.compute-1.amazonaws.com`

这样就可以了，
可是我一直被permission denied，后来搜到aws的论坛里有人说用户名root改成ubuntu就可以了，试了一下确实可以，进去以后也可以sudo，终于可以为所欲为啦。

