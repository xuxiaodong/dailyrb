---
layout: post
title: "rbenv + ruby-build: 轻松管理多个 Ruby 版本"
category: gem
---

在 Ruby 开发中，时常有在多个 Ruby
版本中测试代码的需求场景。为了使事情变得更加简单，我们可以选用 [rbenv][e] 这个
Ruby 版本管理工具。如果将它与 [ruby-build][b] 搭配使用，则可实现自动编译安装
Ruby、轻松管理多个 Ruby 版本的目的。

**rbenv 及 ruby-build 的安装**

rbenv 和 ruby-build 的源代码托管在 GitHub 上，只需通过 `git` 命令直接 clone
到本机即可完成安装。

我们先安装 rbenv：

{% highlight bash %}
$ cd
$ git clone git://github.com/sstephenson/rbenv.git .rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> .bash_profile
$ echo 'eval "$(rbenv init -)"' >> .bash_profile
{% endhighlight %}

Zsh 用户请将 .bash\_profile 替换成 .zshenv。

接着，我们安装 ruby-build：

{% highlight bash %}
$ mkdir .rbenv/plugins
$ cd .rbenv/plugins
$ git clone git://github.com/sstephenson/ruby-build.git
{% endhighlight %}

为使已安装的 rbenv 和 ruby-build 在我们的 shell
中即时生效，所以我们执行以下命令：

{% highlight bash %}
$ source ~/.bash_profile
{% endhighlight %}

同样的，Zsh 用户需换成 .zshenv。

**安装 Ruby**

现在，我们的 rbenv 工具已经准备就绪，可以用它来安装各种 Ruby
版本了。不过，在此之前，我们还得准备编译安装 Ruby
的各种工具（如编译器）及依赖。以 Ubuntu 为例，可通过下列命令安装：

{% highlight bash %}
$ sudo apt-get install build-essential autoconf automake bison libtool \
openssl libreadline6 libreadline6-dev curl zlib1g zlib1g-dev libssl-dev \
libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev libc6-dev ncurses-dev
{% endhighlight %}

假如我们想要安装 Ruby 的最新版本 1.9.3 p194，那么可以执行：

{% highlight bash %}
$ rbenv install 1.9.3-p194
{% endhighlight %}

_提示_：不带参数执行 `rbenv install` 可以获得可安装的 Ruby 版本列表。

rbenv 会先从 Ruby 官方网站下载源码包，然后开始自动化的编译安装过程。
根据机器的配置，该过程稍微有点耗时，你可以通过如下命令来监视：

{% highlight bash %}
$ tailf /tmp/ruby-build.*.log
{% endhighlight %}

你可以根据实际需要安装多个 Ruby 版本。在此，我们也将安装 Ruby 1.8.7 p370：

{% highlight bash %}
$ rbenv install 1.8.7-p370
{% endhighlight %}

在 Ruby 安装完成之后，我们需要执行下面的命令，以便 rbenv 重建 shim 可执行文件：

{% highlight bash %}
$ rbenv rehash
{% endhighlight %}

**管理 Ruby 版本**

rbenv 支持以下三种 Ruby 版本的环境管理：

* global：设置全局的 Ruby 版本，换句话说，所有的 shell 都将使用该 Ruby 版本。
* local：为本地的一个特定项目设置 Ruby 版本，注意这将覆盖全局设置。
* shell：针对 shell 设置 Ruby 版本，该设置将覆盖 global 和 local 设置。

要将我们先前安装的 Ruby 1.9.3 p194 设置为全局性版本，可以执行：

{% highlight bash %}
$ rbenv global 1.9.3-p194
{% endhighlight %}

设置为局部性版本和 shell 级版本，可分别执行：

{% highlight bash %}
$ rbenv local 1.9.3-p194
$ rbenv shell 1.9.3-p194
{% endhighlight %}

最后，通过 `rbenv versions` 能够查看已经安装的 Ruby 版本，其中，带 \*
的项目为当前正在使用的 Ruby 版本。

[e]: https://github.com/sstephenson/rbenv
[b]: https://github.com/sstephenson/ruby-build
