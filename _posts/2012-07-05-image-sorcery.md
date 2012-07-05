---
layout: post
title: "Image Sorcery: 利用 ImageMagick 处理图像"
category: gem
---

[Image Sorcery][i] 是一个新的 Ruby [ImageMagick][m] 库，它着重于提供
ImageMagick 的 mogrify、convert 及 identify
这三个命令行工具的功能。因此，Image Sorcery 具有小巧实用、更省内存的鲜明优点。

**安装 Image Sorcery**

可通过 `gem` 指令来安装 Image Sorcery：

    $ gem install image_sorcery

**Image Sorcery 小试牛刀**

我们将通过 Image Sorcery 来编写一个创建缩略图的小程序，代码如下：

{% highlight ruby %}
require 'image_sorcery'

in_file = ARGV.shift
out_file = "thumb#{File.extname(in_file)}"

image = Sorcery.new(in_file)
image.convert(out_file, resize: '50%')
puts "#{out_file} created"
{% endhighlight %}

其中，第 1 行自然是引入 Image Sorcery
库，以便于我们后续使用；接着我们将从命令行传入的图片赋值给变量；最后我们通过
convert 方法来生成缩略图（原图的 50%）。

想对图像作更多处理？我建议你去读读 ImageMagick
的手册，自会获得一套魔幻的图像处理法术。

[i]: http://github.com/EricR/image_sorcery
[m]: http://www.imagemagick.org
