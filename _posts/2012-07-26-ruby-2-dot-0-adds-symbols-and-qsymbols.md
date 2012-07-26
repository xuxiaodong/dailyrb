---
layout: post
title: "Ruby 2.0 新增表示符号数组的 %i 及 %I"
category: news
---

Ruby 2.0 开发分支的[一次提交][c]显示其中已经新增了用来创建符号数组的 `%i` 及
`%I` 表示法。该表示法与 `%w` 跟 `%W` 颇为相似。其中，`%i` 与 `%I`
的区别是，后者支持变量内插。这里的 `i` 意指 intern/interned。

要体验该特性，你可以通过 rbenv 安装 2.0.0-dev。rbenv
的用法可参考本站先前发表的文章《[rbenv + ruby-build: 轻松管理多个 Ruby 版本][r]》。

例如：

{% highlight ruby %}
%i[foo bar] # => [:foo, :bar]

x = 10
%I[foo b#{x}] # => [:foo, :b10]
{% endhighlight %}

[c]: https://github.com/ruby/ruby/commit/91bd6e711db3418baa287e936d4b0fac99927711
[r]: http://dailyrb.org/gem/2012/07/20/rbenv-and-ruby-build.html
