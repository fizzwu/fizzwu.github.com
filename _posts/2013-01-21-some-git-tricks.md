---
layout: post
title: "Some git tricks"
description: ""
category: 
tags: [git]
---
{% include JB/setup %}

今天要把项目的git从原来我们自己的机器迁到公司搭的gitlab上去，push的时候遇到一个问题，因为公司的gitlab给我建账户的时候用的是公司邮箱，而我之前的提交
都是用的自己gmail邮箱，于是就推不上去了，好在git提供了一个命令可以修改过去所有commits的author和email，就是这个

`git filter-branch --commit-filter 'if [ "$GIT_AUTHOR_NAME" == "Fizz wu" ];
  then export GIT_AUTHOR_NAME="xxx"; export GIT_AUTHOR_EMAIL=xxx@xxx.com;
  fi; git commit-tree "$@"'`

  改完这些终于可以推了，上面有个比较老的分支，和我的分支发生了很多冲突，果断把它删掉，用
  
  `git push origin --delete <branchName>` 或者 `git push origin :<branchName>` 就可以了，后者是一个语法糖。