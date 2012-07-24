---
layout: post
title: "Vagrantbox: Vagrant 虚拟映像集散地"
category: gem
---

[Vagrant][v] 是一个用 Ruby 写成的相当好用的虚拟机管理工具。利用
Vagrant，我们能够基于虚拟映像模板快速搭建开发环境，从而节省时间。
而 Vagrantbox 则为 Vagrant 提供热心人士构建好的虚拟映像，这些虚拟
映像包括 Arch Linux、Debian、Ubuntu、CentOS、RHEL、OpenBSD、Gentoo、
Slackware、openSUSE 等系统，我们不妨奉行拿来主义，直接取用即可。

**如何使用**

要使用这些虚拟映像，你可以从终端执行以下指令：

{% highlight bash %}
$ gem install vagrant
$ vagrant box add {title} {url}
$ vagrant init {title}
$ vagrant up
{% endhighlight %}

其中，你需要将 {title} 和 {url} 替换成实际的信息，比如：

{% highlight bash %}
$ vagrant box add precise64 http://files.vagrantup.com/precise64.box
{% endhighlight %}

Vagrantbox 可通过如下地址访问：

&mdash; [Vagrantbox](http://www.vagrantbox.es)

[v]: http://vagrantup.com
