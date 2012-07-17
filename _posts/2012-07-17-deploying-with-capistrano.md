---
layout: post
title: "使用 Capistrano 部署 Web 应用"
category: howto
---

### 简介

Capistrano 是一个 Ruby 程序，它提供高级的工具集来部署你的 Web
应用到服务器上。Capistrano 允许你通过 SSH 从源代码控制仓库（SVN 或
Git）复制代码到服务器，并执行如重启 Web
服务器、操作缓存、重命名文件、迁移数据库等部署前/后的功能。利用 Capistrano
一次也可部署多台机器在。

本指南并未覆盖 Capistrano
提供的被用于部署多种应用的高级选项。作为学习目的，我们将指引你设置一个简单的
recipe（食谱），以用于从 SVN 或 Git 仓库部署 Rails
应用到单台服务器。我们将覆盖允许你部署多种环境（临时和生产）的常用工作流。

### 安装 Capistrano

为了安装 Capistrano，你的电脑需要已安装 Ruby 和 RubyGems。如果你运行 Mac OS X
10.5+，你已经准备就绪。如果没有，可参考这里来安装 [Ruby 及 RubyGems][r]。

然后从终端或命令行执行下列命令：

    gem install capistrano

我们推荐安装 **capistrano-ext** gem，它包含使你部署更加容易的扩展工具集：

    gem install capistrano-ext

如果你遇到了问题或想要了解更多细节，可以参考官方的 Capistrano
[入门指南][g]，或找到使 Capistrano 工作的[各种组件][c]。

**服务器依赖**

确保你的服务器是 POSIX 兼容，且具有 SSH 访问。别忘记设置 SSH 密钥（[Mac][m] 或
[Windows][w]）。如果你无法 SSH 到服务器，那么 Capistrano 将不会工作。

### 准备项目

在终端中导航到你的应用根目录，并执行以下命令：

    $ capify .

该命令在你的项目中创建一个特殊的 Capfile 文件，并在 Rails 项目中添加模板部署
recipe（config/deploy.rb）。Capfile 将帮助 Capistrano 正确加载你的 recipe
和库，现在你不需要编辑它。

作为替代，在你喜欢的文本编辑器中打开 deploy.rb
文件。该文件即是所有魔法的发生源。你可以删除模板文件中的内容，本指南将帮助你编写用于成功部署
recipe 的正确代码。

### 编写 Capistrano Recipe

**创建 Capistrano Recipe**

在你现有的空 deploy.rb
文件中，让我们在第一行输入应用的名称。如果你的应用名称是“fancy
shoes”，则输入：

    set :application, "fancy_shoes"

接着我们添加要访问的仓库。Git 用户可以添加：

    set :scm, :git
    set :repository, "git@account.beanstalkapp.com:/repository.git"
    set :scm_passphrass, ""

Subversion 用户需添加：

    set :scm, :subversion
    set :repository, "git@account.beanstalkapp.com:/repository.git"

然后，我们设置服务器的帐号：

    set :user, "server-user-name"

确保该帐号具有 `deploy_to` 变量所指定目录的读写访问权限。

在继续前，从你的终端（试试 svn co 或 git
clone）尝试手动连接仓库，以确保你能正确授权。如果你无法连接到仓库，那么
Capistrano 也不能。

下面我们添加服务器信息到 recipe。我们将使用 **Capistrano Multistage** 功能（由
capistrano-ext 随附）。这允许你设置一个 recipe
来部署代码到多个位置。在本例中，我们将部署到临时和生产环境。

在你的 deploy.rb 文件开头包含 multistage：

    require 'capistrano/ext/multistage'

然后指定你的环境，或“stages”：

    set :stages, ["staging", "production"]
    set :default_stage, "staging"

因为部署到临时环境比生产环境更经常，所以我们使临时环境成为默认
stage。接着，在你应用的 config 目录中创建 deploy 目录，并添加 production.rb 和
staging.rb 文件。对于你配置的每个 stage 都需要一个 Ruby
文件，它们的名称需要相同。当你指定哪个 stage 想要部署时，Capistrano 方能加载适合的文件。

现在让我们添加 production.rb 设置：

    server "my_fancy_server.com", :app, :web, :db, :primary => true
    set :deploy_to, "/var/www/fancy_shoes"

接着是 staging.rb：

    server "my_fancy_server.com", :app, :web, :db, :primary => true
    set :deploy_to, "/var/www/fancy_shoes_staging"

在本例中，我们仅有的一个服务器被分配了三个任务（app、web 及
db）。生产环境与临时环境的差别是 `deploy_to`
目录变量。事实上，你可能也想使用不同的服务器。

**验证 Recipe××

全部准备好后，先试试我们的 recipe，以便让 Capistrano
在服务器上创建初始的目录结构。从你的应用根目录执行下列命令：

    $ cap deploy:setup

当你执行该命令时，Capistrano 将 SSH 到你的服务器，进入你在 `deploy_to`
变量中所指定的目录，并创建特殊的 Capistrano 目录结构。如果遇到权限或 SSH
访问错误，你将获得错误消息。当命令执行时仔细看看 Capistrano 的输出。

在我们使用 Capistrano 做实际部署之前的最后一步是，确保 setup
命令在服务器上全都设置正确。使用以下命令进行简单验证：

    $ cap deploy:check

该命令将检查你的本机环境及服务器，并定位问题。如果你看到错误消息，修复后再运行此命令。一旦你执行
`cap deploy:check` 没有错误，则可继续处理。

**使用新的 Recipe 部署**

一旦验证通过你的本机及服务器配置，执行下列命令：

    $ cap deploy

这将执行部署到你的默认 stage，即临时环境。如果你想部署到生产环境，执行：

    $ cap production deploy

当命令执行时，你将看到许多输出。Capistrano
打印在服务器上执行的所有命令及其输出，以便有问题时调试。

### 提示与技巧

**使用远端缓存改进性能**

Capistrano
的工作方式在每次部署时都将创建新的仓库克隆及导出。那必将很慢，通过添加一些扩展选项到
deploy.rb recipe 则可提速。添加下列内容到 deploy.rb 文件描述 `scm`
设置的位置：

    set :deploy_via, :remote_cache

此命令使 Capistrano 在服务器上只克隆/导出仓库一次，然后在每次部署时使用 `svn
up` 或 `git pull` 代替。如果你经常部署，你将发现提速明显。

**添加定制的部署钩子（Hook）**

Capistrano 显然比通过 SSH
复制文件要高级。你可以配置事件或命令以便在文件复制完成后执行，如重启 Web
服务器、执行定制的脚本等。Capistrano 称这些为“任务”。例如，添加以下代码到
deploy.rb 文件：

    namespace :deploy do
      task :restart, :roles => :web do
        run "touch #{ current_path }/tmp/restart.txt"
      end
    
      task :restart_daemons, :roles => :app do
        sudo "monit restart all -g daemons"
      end
    end

Capistrano 中的任务非常强大，我们在本指南中仅接触到表皮。你可以创建任务在部署
前、部署后或单独操作服务器。这可以是任何维护类型：重启进程、清理文件、发送
邮件通知、执行数据库迁移、运行脚本等等。

我们的示例包括两个定制任务。“restart”任务是内建于 Capistrano
中的，将在部署完成后自动执行。我们使用由 Passenger 驱动的现代 Rails 应用技术
touch tmp/restart.txt，你的 Web 服务器可能需要不同的命令。

我们的第二个示例任务是“restart_daemons”，Capistrano
不会默认执行此定制任务。为了让它运行，我们需要添加一个 hook：

    after "deploy", "deploy:restart_daemons"

此命令告诉 Capistrano 在我们的部署操作完成后执行任务。其他可用的 hook 是
before，将在文件复制之前执行任务。

关于 before 及 after hook，你可以阅读官方的 Capistrano 文档：

* [Before Tasks][b]
* [After Tasks][a]

**将 Git 分支与环境关联**

因为我们有两个服务器环境（临时和生产），你可能想要绑定 Git
分支到这些环境。这样，你可以自动部署 staging 分支到临时环境，master
分支到生产环境。简单添加下列内容到 production.rb：

    set :branch, 'production'

并添加以下内容到 staging.rb：

    set :branch, 'staging'

现在每次你执行 `cap deploy` 时，Capistrano 将从你的 staging 分支（因为 staging
是我们的默认环境）部署代码。如果你运行 `cap production deploy`，Capistrano
将从你的 master 分支部署代码。

### 扩展阅读

希望我们的指南能够助你你快速入门。我们建议你去 Capistrano 的官方 [Wiki][i]，从
[From The Beginning][f] 文章开始阅读。

[r]: http://rubyonrails.org/download
[g]: https://github.com/capistrano/capistrano/wiki/2.x-Getting-Started
[c]: https://github.com/capistrano/capistrano/wiki/2.x-From-The-Beginning
[m]: http://guides.beanstalkapp.com/version-control/git-on-mac.html#creating-ssh-keys
[w]: http://guides.beanstalkapp.com/version-control/git-on-windows.html#installing-ssh-keys
[b]: https://github.com/capistrano/capistrano/wiki/2.x-DSL-Configuration-Tasks-Before
[a]: https://github.com/capistrano/capistrano/wiki/2.x-DSL-Configuration-Tasks-After
[i]: https://github.com/capistrano/capistrano/wiki 
[f]: https://github.com/capistrano/capistrano/wiki/2.x-From-The-Beginning

{ 本文编译自 [Deploying with Capistrano](http://guides.beanstalkapp.com/deployments/deploy-with-capistrano.html) by Ilya Sabanin }
