# #Using Gluster 控制台管理器 — — 命令行实用程序

Gluster 控制台管理器是一个单一的命令行实用程序，
简化了配置和管理您的存储环境。的

Gluster 控制台管理器是类似于 LVM （逻辑卷管理器）
CLI 或 ZFS 命令行界面，但跨多个存储服务器。
您可以在网上，卷时使用 Gluster 控制台管理器
安装和激活。Gluster 自动同步卷

跨所有 Gluster 服务器的配置信息。

使用 Gluster 控制台管理器，您可以创建新的卷开始
卷和停止卷，作为所需。您还可以添加到砖

卷，从现有的卷删除砖以及更改
翻译设置，其他的操作之一。

您还可以使用命令来为自动化，以及创建脚本
作为使用的命令作为 API 允许与第三方集成
应用程序。

# # #Running Gluster 控制台管理器

你可以在任何 GlusterFS 服务器上运行 Gluster 控制台管理器或者
通过调用命令或通过在交互式运行 Gluster CLI
模式。您还可以使用 gluster 命令远程使用 SSH。


-对直接运行命令︰

    ` # gluster peer `

例如︰

    ` # gluster peer status `

-向在交互模式下运行 Gluster 控制台管理器

    `# gluster`

您可以从控制台管理器提示执行 gluster 命令︰

    ` gluster> `

例如，要查看的对等服务器的状态︰

    \# `gluster `

    `gluster > peer status `

显示对等的地位。


