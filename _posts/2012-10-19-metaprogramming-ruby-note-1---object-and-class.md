---
layout: post
title: "Metaprogramming Ruby Note 1 - Object and Class"
description: ""
category: 
tags: [Ruby]
---
{% include JB/setup %}

最近開始看《Ruby元編程》這本書，學到了很多Ruby語言的有趣特性，受益匪淺，打算寫一系列的筆記記錄下來，這一篇先寫關於Ruby中對象和類的東東。

### Open Classes
從“打開類”窺探Ruby類定義
{% highlight ruby %}
class D
  def x
    'x'
  end
end

class D
  def y
    'y'
  end
end

obj = D.new
obj.x # => "x"
obj.y # => "y"
{% endhighlight %}
上面的代碼中，第一次提及class D時，並沒有D這個類，於是Ruby定義了D和x方法，第二次提及D時，class D已經存在，因此Ruby會直接打開這個類並定義y方法。

事實上Ruby的class關鍵字更像一個作用域操作符而非類型聲明語句，class關鍵字的核心任務是把你帶到類的上下文中，讓你可以在其中定義方法。

### Monkeypatch
使用打開類定義方法的時候，要注意避免和原本就存在的方法重名，否則會覆蓋原來的方法，出現問題。

### What's in an Object
{% highlight ruby %}
class MyClass
  def my_method
    @v = 1
  end
end

obj = MyClass.new
obj.class # => MyClass
obj.my_method
obj.instance_variables # => [:@v]
obj.methods.grep(/my/) # => [:my_method]
{% endhighlight %}
和Java這樣的靜態語言不同，Ruby中類和實例變量無關，上面的例子中，如果不曾調用`obj.my_method`方法，obj對象就不會生成任何實例變量，因此實例變量是保存在對象中。

共享同一個類的對象也共享同樣的方法，因此方法必須存放在類中，而不是在對象里。

### The Truth about Classes

類自身也是對象
{% highlight ruby %}
"hello".class # => String
String.class # => Class
String.superclass # => Object
Object.superclass # => BasicObject
BasicObject.superclass # => nil
{% endhighlight %}
類的類，叫做Class，所有的類又都繼承自Object，Object又繼承於BasicObject，BasicObject是Ruby對象體系中的根節點。
{% highlight ruby %}
Class.superclass # => Module
Module.superclass # => Object
{% endhighlight %}
類不過是一個增強的Module，只是增加了new()，allocate()，superclass()這三個方法

### Summary
什麼是對象？
對象就是一組實例變量+一個指向其類的引用。對象方法並不存在於對象本身，而是存在於對象的類中，即類的實例方法。

什麼是類？
類就是一個對象+一組實例方法+一個對其超類的引用。同時因爲Class類是Module類的子類，因此類也是一個模塊。
