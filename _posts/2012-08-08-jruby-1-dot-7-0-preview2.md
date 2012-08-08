---
layout: post
title: "JRuby 1.7.0 Preview2 放出"
category: news
---

![JRuby](/images/jruby.png)

JRuby 是 Ruby 程序语言的 Java 实现。昨日，JRuby 的开发团队放出了 1.7.0
的第二个 Preview 版本。该版本对 JRuby 的每个子系统都进行了改善，并且改
进了与 Ruby 1.9.3 的兼容性。

根据 [JRuby 1.7.0 Preview2 发布公告][j]，其主要更改情况如下：

* 默认运行模式为 1.9.3
* 修正许多 1.9.x 兼容性问题
* 针对 Java 7 禁用了 invokedynamic（Java 8 仍然是默认）
* 性能及并发能力增强
* 去掉了 Java 5 支持（要求 Java 6+）
* 解决了一些 IO 转码问题
* YAML 替代使用 Java locale 现在编码标量正确
* Kernel#exec 在所有平台上皆是原生 exec
* 改进及修正了 Java 整合及嵌入
* 修正了一些 Solaris 原生支持问题
* 解决了 122 个问题

JRuby 1.7.0 Preview2 可从其官方网站的[下载页面][d]获取。

[j]: http://jruby.org/2012/08/07/jruby-1-7-0-preview2.html
[d]: http://jruby.org/download
