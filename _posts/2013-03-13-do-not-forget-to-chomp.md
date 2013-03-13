---
layout: post
title: "Do not forget to chomp!!"
description: ""
category: 
tags: [Ruby]
---
{% include JB/setup %}

今天遇到一个巨大的坑，浪费了半个小时

事情是这样的，之前迁移数据，迁移完以后cdn的同学跟我说有部分数据迁移不完整，于是我写了一个脚本来比对，将没有迁移成功的文件名和路径记录在一个日志里。
然后再逐行扫描这个日志，将这些文件再重新迁移。迁移的脚本我是这么写的:

{% highlight ruby %}
File.open("#{Rails.root}/log/unchecked.log", "r").each_line do |line|
  // xxx
  upload(File.open(line).read)
end
{% endhighlight %}

结果一直提示我说这个文件找不到，但是这个文件明显是存在的，在各种调试无果最后问了google以后才知道，原来这个写法Ruby会在每一行后面自动加上`\n`换行符，
所以必须要先

`line.chomp`

否则自然就找不到这个文件，以后读文件的时候千万要注意。

