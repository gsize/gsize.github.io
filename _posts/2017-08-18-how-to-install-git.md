---
layout: post
title: 安装、使用Git记录
description: 
category: blog
---


在linux，有时没有root权限，有些系统软件包没安装全，没办法编译Git，

我就遇到系统里没有curl库

在自己的home目录里编译了curl，

然后通过如下命令：
make CURLDIR=/you/instal/the/curl-dir NO_R_TO_GCC_LINKER=1 prefix=/home/petgroup/tools/test/fmi/tofpet/GaoSize/opt/git-install install

注意：CURLDIR是安装curl的目录，

NO_R_TO_GCC_LINKER这个是编译Git时提示如下出错时的解决方法
cc: error: unrecognized command line option ‘-R’

最后的install是编译后安装指示，如果不加，只是编译，



在克隆某个库时，git clone https://github.com/root-project/root.git root-git
出现如下出错提示时，
fatal: unable to access 'https://github.com/root-project/root.git/': SSL certificate problem: unable to get local issuer certificate

用下面命令修改http.sslVerify变量 
git config --global http.sslVerify false


删除远程分支
git push origin --delete remote_branch_name

强制推送分支到远程库
git push --force remote_repo_name   branch_name

列出所有本地分支的跟踪情况
git branch -vv

本地库标签tag同步到远程库
git push github-gsize --tags

当git本地库很大时，影响库的操作效率，用如下命令优化：
git gc

## 变基（rebase） ##
从远程库push或者是从一个分支分出来一个新的分支后，并做了一次以上的commit提交之后，其上游（upstream）做了变动，
这时可通过rebase进行工作合并，可以使commit的历史看起来是单线的，不会出像各种merger信息。注意了，不要在体检远程库之后，又在本地变基后又提交到库里，

当git在服务器中的安葬路径不再默认的bash执行PATH变量之中，这时远程push、clone或fetch等
操作时，会提示：git-uplocat-pack \ git-receive-pack等command not found

只需设置相应的远程连接参数，如： git config remote.origin.uploadpack git-upload-pack在服务器中的绝对路径
或是在服务器中的/bin/或/usr/bin中创建到git-uplocat-pack \ git-receive-pack的软连接

参考网站
https://yutuo.net/archives/2d5c6a3bcfaf69fe.html


[Gsize]:    http://gsize.github.io  "Gsize"
