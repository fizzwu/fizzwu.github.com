---
layout: post
title: "Metaprogramming Ruby Note 2  - Finding Method"
description: ""
category: 
tags: [Ruby]
---
{% include JB/setup %}

寫這篇筆記之前先寫一個例子
{% highlight ruby %}
# a.rb
module Printable
  def print
    # ...
  end
end

module Document
  def print_to_screen
    format_for_screen
    print
  end
  
  def format_for_screen
    # ...
  end
  
  def print
    # ...
  end
end

class Book
  include Document
  include Printable
end

# b.rb
b = Book.new
b.print_to_screen
{% endhighlight %}
這段代碼是有問題的，本意在`print_to_screen`中調用的print方法是`Document::print`，事實上結果調用了`Printable::print`這個方法，why?


### Method Lookup
名詞解釋：接收者——調用方法所在的對象；祖先鍊——從一個類通過它的超類逐步移動到BasicObject的類路徑

* 在Ruby中調用一個對象的方法時，會先去接收者的類中查找，如果沒找到，就會從下往上在祖先鍊中查找這個方法。

* 如果在一個類中include了一個模塊，這個模塊就會被插入到祖先鍊，位置就在該類的上方。

* 當調用一個方法時，接收者就是self

根據這個原理回來看上面的例子
{% highlight ruby %}
Book.ancestors # => [Book, Printable, Document, Object, Kernel, BasicObject]
{% endhighlight %}
從這個祖先鍊上來看，Ruby會首先查找到Printable這個模塊，在裏面找到print方法以後，就直接執行了，而不會再去查找Document這個模塊，因此最簡單的方法就是交換include Document和include Printable這兩句話的順序，改變它們在祖先鍊中的順序就可以了。


