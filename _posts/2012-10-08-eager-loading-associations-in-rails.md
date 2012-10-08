---
layout: post
title: "Eager loading associations in Rails"
description: ""
category: 
tags: [Rails]
---
{% include JB/setup %}

在Rails中，渲染一個列表的時候往往需要展現出多個相互關聯的Model，如果取的時候不注意的話很容易產生所謂的N+1問題，看如下代碼

{% highlight ruby %}
clients = Client.limit(10)
 
clients.each do |client|
  puts client.address.postcode
end
{% endhighlight %}

這段代碼乍看之下沒什麼問題，但是看服務器日誌的時候，會發現一共做了11次查詢，1次find clients的查詢 + 10次取client.address的查詢，這無疑會嚴重影響性能，因此controller里要做預加載，一次查詢都查出來，代碼修改如下

{% highlight ruby %}
clients = Client.includes(:address).limit(10)
 
clients.each do |client|
  puts client.address.postcode
end
{% endhighlight %}

用上include這個參數，這個時候看服務器日誌，就只有兩條sql而不是一大坨了

{% highlight sql %}
SELECT * FROM clients LIMIT 10
SELECT addresses.* FROM addresses
  WHERE (addresses.client_id IN (1,2,3,4,5,6,7,8,9,10))
{% endhighlight %}

會想到記這個問題是因爲今天遇到了這個場景，我有n組人羣定向的集合DirectionGroup，每個集合下面有若干定向信息Direction，每個Direction又一一對應一塊廣告adboard，我需要展現這些所有這些集合的列表，同時要展現每塊adboard的縮略圖，那我就應當在一開始都取出來，而不是在渲染的時候一次次的去查數據庫，大概是這樣子

{% highlight ruby %}
# Models...
class DirectionGroup
  has_many :directions
end

class Direction
  belongs_to :direction_group
  belongs_to :ad_board
end

# DirectionGroupController中的index方法
def index
  @direction_groups = current_person.direction_groups.all(:include => [{:directions => :ad_board}])
end

{% endhighlight %}

上面的index方法中，用了嵌套的include，再看服務器日誌，這樣三句sql就都查出來了，我可以直接在render的partial中去展現這些東西
{% highlight sql %}
SELECT `direction_groups`.* FROM `direction_groups` WHERE (`direction_groups`.person_id = 2191885)
SELECT `directions`.* FROM `directions` WHERE (`directions`.direction_group_id IN (1,2,3,4,5,6,7,14,10)) ORDER BY id desc
SELECT `ad_boards`.* FROM `ad_boards` WHERE (`ad_boards`.`id` IN (6544321,6544322,6544324))
{% endhighlight %}

這些用法我也是看了[這裏](http://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations)才知道，Rails Guides真的要好好讀。。

關於include和join的區別，先挖個坑，下篇再總結




