---
layout: post
title: "Phusion Passenger 4.0 前瞻"
category: news
---

昨日，Phusion Passenger 的开发者在其 Blog 中披露了下一版——Phusion Passenger
4.0 的最新动向。简言之，Phusion Passenger 4.0 将包含两个版本，一为开源版，
一为企业版。Phusion Passenger 开源版和企业版将并行开发，其中，后者为商用收
费版本，将比前者提供更多的功能。

**企业版特性**

从目前公布的信息来看，Phusion Passenger 4.0 企业版将会提供以下两个相当不错
的新特性：

1. Rolling restarts（无缝重启）：当重启或部署新版本时，该特性不会临时冻结你
   的网站。利用无缝重启功能，Phusion Passenger 企业版将在后台重启应用进程，
   这样网站访问者不会感到响应变慢了。

2. Mass deployment（批量部署）：如果为 Phusion Passenger 企业版指定一个目录，
那么它将自动部署该目录中的所有 Web 应用。

**开源版特性**

在 Phusion Passenger 4.0 开源版中主要是内部改进，诸如切换到事件 I/O 核心、
重写了 ApplicationPool/进程执行子系统、减少了内存占用等等。

Phusion Passenger 的开发者同时在 Blog 中表示，他们将会很快发布 4.0 Beta 1，
让我们拭目以待吧。

&mdash; [Roadmap Preview 1 – Phusion Passenger 4.0 and Phusion Passenger
Enterprise](http://blog.phusion.nl/2012/07/24/roadmap-preview-1-phusion-passenger-4-0-and-phusion-passenger-enterprise/)
