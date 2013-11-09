---
layout: post
title: 开博第一篇
category: blog
description: 整了将近一周，终于搞明白怎么在github-blog上面用jekyll建博客
---

##为什么要建博客？##

一周前，在google中收索vim的配置时，看到[beiyuu]的一篇博客《[我为什么写博客？]》，写了他为啥要建博客，好奇心的驱使，关注了一下，刚好看到[github]也能写博客！很有诱惑力，就跟着其中一篇[博客]建起了博客，想建博客的可以参考一下。

在[github]上用的博客引擎是jekyll，这个软件的windows版本可以在[portablejekyll]这个站点下载，是一个免安装版。起先对jekyll的构架还不是很了解，我选择用markdown语言作为博客的标志语言，而且[beiyuu]博客选择用rdiscount作为解析器，而[portablejekyll]刚好没预装这东西，结果在安装rdiscount时老是卡在一个错误上面，耽误了几个晚上的功夫，两天前突然发现解析markdown不只有这么一个解析器，[portablejekyll]也预装了redcarpet和kramdown这两个解析器，只要在_config.yml中更改markdown的解析器就能解决了，`markdown: rdiscount`改为`markdown: kramdown`或者其他的，遇到的问题就迎刃而解了，但由于我用的是中文版windows系统，得改变cmd的显示字符码为utf-8，不然会遇到提示关于`GBK`的错误，所以在执行命令前先执行一下`chcp 65001`，即可解决问题。

*那为什么要建这个博客？*其实，作为一个开源社区的精神的崇拜者，看到关于Linux、Git之类相关的东西都会激起本人的好奇心。熟话说，好奇害死猫！为了建起这个博客，连续费了好几个晚上，下班一吃完饭就搞到想睡觉为止。但本人有个毛病，就是一旦搞定一个东西之后，就对它没有激情继续维护、发展下去。因为写作不是我的强项，而且思绪容易混乱，剧情跳跃过大，毫无逻辑性。想一想，写博客不正是养成长篇写作具逻辑性的一种非常手段，不像微博，简短几句话就挂出去了。那就努力坚持一下了！

孩子，好好写啊！哥留名网络就靠你了！

[github]: github.com "GitHub"
[beiyuu]: beiyuu.com "BeiYuu"
[我为什么写博客？]:  http://beiyuu.com/why-blog/
[博客]:  http://beiyuu.com/github-pages/
[portablejekyll]:  http://www.madhur.co.in/blog/2013/07/20/buildportablejekyll.html


##要干点什么##

既然要写博客，那就得给站点给个行动纲领之类的吧，不然毫无目的性的写，这个站点就像是清晨的一群村妇在溪边洗衣服时的闲扯了。

那就分三种类型的博文吧：
   --  身边事
   --  工作、技术贴
   --  心事

##谈谈逻辑性##

别扯别的，来点实在事！逻辑思维

`未完成！`


