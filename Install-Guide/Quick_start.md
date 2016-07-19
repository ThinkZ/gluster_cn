# # # 快速入门指南

这里是你需要去 Gluster 启动和运行的裸最低步骤。
如果这是你第一次的时间设置东西了，它建议你
即选择您所需的设置方法 [AWS](./Setup_aws.md)，
[虚拟机](./ Setup_virt.md) 或 [baremetal](./Setup_Bare_metal.md)

第四步后, 将条目添加到 fstab。如果你想要权

它 （和不需要更多的信息），下面的步骤都是你需要
若要开始。

你将需要有至少两个节点与 64 位操作系统和工作
网络连接。至少一个千兆的内存是最低限度的

建议进行测试，并且在任何系统中要将至少 8 GB
你计划做任何实际工作上。单个 cpu 是很好的测试，作为

只要它是 64 位。

# # # 分区，格式和装载砖

假设你有一块砖头在 /dev/康体发展局︰

fdisk/dev/康体发展局和创建一个分区

# # # 格式化分区

mkfs.xfs-i 大小 = 512-n 大小 = 8192/dev/sdb1

# # # 挂载分区作为 Gluster"砖"

mkdir-p/出口/sdb1 & & 山 /dev/sdb1/出口/sdb1 & & mkdir-p /export/sdb1/brick

# # # 添加一个条目到 /

回声"/ dev/sdb1/出口/sdb1 xfs 默认值 0 0"> > fstab/等 /

# # # 两个节点上安装 Gluster 软件包

rpm-Uvh http://download.gluster.com/pub/gluster/glusterfs/LATEST/Fedora/glusterfs {-server,-fuse,-geo-replication}-3.3.1-1.fc16.x86_64.rpm

* 注意︰ 本示例假设 Fedora 16 *

# # # 运行 gluster 同行探头命令

* 注意︰ 从一个节点到其他节点 （做不同行探讨第一
节点本身，应在受信任的池中的所有节点重复此命令） *

gluster 同行探针 < ip 或另一台主机的主机名 >

# # # 配置您 Gluster 卷

gluster 卷创建 testvol 副本 2 运输 tcp node01: / 出口/sdb1/砖 node02: / 出口/sdb1/砖

# # # 测试使用的卷

mkdir/产妇和新生儿破伤风/gluster;装载-t glusterfs node01: / testvol;cp-r/var/登录 /mnt/gluster

