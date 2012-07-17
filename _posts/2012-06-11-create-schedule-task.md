---
layout: post
title: "Rails创建定时任务"
description: ""
category: 
tags: [Rails, Gem, Linux ]
---
{% include JB/setup %}

最近项目里面需要针对每月的购买记录生成一份报表，因为数据有点多，而且需要做join好几张表的查询，如果做成action的话直接就跑死了，而且事实上并没有实时查询的需求，所以搞成后台task每月最后一天定期生成一份报表就好了。

### Task & Rexcel

最终生成的报表需要excel格式，我用了[Rexcel](http://www.xaop.com/blog/2008/12/23/rexcel-plugin/)这个插件，先写下一个task
{% highlight ruby %}
namespace :reports do
  desc "my report"
  task :my_report => :environment do
     arr = ActiveRecord::Base.connection.execute("#{your sql}").to_a
    workbook = Rexcel::Workbook.new
    worksheet = workbook.add_worksheet("reports")
    worksheet.add_line(["col1", "col2", "col3"])
    worksheet.add_lines(arr)
    Rails.logger.info("===================begin to write excel")
    File.open("#{RAILS_ROOT}/public/uploads/reports/#{month}月报表.xls", "w") do |f|
      f.write(workbook.build)
    end 
  end
end
{% endhighlight %}

### Crontab & whenever
cron是unix的一个系统工具，用来在后台执行一些定时任务，crontab就是定义这些定时任务的文件

#### crontab commands
* `crontab -e` Edit your crontab file, or create one if it doesn’t already exist.
* `crontab -l` Display your crontab file
* `crontab -r` Remove your crontab file

#### whenever
[whenever](https://github.com/javan/whenever)是一个用来生成可读性更好的crontab的gem

在`config/schedule.rb`中写完任务

{% highlight ruby %}
# 每个月1号凌晨4点执行任务
every '0 4 1 * *' do
  rake "reports:my_report"
end
{% endhighlight %}

在控制台直接执行whenver，得到原生的crontab line，放到部署应用的机器上


### 关于rake task传参

如果想在控制台手动执行task，也许需要传一些参数，写法是这样

{% highlight ruby %}
desc "模版应用次数统计报表 -- 手动执行，输入月份"
task :my_task_by_params, [:arg1, :arg2] => :environment do |t, args|
  puts args.arg1
  puts args.arg2
  # your code ...
end
{% endhighlight %}

控制台执行`RAILS_ENV=production rake my_task_by_params[1, 2]`

如果用的是zsh的话，这个rake命令可能会出问题，记得用noglob，[出处](http://www.scottw.com/zsh-rake-parameters)
