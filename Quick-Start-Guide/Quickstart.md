安装 GlusterFS-快速入门指南
-------

# # # 本文档的目的
这份文件被要给你一步一步引导设置
第一次的 GlusterFS。在本教程中，我们将假设你是

使用 Fedora 22 （或更高版本） 虚拟机 （其他分布和方法可以是
在新的用户指南中，见下文。我们也不解释中的步骤

这里的细节，本指南是只是为了帮助你让它飞起来并运行作为
尽可能快。之后你将 GlusterFS 部署按照下面的步骤，

我们建议您阅读 GlusterFS 管理员指南，以了解如何
管理 GlusterFS 和如何选择卷类型适合你
需要。阅读更详细的解释 GlusterFS 新用户指南

步骤，我们就在这里。我们希望你能成功地作为短

尽可能的时间。

如果你想散步穿过要更详细的说明
安装使用不同的方法 (在本地虚拟机，EC2 中和
baremetal) 和不同的分布，然后看一看安装
指南。

# # # 自动部署与傀儡 GlusterFS-Gluster + 流浪

如果你想要部署 GlusterFS 自动使用
傀儡-Gluster + 流浪汉，看一看 [这
文章] (https://ttboj.wordpress.com/2014/01/08/automatically-deploying-glusterfs-with-puppet-gluster-vagrant/)。

# # # 第 1 步 – 有至少两个节点

-22 （或更高版本） 的 fedora 上名为"server1"和"server2"的两个节点
-工作的网络连接
-至少两个虚拟磁盘，对于 OS 安装，一个，一个要
用于服务 GlusterFS 存储 (sdb)。这将模仿真实

世界中进行部署，在那里你想要分开 GlusterFS 存储
从操作系统的安装。
-注意︰ GlusterFS 存储其动态生成的配置文件
在 /var/lib/glusterd。如果在任一时间点的 GlusterFS 是无法

写入这些文件 （例如，支持文件系统已满时），
它将在最低原因怪异的行为，为您的系统;或者更糟，

完全脱机您的系统。它是建议创建单独

对于诸如 /var/log 确保这不会发生这样的目录分区。

# # # 第 2 步-格式和装载砖

（两个节点上）︰ 注意︰ 这些例子要承担砖是
要驻留在 /dev/sdb1 上。

mkfs.xfs-i 大小 = 512/dev/sdb1
mkdir-p/数据/brick1
回声 '/ dev/sdb1 /data/brick1 xfs 默认值 1 2' > > fstab/等 /
装载-& & 山

现在，您应该看到 sdb1 安装在 /data/brick1

# # # 第 3 步-安装 GlusterFS

（在这两个服务器上）安装软件


百胜餐饮集团安装 glusterfs 服务器

启动 GlusterFS 管理守护进程︰

服务 glusterd 开始
服务 glusterd 状态
glusterd.service-LSB: glusterfs 服务器
加载︰ 加载 (/ etc/rc.d/init.d/glusterd)
主动︰ 主动 （运行） 自星期一 2012 年 8 月 13 日 13:02:11-0700;2s 前

过程︰ 19254 ExecStart=/etc/rc.d/init.d/glusterd 开始 (代码 = 退出，状态 = 0/成功)
CGroup: name=systemd:/system/glusterd.service
├ 19260 /usr/sbin/glusterd-p /run/glusterd.pid
├ 19304 /usr/sbin/glusterfsd — — xlator 选项 georep-server.listen-端口 = 24009-s localhost......
└ 19309 /usr/sbin/glusterfs-f /var/lib/glusterd/nfs/nfs-server.vol-p /var/lib/glusterd/...

# # # 第 4 步-配置受信任的池

从"server1"

gluster 同行探针 server2

注意︰ 当使用主机名，第一台服务器需要从探测
一个 * * * 其他服务器来设置其主机名。

从"server2"

gluster 同行探针 server1

注意︰ 一旦建立了此池，只有受信任的成员才可以
探头入池的新服务器。一个新的服务器不能探讨池，它

必须探讨从池中。

# # # 第 5 步-设置 GlusterFS 卷

在 server1 和 server2:

mkdir /data/brick1/gv0

从任何一台服务器︰

gluster 卷创建 gv0 副本 2 server1: / 数据/brick1/gv0 server2: / 数据/brick1/gv0
gluster 卷开始 gv0

请确认该卷显示"已启动":

gluster 卷信息

注意︰ 如果该卷不开始，问题出在哪里的线索将会
在上一个或两个服务器-在 /var/log/glusterfs 下的日志文件
通常在等-glusterfs-glusterd.vol.log

# # # 第 6 步-测试 GlusterFS 卷

这一步，我们将使用一台服务器要装入卷。
通常情况下，你会做这从外部的机器，称为
"客户端"。因为使用此方法将需要额外软件包到

安装在客户端计算机上，我们将使用的服务器作为一个
一个简单的地方，要考第一，就好像它那"客户端"。

装载-t glusterfs server1: / gv0 命令
		  for i in `seq -w 1 100`; do cp -rp /var/log/messages /mnt/copy-test-$i; done

首先，检查装载点︰

ls-lA /mnt |wc-l


您应该看到返回的 100 个文件。接下来，检查 GlusterFS 山

每个服务器上的点︰

ls-lA /data/brick1/gv0

您应该看到这里使用我们列出的方法每个服务器上的 100 个文件。
没有复制，分发唯一一卷 （不详细在这里），在你
应该会看到大约 50 上每个文件。

[用语](./ Terminologies.md) 你应该熟悉。

