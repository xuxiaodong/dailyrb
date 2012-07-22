---
layout: post
title: "Phusion Passenger 3.0.14 发布"
category: news
---

[Phusion Passenger][p]，即 mod\_rails，是用于部署 Ruby Web 应用的 Apache 和 Nginx
模块。自 3.0 版本开始，Phusion Passenger 也能够不依赖于外部 Web
服务器而独立运行。今天，Phusion Passenger 的开发者发布了 3.0.14
版本，主要修正了早前版本中的一些缺陷。

* Apache：修复了长久的 mod\_rewrite 相关问题
* Nginx：首选 Nginx 版本更新到 1.2.2
* 修复 Ruby 1.9 编码问题

Phusion Passenger 支持 Linux、BSD、OS X 等平台，可从其官方网站[下载][d]。

[p]: http://www.modrails.com
[d]: http://www.modrails.com/install.html
