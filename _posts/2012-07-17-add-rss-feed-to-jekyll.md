---
layout: post
title: "Add RSS feed to jekyll"
description: ""
category: 
tags: [Jekyll]
---
{% include JB/setup %}

搞了半天blog连个RSS订阅都没有，搞毛啊。。。

其实只要搞一个xml模版就可以了，我把它存成`/feed/index.xml`， 模版可以参考我的[index.xml](https://github.com/fizzwu/fizzwu.github.com/blob/master/feed/index.xml)

然后把这个地址拿到feedsky去烧一下就可以在google reader里面订阅了。