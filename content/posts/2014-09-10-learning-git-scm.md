---
title: "Learn Git SCM"
date: 2014-09-10T00:00:00+08:00
draft: false
aliases: [/posts/learn-Git-SCM.html]
description: Git 是一个分布式的版本控制系统，SVN 的操作日志等都是存储在服务端，用户如果要查看修改记录等，每次都是需要联网的，我们公司目前就是使用的 SVN，由于经常上 GitHub 所以对 Git 有所好感，它没有 SVN 服务器这种单点故障，在对比差异和查看以前修改的版本时无需联网，对于这么牛的东西还是得好好学习一下。
tags: ["Git"]
---

Git 是一个分布式的版本控制系统，SVN 的操作日志等都是存储在服务端，用户如果要查看修改记录等，每次都是需要联网的，我们公司目前就是使用的 SVN，由于经常上 GitHub 所以对 Git 有所好感，它没有 SVN 服务器这种单点故障，在对比差异和查看以前修改的版本时无需联网，对于这么牛的东西还是得好好学习一下。

## 第一章 起步
版本控制是很早以前就提出来了，最开始的时候是本地版本控制系统，最流行的叫 rcs，后来进化到集中化版本控制系统，再才过渡到分布式版本控制系统。

Git 诞生于 2005 年，因为 BitKeeper 与 Linux 内核开源社区的合作终止。承受后 Linus Tovalds 不得不开发属于自己的版本控制系统，他们对本次系统的目标有：
* 速度
* 简单的设计
* 对非线性开发模式的强力支持（允许上千个并行开发的分支）
* 完全分布式
* 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

Git 直接记录快照，而不是差异比较，SVN 会把每个文或几个文件在这个版本做了什么修改而记录下来，Git 则不然，它在每一次提交都是整个项目的快照。Git 中绝大多数都可以在本地完成，因为在本地磁盘上就保存了项目的历史版本信息。

#### 文件的三种状态
* 已提交 committed 表示文件已经被安全地保存在本地数据库中了
* 已修改 modified  表示修改了某个文件，但还没有提交保存
* 已暂存 staged    表示把已修改的文件放在下次提交时要保存的清单中。

#### 第一次运行 Git 的配置
使用Git之前需要配置下自己的 Git 工作环境。配置工作只需一次，以后升级时还会沿用现在的配置。

~~~bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --global core.editor vim
$ git config --global merge.tool vimdiff
$ git config --list
user.name=Scott Chacon
user.email=schacon@gmail.com
color.status=auto
...
$ git config user.name
Scott Chacon
~~~

## 第二章 基础

#### 暂存区的理解很重要
如果仅仅是直接修改了文件，那么你直接提交是没有任何作用的，需要把这些文件先放到暂存区来，然后再提交。那如何加入到暂存区呢，使用 `git add file` 就可以把它们加入了。

~~~bash
$ git diff 查看未暂停的文件与已暂存起来的改动。
$ git diff --staged 查看已暂存的文件与上次提交的变化
~~~

有时候我们会嫌这个过程比较繁琐，因为我们知道自己就是要修改这些文件并且已经OK了，那么可以使用 git commit -a 来提交它会跳过暂存区，把已经跟踪的文件提交上，<font color="#cc3300">注意：如果未跟踪的文件则不会提交</font>


#### git 进行文件批量操作
比如我们要添加当前目录下以及所有子目录以 .txt 结尾的文件到 Git 中 需要使用 `git add \*.txt`，为什么要加 `\`，是因为如果不加的话 Shell 帮忙匹配，并把匹配的结果传给 Git 告诉它要添加哪些文件，而 Shell 这样匹配的时候是不进行递归操作的，也就是子目录里面的 .txt 结尾的文件不会被匹配上，而 Git 对这个的处理做的比较好。所以如果用 `*` 的话最后使用 `\*`。

#### git log
    记几个比较常用的
	-p 显示每次提交的内容差异
	-N 代表仅显示最近的N 次更新
	-UN 代表显示有修改的上下文N条

#### 自动补全。。可以补全 Git 各种命令和选项
