---
layout: post
title: "How to fix mysql error Library not loaded"
description: ""
category: 
tags: [Rails, MySQL]
---
{% include JB/setup %}

I got this error on mac rvm in my rails project

{% highlight ruby %}
/Users/fizz/.rvm/gems/ree-1.8.7-2012.02/gems/mysql2-0.2.18/lib/mysql2/mysql2.bundle: dlopen(/Users/fizz/.rvm/gems/ree-1.8.7-2012.02/gems/mysql2-0.2.18/lib/mysql2/mysql2.bundle, 9): Library not loaded: libmysqlclient.18.dylib (LoadError)
{% endhighlight %}

just use this command to solve it

`sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib/libmysqlclient.18.dylib`
