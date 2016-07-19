访问 GlusterFS 使用煤渣主机
======================================

1.介绍
===============

GlusterFS 和煤渣一体化提供使用户能够访问相同的数据，作为一个对象和一个文件中，从而简化管理和控制存储成本的数据存储系统。

*GlusterFS*-GlusterFS 是一个开放源码的分布式的文件系统，能够扩展到几个字节和处理数千个客户端。GlusterFS 聚集在一起，存储构建基块在无限带宽 RDMA 或 TCP/IP 互连，聚合磁盘和内存资源，并在一个单一的全局命名空间中管理数据。GlusterFS 基于可堆叠用户空间设计，可以提供卓越的性能，为不同的工作负载。


* 渣 *-渣是 OpenStack 服务，负责处理持久性存储的虚拟机。这是持久性数据块存储在新星运行的实例。快照可以备份和还原数据，或者用于创建新的块存储卷的数据。


与企业 Linux 6，配置 OpenStack 灰熊 GlusterFS 用于其渣 （块） 存储是相当简单的。

使用 GlusterFS 3.3 和 GlusterFS 3.4 测试过这些指令。其他版本也可以工作，但尚未经过测试。


2.系统必备组件
================

GlusterFS
---------

Prerequsities 和安装 GlusterFS 的说明信息，请参阅 < http://www.gluster.org/community/documentation/index.php >。

煤渣
------

Prerequsities 和安装煤渣的说明信息，请参阅 < http://docs.openstack.org/ >。

在开始之前，你必须确保有 * * 没有现有卷 * * 烧渣中。使用"煤渣删除"来删除任何，和"煤渣列表"来验证删除它们。如果您不删除现有的煤渣卷，它将导致错误后在过程中，打破你的煤渣安装。


**NOTE**-与其他软件不同的是"openstack 配置"和"渣"命令一般需要你以根用户身份运行它们。而无需事先配置，一般通过 sudo 运行他们不工作。（这可以改变的但超出这个操作的范围）。


3 安装 GlusterFS 客户端在煤渣主机上
=============================================

在煤渣的每台主机上安装 GlusterFS 客户端软件包︰

sudo 百胜 y $ 安装 glusterfs 熔断器

4.配置煤渣添加 GlusterFS
======================================

在每个煤渣主机上，运行以下命令以将 GlusterFS 添加到煤渣配置︰

# openstack config--设置的 /etc/cinder/cinder.conf 默认 volume_driver cinder.volume.drivers.glusterfs.GlusterfsDriver
# openstack config--设置的 /etc/cinder/cinder.conf 默认 glusterfs_shares_config /etc/cinder/shares.conf
# openstack config--设置的 /etc/cinder/cinder.conf 默认 glusterfs_mount_point_base /var/lib/cinder/volumes

5.创造 GlusterFS 卷列表
=================================

在每个煤渣节点上创建一个简单的文本文件 * * / etc/cinder/shares.conf**。

此文件是一个简单的 GlusterFS 卷将用于列表每行一个，使用以下格式︰

GLUSTERHOST:VOLUME
GLUSTERHOST:NEXTVOLUME
GLUSTERHOST2:SOMEOTHERVOLUME

例如︰

myglusterbox.example.org:myglustervol

6.更新防火墙为 GlusterFS
==================================

您必须更新每个煤渣节点与 GlusterFS 节点进行通信的防火墙规则。

要打开的端口在步骤 3 中进行了说明︰

< http://gluster.org/community/documentation/index.php/Gluster_3.2:_Installing_GlusterFS_on_Red_Hat_Package_Manager_ (RPM) _Distributions >

如果你使用 iptables 作为您的防火墙，可以将这些行添加下 * *: 输出接受 * *"\*filter"一节中。你可能应该调整它们来适合您的环境 (如.只接受来自您的 GlusterFS 服务器的连接)。


-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 111-j 接受
-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 24007-j 接受
-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 24008-j 接受
-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 24009-j 接受
-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 24010-j 接受
-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 24011-j 接受
-输入-m 状态 — — 国家新-m tcp-p tcp — — dport 38465:38469-j 接受

重新启动防火墙服务︰

$ sudo 服务 iptables 重新启动

7.重新启动煤渣服务
=============================

配置已完成，现在你必须重新启动煤渣服务以激活它。

$ 为我在 api 调度器体积;不要使用 sudo 服务 openstack-渣-${i} 开始;完成


检查煤渣卷日志，以确保没有错误︰

$ sudo 尾巴-50 /var/log/cinder/volume.log

8.验证与煤渣 GlusterFS 集成
===========================================

要验证是否成功安装和配置，创建煤渣卷然后检查使用 GlusterFS。

创建煤渣卷︰

# 煤渣创建 — — display_name myvol 10

创建卷需要几秒钟。一旦创建，请运行以下命令︰


# 煤渣列表

该卷应处于"可用"状态。现在，寻找 GlusterFS 卷目录中的新文件︰


$ sudo ls-啦 /var/lib/cinder/volumes/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX /

（XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX 将大量特定于您的安装）

新创建的文件应该在这个目录是您刚刚创建的新卷中。新的文件将显示每次创建一个卷。


例如︰

$ sudo ls-啦 /var/lib/cinder/volumes/29e55f0f3d56494ef1b1073ab927d425 /
总计 4.0 K
drwxr-xr-x。3 根 73 Apr 4 15:46。

drwxr-xr-x。3 煤渣煤渣 4.0 K Apr 3 9:31。

-rw-rw-rw-。1 根根 10g Apr 4 15:46 volume-a4b97d2e-0f8e-45b2-9b94-b8fa36bd51b9
