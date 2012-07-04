---
layout: post
title: "在 Ruby 中执行 Shell 命令的 6 种方法"
category: howto
---

我们时常会与操作系统交互或在 Ruby 中执行 Shell 命令。Ruby
为我们提供了完成该任务的诸多方法。

1. Exec

   `Kernel#exec` 通过执行给定的命令来替换当前进程，例如：

       $ irb
       >> exec 'echo "hello $HOSTNAME"'
       hello codefun
       $

   注意 `exec` 利用 `echo` 命令替换了 `irb` 进程，然后退出。因为 Ruby
   实际上结束了该方法，所以只能有限使用。该方法的缺点是，你无法从 Ruby
   脚本中知道命令是执行成功还是失败。

2. System

   `system` 命令与 `exec` 操作相似，但它是在 subshell
   中执行，而不是替换当前进程。与 `exec` 相比，`system`
   给我们更多的信息。如果命令执行成功，它返回 true；否则返回 false。 

       $irb
       >> system 'echo "hello $HOSTNAME"' 
       hello codefun
       => true
       >> system 'false'
       => false
       >> puts $?
       256
       => nil
       >>

   `system` 将进程的退出状态设置到全局变量 `$?`。注意 `false`
   命令的退出状态，总是非 0
   值。检查退出码让我们既可以抛出（raise）异常，又能够重试（retry）命令。 

   新手注意：Unix 命令执行成功退出码为 0，否则为非 0。

   如果我们想知道“命令是否执行成功呢？”，使用 `system`
   就很好。然而，我们时常想要捕获命令的输出，并在程序中使用。

3. Backticks (`)

   backticks（也叫“backquotes”）在 subshell
   中执行命令，并从该命令返回标准输出。

       $ irb
       >> today = `date`
       => "Wed Jul  4 22:03:15 CST 2012\n"
       >> $?
       => #<Process::Status: pid 6169 exit 0>
       >> $?.to_i
       => 0

   这也许是在 subshell 中执行命令最广为人知的方法。如你所见，它返回了命令
   的输出，然后我们可以像其他字符串一样使用它。 

   注意 `$?` 并非是返回状态的整数，而实际是 Process::Status
   对象。我们不仅有退出状态，而且有进程 ID。Process::Status#to_i
   给我们作为整数的退出状态（#to_s 给我们作为客串的退出状态）。

   使用 backticks 我们只能获得命令的标准输出（stdout），而不能获得其标准错
   误（stderr）。在下面的例子中，我们执行 Perl 脚本来输出字符串到标准错误。

       $ irb
       >> warning = `perl -e "warn 'dust in the wind'"`
       dust in the wind at -e line 1.
       => ""
       >> puts warning
    
       => nil

   注意变量 warning 没有被设置。当我们在 Perl 中执行 warn
   时，产生的标准错误输出并没有被 backticks 捕获。 

4. IO#popen

   `IO#popen` 是在子进程中执行命令的另一种方法。`popen`
   给你更多的控制，子进程的标准输入和标准输出都会连接到 IO 对象。

       $ irb
       >> IO.popen("date") { |f| puts f.gets }
       Wed Jul  4 22:02:31 CST 2012
       => nil

   虽然 `IO#popen` 不错，但当我需要间隔层次时更典型的使用
   `Open3#popen3`。

5. Open3#popen3

   Ruby 标准库包含 Open3 类。它易用，并能返回标准输入、标准输出、以及标准
   错误。在本例中，让我们使用交互命令 `dc`。dc 是从标准输入读取的逆波计算
   器（reverse-polish calculator）。我们先 push 两个数字和一个操作符到堆栈
   中。然后我们使用 `p` 来打印输出结果。下面我们 push 5、10 及 +，结果标准
   输出获得了 15\n。

       $ irb
       >> require 'open3'
       => true
       >> stdin, stdout, stderr = Open3.popen3('dc')
       => [#<IO:fd 6>, #<IO:fd 7>, #<IO:fd 9>, #<Thread:0x816d46c sleep>]
       >> stdin.puts(5)
       => nil
       >> stdin.puts(10)
       => nil
       >> stdin.puts("+")
       => nil
       >> stdin.puts("p")
       => nil
       >> stdout.gets
       => "15\n"

   使用该命令我们不仅可以读取命令的输出，而且也能写到命令的标准输入。这允
   许我们灵活地处理与命令的交互。 

   如果我们需要，`popen3` 也将给我们标准错误。

       # (irb continued...)
       >> stdin.puts("asdfasdfasdfasdf")
       => nil
       >> stderr.gets
       => "dc: stack empty\n"

   使用 popen3 的缺点是 `$?` 不返回适当的退出状态。

       $ irb
       >> require 'open3'
       => true
       >> stdin, stdout, stderr = Open3.popen3('false')
       => [#<IO:fd 8>, #<IO:fd 10>, #<IO:fd 12>, #<Thread:0x8297644 sleep>]
       >> $?
       => nil
       >> $?.to_i
       => 0

   0？false 是假定返回非 0 退出状态。该缺点带我们到 Open4。 

6. Open4#popen4

   `Open4#popen4` 是 Ara Howard 创建的 Ruby Gem。它的操作与 `open3`
   相似，例外是我们能从程序获得退出状态。`popen4` 为 subshell 返回进程
   ID，这样我们能从等候的进程获得退出状态。（你需要安装 open4 gem） 

       $ irb
       >> require 'open4'
       => true
       >> pid, stdin, stdout, stderr = Open4::popen4 "false"
       => [17913, #<IO:fd 6>, #<IO:fd 7>, #<IO:fd 9>]
       >> $?
       => nil
       >> pid
       => 17913
       >> ignored, status = Process::waitpid2 pid
       => [17913, #<Process::Status: pid 17913 exit 1>]
       >> status.to_i
       => 256

   你也可以作为块来调用 `popen4`，它将自动等候退出状态。

       $ irb
       >> require 'open4'
       => true
       >> status = Open4::popen4("false") do |pid, stdin, stdout, stderr|
       ?>   puts "PID #{pid}"
       >> end
       PID 18535
       => #<Process::Status: pid 18535 exit 1>
       >> puts status
       pid 18535 exit 1
       => nil

{ via [tech.natemurray.com](http://tech.natemurray.com/2007/03/ruby-shell-commands.html) }
