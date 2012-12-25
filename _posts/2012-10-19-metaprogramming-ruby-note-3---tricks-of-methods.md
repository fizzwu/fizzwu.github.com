---
layout: post
title: "Metaprogramming Ruby Note 3 - Tricks of Methods"
description: ""
category: 
tags: [Ruby]
---
{% include JB/setup %}

寫這篇筆記之前需要先簡單提一下Ruby這樣的動態語言和Java這樣的靜態語言的區別。像Java在每一次執行方法調用的時候，編輯器都會做靜態類型檢查，一旦調用了一個不存在的方法，編譯器就會直接報錯，這種特性的優點是使代碼運行前編譯器就可以發現其中的錯誤，但爲此你必須被迫寫許多重複的代碼，即樣板方法（比如Java中大量的get，set)，而Ruby只有在代碼真正運行的時候才會做檢查，這樣你就可以在最後一刻來決定調用哪個方法，甚至定義方法，事實上這也就是Ruby元編程的定義，在代碼運行時的編程。

### A Refactoring Example
關於Ruby方法的有趣特性，這裏通過一個重構的實際需求來展現。

默認的需求是需要生成一個關於計算機部件信息和價格的報表，它有如下類定義：

{% highlight ruby %}
# data_source.rb 數據源類
class DS
  def initialize # connect to data source...
  def get_mouse_info(workstation_id) #...
  def get_mouse_price(workstation_id) #...
  def get_keyboard_info(workstation_id) #...
  def get_keyboard_price(workstation_id) #...
  def get_cpu_info(workstation_id) #...
  def get_cpu_price(workstation_id) #...
  #...
end

# irb
ds = DS.new
ds.get_cpu_info(42) # => 2.16 GHz
ds.get_cpu_price(42) # => 150


{% endhighlight %}

