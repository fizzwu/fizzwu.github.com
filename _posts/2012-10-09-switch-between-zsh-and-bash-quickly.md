---
layout: post
title: "Switch between zsh and bash quickly"
description: ""
category: 
tags: [terminal]
---
{% include JB/setup %}

上週公司的mac掛了，國慶供應商拿去換了個硬盤，今天送過來了，坑爹的給我裝了個雪豹，只好自己再刻個10.8的啓動重裝。

裝完系統裝環境，最近重裝系統次數有點多，折騰環境越來越熟練了，可是今天遇到個詭異的問題，裝完rvm裝ruby的時候死活裝不上去，日誌也說得不明不白，
google了一圈在github的一條issues裏面看到有個傢伙也和我一樣，他說是oh-my-zsh的問題，切到bash下面去就可以裝了，也不知道爲啥，
我估計多半還是環境變量的問題，動手試了一下，確實可以裝了，詭異啊。

其實這篇文章想說的只有一句話，學到了一招簡單的切換bash和zsh的辦法，只要`exec bash`和`exec zsh`就可以在當前session里自由切換了，
而且新開一個terminal標籤也不會影響。