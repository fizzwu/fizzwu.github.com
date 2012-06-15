---
layout: post
title: "How to make pygments work on jekyll"
description: ""
category: 
tags: [Jekyll]
---
{% include JB/setup %}

关于在blog里面贴代码，jekyll用的是[pygments](http://pygments.org/)，如果你和我一样用的是jekyll bootstrap的话，有的主题里面是没有syntaxhighlight的css的，需要自己生成一把

先自己装好pygments，jekyll config.yml里面设置`pygments: true`

cd到项目目录里面，执行`pygmentize -S default -f html > stylesheets/pygments.css`

记得在layout里引用这个css就好了

原文在[这里](http://www.stehem.net/2012/02/14/how-to-get-pygments-to-work-with-jekyll.html)

我用的是[github风格的代码高亮](http://pypi.python.org/pypi/pygments-style-github)

