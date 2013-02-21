---
layout: post
title: "Matters need attention when updating Ruby 1.9"
description: ""
category: 
tags: [Ruby, Rails]
---
{% include JB/setup %}

最近手头的项目要迁移，原来应用部署的机房和存储都要下线了，需要把应用和原来nas上的老数据迁到别的地方去。趁着这次迁移的机会，
决定先动一些原来的代码，至少把Ruby和Rails版本升了，毕竟Ruby 2.0都快出了还在用REE，说出来有些丢人。

感觉迁移的话题可以总结的经验非常多，留到下次再说，这次先总结下1.8升1.9时踩到的一些坑吧。

### 编码问题

Ruby 1.9为了增加对多语言的支持，对字符编码的限定非常严格，具体细节可以看[这篇文章](http://about.ac/2012/06/understanding-m17n.html)

我遇到的问题是render一些模板的时候报了`incompatible character encodings: UTF-8 and ASCII-8BIT`这样的错误，解决的办法是这样：

* `application.rb`中加入`config.encoding = "utf-8"`这行

* 用`mysql2`这个gem，而不是'mysql'，否则从mysql里读出来的东西在渲染的时候也可能因为编码问题报错

* 在'environment.rb'中加这两行，记得加在`<App Name>::Application.initialize!`这一行前面
{% highlight ruby %}
Encoding.default_external = Encoding::UTF_8
Encoding.default_internal = Encoding::UTF_8
{% endhighlight %}

* 如果你的项目有用`memcached`的话，千万千万千万别忘了杀掉重启，这个坑浪费了我两个小时

### 语法问题

有些1.8的语法在1.9中也不再支持了，比如说switch
{% highlight ruby %}
#wrong
case xxx
  when 1: xx
  when 2: yy
  else: zz
end

#right
case xxx
  when 1 then xx
  when 2 then yy
  else zz
end
{% endhighlight %}

### 其他
1.9里面有一些基础的方法也变了，比如说下面String的这个方法，在1.8中取出的是asc码
{% highlight ruby %}
#1.8.7
str = "T1x.WyXghaXXb1upjX"
str[2] # => 120

#1.9
str[2] # => 'x'
str[2].ord # => 120
{% endhighlight %}