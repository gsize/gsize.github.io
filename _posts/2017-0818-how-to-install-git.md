---
layout: post
title: 安装Git记录
description: 
category: blog
---


在linux，有时没有root权限，有些系统软件包没安装全，没办法编译Git，

我就遇到系统里没有curl库

在自己的home目录里编译了curl，

然后通过国如下命令：
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

参考网站
https://yutuo.net/archives/2d5c6a3bcfaf69fe.html


[Gsize]:    http://gsize.github.io  "Gsize"
