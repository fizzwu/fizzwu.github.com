---
layout: post
title: "Delegate in Rails"
description: ""
category: 
tags: [Rails]
---
{% include JB/setup %}

### Delegate

在项目中为了简化代码的复杂度，会经常的用到代理模式，所谓代理模式我的理解就是一个对象对外暴露的方法实质上是另一个相关对象的代理。

在Ruby中通常会使用`method_missing`这个特性来实现代理，Ruby自身也自带了一个Delegate module，但是看起来没有很多人用的样子。。

### ActiveSupport Delegate Module

在Rails项目里，有一个非常简洁的方式来实现代理模式，那就是[Module#delegate](http://apidock.com/rails/Module/delegate)，用这个东西就可以在你的class或者
module里面代理其他的方法或者相关的对象

举例说明，有如下两个model

{% highlight ruby %}
class Post
  belongs_to :user
end

class User
  belongs_to :posts
end
{% endhighlight %}

如果你想用`post.name`来返回文章作者的名字的话，就需要在`Post`model里加入如下定义

{% highlight ruby %}
class Post
  belongs_to :user

  def name
    user.try(:name)
  end
end
{% endhighlight %}

但是如果用`delegate`方法的话

{% highlight ruby %}
class Post
  belongs_to :user
  delegate :name, :to => :user, :allow_nil => true
end
{% endhighlight %}

事实上`delegate`并不局限于AcitveRecords，任何你定义的class和module都可以。代理方法指向的可以是变量，类变量或者是作为symbol的常量，其中`:to`参数是必须被
指定的，例如：

{% highlight ruby %}
class QueueManager
  
  attr_accessor :queue

  delegate :size, :clear, :to => :queue

  def initialize
    self.queue = []
  end
end

qm = QueueManager.new
qm.size  # => 0
qm.clear # => []
{% endhighlight %}

### Options

`:prefix`参数可以用来帮助你自定义代理方法的名称

{% highlight ruby %}
class Post
  belongs_to :user

  delegate :name, :to => :user, :prefix => true
  # post.user_name

  delegate :name, :to => :user, :prefix => "author"
  # post.author_name
end
{% endhighlight %}

最后，关于`delegate`方法的实现，可以直接看Rails源码，[这里](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/module/delegation.rb)


原文出处：[http://www.simonecarletti.com/blog/2009/12/inside-ruby-on-rails-delegate/](http://www.simonecarletti.com/blog/2009/12/inside-ruby-on-rails-delegate/)