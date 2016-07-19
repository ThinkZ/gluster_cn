# 介绍

GlusterFS 支持使用 RDMA 协议 glusterfs 客户和 glusterfs 砖之间进行通信。
GlusterFS 的客户包括保险丝客户端、 libgfapi 客户端 （Samba 和 NFS 甘尼萨包括）、 金纳米花服务器和其他沟通像自我修复守护程序，quotad，砖的 glusterfs 进程平衡过程等。

注︰ 截至现在唯一保险丝客户端和金纳米花的服务器会支持 RDMA 运输。


注意︰
NFS 客户端到服务器/NFS 甘尼萨服务器通讯金纳米花仍然会通过 tcp。
CIFS 客户端/Windows 客户端到 Samba 服务器通信仍然会通过 tcp。

# 安装程序
请参阅这些外部的文档，以在您的机器上安装 RDMA
http://pkg-ofed.alioth.debian.org/howto/infiniband-howto.html
http://people.redhat.com/dledford/infiniband_get_started.html

# # 创建信任存储池
受信任的存储池中的所有服务器都必须都有 RDMA 设备如果 RDMA 或 TCP，RDMA 卷在创建存储池。
必须使用 IP 主机名称分配给 RDMA 设备执行同行探讨。

# # 端口和防火墙
过程 glusterd 将监听 tcp 及 rdma 如果找到 rdma 设备。用于 rdma 端口是 24008。同样，砖过程，将为创建与运输"tcp，rdma"卷来侦听两个端口也。


请确保您更新防火墙，以接受对这些端口的数据包。

# Gluster 卷创建

卷可以支持一个或多个运输类型为客户和砖进程之间的通信。有三种类型的支持的传输，是，tcp，rdma，和 tcp，rdma。


示例︰ 要创建一个分布式的卷通过无限带宽与四个存储服务器︰

`# gluster volume create test-volume transport rdma server1:/exp1 server2:/exp2 server3:/exp3 server4:/exp4`  
创建测试卷已成功
请开始访问的数据量。

# 改变运输卷
若要更改现有卷的支持的传输类型，请按照步骤︰
注︰ 这是可能的只是如果该卷创建与 IP 主机名称分配给 RDMA 设备。

1.所有在客户端上使用下面的命令卸载卷︰
`# umount mount-point`  
2.停止卷使用下面的命令︰
`# gluster volume stop volname`  
3.改变的传输类型。
例如，若要启用 tcp 和 rdma 执行宣读命令︰
`# gluster volume set volname config.transport tcp,rdma`  
4.装入卷上的所有客户端。
例如，要使用 rdma 运输的装载，请使用以下命令︰
`# mount -t glusterfs -o transport=rdma server1:/test-volume /mnt/glusterfs`

注意︰
config.transport 选项帮助 gluster cli 中没有条目。
`#gluster vol set help | grep config.transport`  
然而，关键是有效的。

# 安装使用 RDMA 卷

装载"运输工具"选项用于指定保险丝客户端必须使用与砖进行通信的传输类型。如果只有一个运输类型创建了此卷，那成为默认时未不指定任何值。在 tcp，rdma 体积的情况下 tcp 是默认值。


例如，要使用 rdma 运输的装载，请使用以下命令︰
`# mount -t glusterfs -o transport=rdma server1:/test-volume /mnt/glusterfs`

# 由辅助进程使用的运输
像自愈守护进程的所有辅助进程，再平衡过程等都使用默认传输。如果你有一个 tcp，它将使用 tcp rdma 卷。 

在 rdma 体积的情况下将使用 rdma。
配置选项可供选择使用这些过程时体积是 tcp，rdma 交通工具尚不可用，将会在以后的版本中。



