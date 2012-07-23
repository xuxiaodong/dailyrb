---
layout: post
title: "Git URL 缩短术"
category: howto
---

每当我从 GitHub 或别的地方克隆 Git 仓库时，总是会先复制该仓库的
URL，然后再粘贴到终端。毕竟，有些 URL 太长。其实，Git 允许我们将仓库的 URL
进行缩短，这样我们就不用再敲那么多字了。

以 GitHub 的仓库地址为例，如果我们要定义一个只读缩短
URL，那么可将下列内容添加到 ~/.gitconfig 文件中：

{% highlight bash %}
[url "git://github.com/"]
  # Read-only
  insteadOf = gh:
{% endhighlight %}

现在假设我想从 GitHub 克隆 Rails 仓库，只需执行以下指令即可：

{% highlight bash %}
$ git clone gh:rails/rails
{% endhighlight %}

另外，我们也可以定义一个具有读写访问权限的 URL，以 `wgh:` 抬头：

{% highlight bash %}
[url "git@github.com:"]
  # With write access
  insteadOf = wgh:
{% endhighlight %}

Git URL 缩短的关键是使用了 insteadOf 变量，查看 Git 帮助（`git help
gitconfig`），发现其中还有一个
`pushInsteadOf`，用于推送时，有兴趣的同学不妨自行尝试。

{ via <http://robots.thoughtbot.com> }
