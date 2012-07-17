---
layout: post
title: "Pry 更新到 0.9.10 版"
category: news
---

![Pry](/images/pry.png)

功能至为强大的 IRB shell 替代品 [Pry][p] 近日更新到了新的 0.9.10
版本。该版本不仅升级了依赖 Gem，而且还添加了不少新特性。

* 将 slop 升级到版本 3、method\_source 升级到 0.8，同时从 gist 迁移到 [jist][j]
* 为 gist 命令添加 `--hist`、`-o`、`-k` 选项
* show-source/doc 支持 class-eval 中的方法定义及 C 中的 gem 方法定义
* 添加 `--disable-plugin` 及 `--select-plugin` 选项
* 允许使用 pry \<file\> 来让 pry input 执行文件
* 支持 `ri` 命令中的颜色

通过 Pry 0.9.10 的[更改日志][l]，你可以了解更多详情。

[p]: http://pryrepl.org
[j]: http://dailyrb.org/gem/2012/06/28/jist.html
[l]: http://github.com/pry/pry/blob/master/CHANGELOG
