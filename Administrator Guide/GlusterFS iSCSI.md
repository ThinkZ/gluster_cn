介绍
------------

在 Gluster 上的 iSCSI 可以设置使用 Linux 目标驱动程序。这是一个用户空间守护程序接受 iSCSI （以及伊瑟尔和 FCoE。）它解释 iSCSI Cdb 并将它们转换成其他 I/O 操作，根据用户配置。在我们的例子，我们可以将 Cdb 转换对 gluster 文件运行的文件操作。该文件所代表的 LUN 和 LBA 文件中的偏移量。


插件为 Linux 目标驱动程序已经有了使用 libgfapi。它是 Linux 目标驱动程序 (bs\_glfs.c) 的一部分。使用它，数据通路跳过保险丝。会对此文档进行更新，来描述如何使用它。你可以看到 README.glfs 在 Linux 目标驱动程序的文件的子目录中。


郭嘉是替代品，Linux 目标驱动程序包含在 RHEL7 中。它的用户空间插件机制正在发展。一旦这段代码存在可以为 gluster 建立类似的机制，如 Linux 目标驱动程序的做法。


下面是一本食谱，它设置在服务器上使用 Linux 目标驱动程序。这已经被测试的 XEN 与 KVM 实例 RHEL6、 RHEL7 和 Fedora 19 实例中。在此设置中单个路径通向 gluster，代表一个性能瓶颈和单点故障。房委会和负载平衡，就可以安装两个或更多路径到不同 gluster 服务器使用 mpio;如果目标名称在每个路径等效，mpio 将洼地两成单个设备的路径。


在 iSCSI 和 Linux 上的详细信息的目标驱动程序，请参见 [1] 和 [2]。

安装程序
-----

在 gluster 服务器上本地挂载 gluster。注意您也可以在 gluster 客户端运行它。有到这些配置中，描述了利弊 [下] (#Running_the_target_on_the_gluster_client"wikilink")。


# 山-t glusterfs 127.0.0.1:gserver 命令

创建一个大的文件，代表你在 gluster fs 内的块设备。在这种情况下，lun 是 2 G。（<i>您还可以为此目的，它会跳过文件系统创建 gluster"块设备"</i>）。


# dd 如果 = / dev/零的 = disk3 bs = 2 G count = 25

创建一个使用该文件作为后端存储的目标。

如有必要，下载 Linux SCSI 目标。然后启动该服务。


# yum 安装 scsi 目标 utils
# 服务 tgtd 开始

你必须给 iSCSI 限定的名称 (IQN) 格式︰ iqn.yyyy mm.reversed.domain.name:OptionalIdentifierText

地点︰

yyyy mm 代表的 4 位数字的年份和 2 位数月份启动的设备 (例如︰ 2011年-07)

# tgtadm — — lld iscsi — — op 新 — — 模式目标 — — 工贸 1-T iqn.20013 10.com.redhat

你可以看看目标︰

# tgtadm — — lld iscsi — — op 显示 — — 模式 conn — — 工贸 1

会话︰ 11 连接︰ 0 启动器 iqn.1994-05.com.redhat:cf75c8d4274d

接下来，添加一个逻辑单元到目标

# tgtadm — — lld iscsi — — op 新 — — 模式 logicalunit — — 工贸 1 — — lun 1-b/产妇和新生儿破伤风/disk3

允许任何启动器来访问该目标。

# tgtadm — — lld iscsi — — op 绑定 — — 模式目标 — — 工贸 1-我所有

现在是时间来设置您的客户端。

发现你的目标。注意在此示例中的目标 IP 地址是 192.168.1.2


# iscsiadm — — 模式发现 — — 键入 sendtargets — — 门户 192.168.1.2

登录到您的目标会话。

# iscsiadm — — 模式节点 targetname — — iqn.2001 — — 04.com.example:storage.disk1.amiens.sys1.xyz — — 门户网站 192.168.1.2:3260 — — 登录

你应该有一个新的 SCSI 磁盘。你会看到它在 /var/log/messages 中创建。你会看到它在 lsblk。


您可以向它发送 I/O:

# dd 如果 = / dev/零的 = / /dev/sda bs = 4k count = 100

拆掉您的 iSCSI 连接︰

# iscsiadm-m 节点-T iqn.2001-04.com.redhat-p 172.17.40.21-u

在 gluster 客户端上运行 iSCSI 目标
----------------------------------------------

你可以在 gluster 客户端运行 Linux 目标守护进程。此安装程序的优点是客户端可以运行 gluster 并享受所有 gluster 的好处。例如，gluster 可以"扇出"对不同 gluster 服务器的 i/o 操作。负面影响将是客户端将需要加载并配置 gluster。它是更好地在客户端上运行 gluster，如果可能的话。


引用
----------

[1] < http://www.linuxjournal.com/content/creating-software-backed-iscsi-targets-red-hat-enterprise-linux-6 >

[2] < http://www.cyberciti.biz/tips/howto-setup-linux-iscsi-target-sanwith-tgt.html >
