# 在 GlusterFS 配置 NFS 甘尼萨

NFS 甘尼萨是支持 NFSv3，v4，v4.1，pNFS NFS 协议用户空间文件服务器。它提供了保险丝兼容文件系统抽象 Layer(FSAL)，允许文件系统开发者插上自己的存储机制和从任何 NFS 客户端访问。NFS 甘尼萨可以访问保险丝文件系统直接通过其 FSAL 没有将任何数据复制到或从内核，从而有可能提高响应时间。


# # 安装 nfs 甘尼萨

# # # Gluster Rpm (> = 3.7)
> glusterfs 服务器

> glusterfs api

> glusterfs 甘尼萨

# # # 甘尼萨 Rpm (> = 2.2)
> nfs 甘尼萨

> nfs-甘尼萨-gluster

# # 手动启动 NFS 甘尼萨

-手动启动 NFS 甘尼萨，使用命令︰
-* 服务 nfs 甘尼萨开始 *
```sh
地点︰
/var/log/ganesha.log 是甘尼萨进程的默认日志文件。
/etc/ganesha/ganesha.conf 是默认的配置文件
NIV_EVENT 是默认的日志级别。
```
-如果用户想要在首选模式下运行甘尼萨，执行以下命令︰
-*#ganesha.nfsd-f <location_of_nfs-ganesha.conf_file>-L <location_of_log_file>-N <log_level> *

```sh
例如︰
#ganesha.nfsd-f nfs ganesha.conf-L nfs ganesha.log-N NIV_DEBUG
地点︰
nfs ganesha.log 是 ganesha.nfsd 过程的日志文件。
nfs ganesha.conf 是一个配置文件
NIV_DEBUG 是日志级别。
```
-在默认情况下导出服务器将为 Null

```sh
注︰ 包括以下用于导出 gluster 卷甘尼萨配置文件参数
NFS_Core_Param {
#Use 提供的名字其他 tha IP 在 NSM 操作
NSM_Use_Caller_Name = true;
#Copy 锁国进"/ var/lib/nfs/甘尼萨"dir
群集 = false;
RQuota #Use 非特权端口
Rquota_Port = 4501;
}
```
# # 一步一步走到导出 GlusterFS 卷通过 NFS 甘尼萨的程序

# # # 第 1 步︰

若要导出任何 GlusterFS 卷或目录内卷，为每个这些导出配置文件中的条目创建出口块。以下参数需要导出的任何条目。

-* #cat export.conf*

```sh
出口 {
Export_Id = 1;  # 出口 ID 唯一的每个出口

路径 ="volume_path"; # 要导出的卷路径。如:"/ test_volume"


FSAL {
名称 = GLUSTER;
主机名 ="10.xx.xx.xx"; # IP 的受信任的游泳池中的节点之一

体积 ="卷名称";	 # 卷名。如:"test_volume"

}

Access_type = RW;	 # 访问权限

壁球 = No_root_squash;# 对启用/禁用根挤压

Disable_ACL = TRUE;	 # 要启用/禁用 ACL

伪 ="pseudo_path";	 # 此导出的 NFSv4 伪路径。如:"/ test_volume_pseudo"

协议 ="3"，"4";	 # NFS 协议支持

运输 ="UDP"，"TCP";# 传输协议支持

SecType ="sys";	 # 安全口味支持

}
```

# # # 第 2 步︰

现在包括导出配置文件在甘尼萨配置文件中 （默认情况下）。这可以通过在文件末尾添加下面的一行

-%，其中包括"< 配置导出路径 >"

# # # 第 3 步︰

-对检查是否出口量，运行
-* #showmount-e localhost *

# # 使用高度可用的主动-主动 NFS 甘尼萨和 GlusterFS cli
在一个高度可用的主动-主动环境，如果被连接到运行特定应用程序的 NFS 客户端的 NFS 甘尼萨服务器崩溃，应用程序/NFS 客户端无缝连接到另一个 NFS 甘尼萨服务器无需任何行政干预。
群集是使用心脏起搏器和 Corosync 来维护的。心脏起搏器行为的资源管理器和 Corosync 提供了群集的通信层。

使用 UPCALL 基础设施，实现了跨多机头 NFS 甘尼萨服务器群集中数据的一致性。UPCALL 基础设施是一个泛型和可扩展的框架，将通知发送到各自 glusterfs 客户 （在此案例 NFS 甘尼萨服务器） 在后端文件系统中检测到任何更改的情况下。


高可用性集群配置在以下三个阶段︰
# # # 创建甘尼萨 ha.conf 文件
时安装 Gluster 存储在以下位置 /etc/ganesha 创建甘尼萨 ha.conf.example。将该文件重命名为甘尼萨 ha.conf 和更改建议在下面的示例︰

甘尼萨 ha.conf 文件示例︰

> \ # 的 HA 集群创建名称。在子网内必须唯一


> HA_NAME ="甘尼萨-哈哈-360"

> \ # Gluster 服务器从中装入共享的数据卷。

> HA_VOL_SERVER ="server1"

> \ # Gluster 信任池的形成甘尼萨 HA 集群的节点的子集。

> \ # 指定主机名。

> HA_CLUSTER_NODES ="server1，server2，......"

> \#HA_CLUSTER_NODES="server1.lab.redhat.com,server2.lab.redhat.com，......"

> \ # 为每个节点上面指定虚拟 ip 地址。

> VIP_server1 ="10.0.2.1"

> VIP_server2 ="10.0.2.2"

# # # 配置 NFS 甘尼萨使用 gluster CLI
可以设置或拆除使用 gluster CLI HA 集群。此外，它可以导出和导出特定的卷。有关详细信息，请参阅节配置 NFS 尼萨使用 gluster CLI。


### Modifying the HA cluster using the `ganesha-ha.sh` script
Post the cluster creation any further modification can be done using the `ganesha-ha.sh` script. For more information, see the section Modifying the HA cluster using the `ganesha-ha.sh` script.

# # 分步指南
# # # 配置 NFS 甘尼萨使用 Gluster CLI
# # # 必备运行 NFS 甘尼萨
确保在您的环境中运行 NFS 甘尼萨之前考虑以下先决条件︰

* Gluster 存储卷必须是可供出口和 NFS 甘尼萨 rpm 安装。
* IPv6 必须使用的 NFS 甘尼萨守护进程的主机接口上启用。要启用 IPv6 支持，请执行以下步骤︰

-发表评论或删除线选项禁用 ipv6 = 1 /etc/modprobe.d/ipv6.conf 文件中的。
-重新启动系统。

* 确保群集中的所有节点都是可解析的 DNS。例如，您可以使用填充 /etc/hosts 与群集中的所有节点的详细信息。

* 禁用并停止网络管理器服务。
* 启用并在所有机器上启动网络服务。
* 创建并装入 gluster 共享卷。
安装心脏起搏器和 Corosync 在所有机器上。
* 在所有机器上设置群集验证密码。
* 无密码 ssh 需要医管局的所有节点上启用。请按照这些步骤操作，


-在群集中的一个 （主） 节点，运行︰
-ssh 凯基-f /var/lib/glusterd/nfs/secret.pem
-部署公钥 ~root/.ssh/authorized 键 _all_ 在节点上，运行︰
-ssh 副本 id-i /var/lib/glusterd/nfs/secret.pem.pub root@$node
-将密钥复制到 _all_ 节点在群集中，运行︰
-scp /var/lib/glusterd/nfs/secret.* $node: / var/lib/glusterd/nfs /

# # # 配置 HA 群集
若要安装 HA 集群，通过执行以下命令启用 NFS 甘尼萨︰

#gluster nfs 甘尼萨启用

拆掉 HA 集群，请执行以下命令︰

#gluster nfs 甘尼萨禁用

# # # 导出卷通过 NFS 甘尼萨
若要导出一个红色帽子 Gluster 存储卷，请执行以下命令︰

#gluster 卷上设置 <volname>ganesha.enable

要导出一个红色帽子 Gluster 存储卷，请执行以下命令︰

#gluster 卷出发 <volname>ganesha.enable

此命令不会影响其他出口 unexports 红色帽子 Gluster 存储卷。

若要验证状态的卷设置选项，请按照下面提到的指导原则︰

* 检查是否 NFS 甘尼萨通过执行下面的命令启动︰
-ps aux |grep 甘尼萨

* 检查是否出口量。
-showmount-e 本地主机

Ganesha.nfsd 守护进程的日志写入到 /var/log/ganesha.log 中。检查日志文件上发现任何意外的行为。


# # # 修改 HA 集群使用甘尼萨 ha.sh 脚本
若要修改现有的 HA 集群并更改默认出口值使用位于/usr/libexec/甘尼萨 / 甘尼萨 ha.sh 脚本。
# # # 向群集添加节点
Before adding a node to the cluster, ensure all the prerequisites mentioned in section `Pre-requisites to run NFS-Ganesha` is met. To add a node to the cluster. execute the following command on any of the nodes in the existing NFS-Ganesha cluster:

#./ganesha-ha.sh — — 添加 <HA_CONF_DIR> <HOSTNAME><NODE-VIP>
在那里，
HA_CONF_DIR︰ 包含甘尼萨 ha.conf 文件的目录路径。
主机名︰ 主机名的新节点被添加
节点-VIP︰ 新的节点要添加虚拟的 IP。
# # # 删除群集中的节点
要从群集中删除节点，请在任何现有的 NFS 甘尼萨群集中的节点上执行以下命令︰

#./ganesha-ha.sh — — 删除 <HA_CONF_DIR> <HOSTNAME>

在那里，
HA_CONF_DIR︰ 包含甘尼萨 ha.conf 文件的目录路径。
主机名︰ 主机名的新节点被添加
# # # 修改默认导出配置
若要修改默认导出配置在任何现有的甘尼萨群集中的节点上执行以下步骤︰

* Edit/add the required fields in the corresponding export file located at `/etc/ganesha/exports`.

* 执行以下命令︰

#./ganesha-ha.sh — — 刷新配置 <HA_CONFDIR> <volname>

在那里，
HA_CONF_DIR︰ 包含甘尼萨 ha.conf 文件的目录路径。
volname︰ 其出口配置已更改的卷的名称。

注意
出口 ID 一定不能更改。
⁠
# # 配置 pNFS Gluster 卷
并行的网络文件系统 (pNFS) 是允许计算客户端直接访问存储设备并同时 NFS v4.1 协议的一部分。PNFS 群集包括 MDS(Meta-Data-Server) 和 DS （数据服务器）。客户端发送直接到 DS 的所有读/写请求和所有其他操作会处理由 MDS。


# # # 一步一步一步指导

-打开卷 feature.cache 失效。
-gluster v 上设置 <volname>features.cache 失效

-选择一个节点作为 MDS 群集中并将其向甘尼萨配置文件中添加以下块配置
```sh
GLUSTER
{
PNFS_MDS = true;
}
```
-手动启动 NFS 甘尼萨在群集中的每个节点。

-检查是否通过 nfs 甘尼萨在所有节点中出口量。
-* #showmount-e localhost *

-装入卷的 MDS ip 使用 NFS 版本 4.1 协议
-* #mount-t nfs4-o minorversion:/ = 1 < ip MDS > < 卷名称 > < 装载路径 > *

# # # 应要注意的点

-当前体系结构只支持单个 MDS 和组织架构 DS。坐骑将会与哪些客户端行为作为 MDS 和所有服务器包括 MDS 服务器可以充当 DS。


-目前医管局不是支持 pNFS （更多方言 MDS）。虽然它是可配置的但整个群集内保证一致性。


-如果任何 DS 下山时，时，MDS 将处理那些我 / O 的

-以后，所有随后的 NFS 客户端需要使用相同的服务器安装通过 pNFS 该卷。更多比一个 MDS 的卷不是首选，即


-pNFS 支持只测试与分布式、 复制或分发复制卷

-它测试和验证与 RHEL 6.5，fedora 20，fedora 21 nfs 客户端。它始终是更好地使用最新的 nfs 客户端


