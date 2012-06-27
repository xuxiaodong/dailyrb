---
layout: post
title: "在 Windows 上安装 Ruby 1.9.3"
category: howto
---

如果你打算在 Windows 上安装 Ruby 当前的最新版本
1.9.3，那么我们极力向你推荐 [RubyInstaller][i]。

RubyInstaller 支持 Windows XP、Windows Vista、以及 Windows 7，包括它们的 32
位和 64 位版本。你无需执行额外的安装过程，只用几次点击便可将 Ruby 安装到你的
Windows 系统中。

1. 转到 RubyInstaller
   主页，点击红色的“Download”按钮，在“RubyInstallers”下选择最新的 Ruby
   版本下载，写本文时为“Ruby 1.9.3-p194”。

   ![RubyInstaller](/images/rb-installer-1.png)

2. 点击运行下载的可执行文件，并选择“I accept the
   License”来接受许可协议，然后点击“Next”继续。

   ![RubyInstaller](/images/rb-installer-2.png)

3. 设置安装位置及选项。RubyInstaller 默认会将 Ruby 安装到 `C:\Ruby193`
   下，你可以视需要来加以更改。推荐勾选“Install Tcl/Tk support（安装 Tcl/Tk
   支持）”、“Add Ruby executables to your PATH（将 Ruby 可执行文件添加到 PATH
   中）”、“Associate .rb and .rbw files with this Ruby installation（关联 .rb
   及 .rbw 文件）”等选项。接着点击“Install”。

   ![RubyInstaller](/images/rb-installer-3.png)

4. 待安装完成后，点击“Finish”即可。

   ![RubyInstaller](/images/rb-installer-4.png)

至此，在 Windows 上安装 Ruby 完成。你可以打开“命令提示符”执行
`irb`，并输入一些测试代码来检验 Ruby 是否安装成功。

    > cd /d C:\Ruby193\bin
    > irb
    > puts "Hello RubyInstaller"

[i]: http://rubyinstaller.org
