---
layout: post
title: "How to share files between linux servers with NFS"
description: ""
category: 
tags: [Linux]
---
{% include JB/setup %}

项目部署在多台机器的时候，有一些静态资源，cache和临时文件希望固定在同一台机器上，方便更新和保持数据一致性。原本我们的机器挂载了多台nas来作为公共的空间，
现在机房迁移以后没有nas了，只能拿几台机器里的其中一台做NFS，挂载到其他机器上。

NFS(Network File System)是允许将远程机器上的某个目录挂载到本地机器，使本地机器可以像操作本地目录一样操作远程目录的一种服务。
我个人理解就相当于一个本地的软链接链接到远程的目录去。

### 配置NFS服务端

编辑/etc/exports，在里面添加如下内容：

`/home/share 10.234.33.*（rw,sync,no_root_squash）`

意思是允许ip地址为10.234.33.* 的机器以读写权限访问/home/share目录

启动端口映射：

`/etc/rc.d/init.d/portmap start `

启动NFS服务：

`/etc/rc.d/init.d/nfs start `

### 配置客户端

在需要挂载NFS服务端共享目录的机器上输入如下命令(假设刚才服务端的ip地址是10.234.33.10)：

`mount -t nfs 10.234.33.10:/home/share /mnt`

就将本地的/mnt目录挂载到了刚才服务端的/home/share目录，这时候尝试在这个目录做一些操作，如果两边都能看到的话，说明就成功了。

上面的命令有一个问题，一旦客户端机器重启，这个挂载就会丢失，所以还有一种办法编辑`/etc/fstab`文件，添加如下这一行：

`10.234.33.10:/home/share  /mnt  nfs rw,actimeo=120,noacl,fsc 0 0`

挂载点/mnt目录必须存在，执行`mount /mnt`


