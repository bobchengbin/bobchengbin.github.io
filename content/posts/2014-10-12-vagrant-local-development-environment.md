---
title: "Local development environment -- Vagrant"
date: 2014-10-12T00:00:00+08:00
draft: false
aliases: [/posts/local-development-environment----Vagrant.html]
description: Vagrant 对于本地开发来说真的是非常的棒，一般公司的开发人员都不止一个人，所以要每个开发人员都自己去搭建本地环境，会耗费非常多的时间，甚至还有可能会搭不起来的，就算搭起来了也不够统一，不能够和线上的保持一致。
tags: ["Vagrant"]
---

Vagrant 对于本地开发来说真的是非常的棒，一般公司的开发人员都不止一个人，所以要每个开发人员都自己去搭建本地环境，会耗费非常多的时间，甚至还有可能会搭不起来的，就算搭起来了也不够统一，不能够和线上的保持一致。

## Vagrant 是什么呢？
它是基于 Virtual box(后面简称 VBox 了)而运行的，其实它就是在使用命令来控制 Vbox，它可以让你非常方便的去配置虚拟机，因为所有的配置都是在配置文件里面。

## 各种坑

### 一、静态文件
安装好 Apache 和 Nginx 首先来测试静态文件(我使用的是 .html 结尾的)，这些文件是从主机共享，然后虚拟机里面挂载的，使用的是Vagrant的：`config.vm.synced_folder "/Users/bob/Documents/code/", "/var/www/"`，然后我在本地修改这些文件，发现在浏览器上刷新都不变，总是不相信浏览器，于是用命令行的 cURL 去测试，结果还是一样的，后来终于在网上搜到了[帖子：《使用 Vagrant 打造跨平台开发环境》](http://segmentfault.com/blog/fenbox/1190000000264347)，它这里说到了， 不然的话它就不变。<pre><code class="hljs">\# Apache 配置添加: EnableSendfile off
\# Nginx 配置添加:sendfile off;
</code></pre>

### 二、磁盘满了
这个其实不能算 Vagrant 的坑，而是自己不细心，但是放到这也是在提醒自己和读者不要忘记这件事。
<img src="/assets/blog/D2E51D4A-72D3-4ABF-A26B-CFEA5773ED5B.png" />

### 三、文件权限
在 Windows 上的时候进入虚拟机后发现，共享的文件全部都是 777 的权限，而且还改不过来，这是怎么回事呢。这是 PHP MyAdmin 报出的错误：

~~~
Wrong permissions on configuration file, should not be world writable!
( ! ) Notice: Array to string conversion in
/var/www/phpmyadmin/libraries/Error.class.php

通过一番 Google 之后终于找到了问题的解决：http://serverfault.com/questions/398414/vagrant-set-default-share-permissions， 可以在配置文件里面填写：

config.vm.synced_folder "/Users/bob/Documents/code/", "/var/www/", :mount_options => ["dmode=755","fmode=644"]
~~~

### 四、运行 vagrant up 之后机器起不来
这个问题比较有意思，在我们运行 `vagrant up` 这个命令的时候它会让 Vbox 去启动这个虚拟机，然后等虚拟机启动完成之后就修改相应的配置，但是却遇到了一个问题，Vbox 撒娇了——就不起来，于是只能打开 Vbox 手动去点启动，发现也启不来，却得到了一个报错：`Error VT-x`，最终解决也是在 stackoverflow 里面找到的，默认可能 cpus 等于的是 2，但是我们有的机器配置偏低就起不来了，所以把它改成1之后再启动就好了。

~~~
config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpus", "1"]
end
~~~
