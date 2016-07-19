#Accessing 数据-设置 GlusterFS 客户机

你可以访问 gluster 卷在多个方面。您可以使用 Gluster

本机客户端方法对于高并发性、 性能和透明
在 GNU/Linux 客户端中的故障转移。您还可以使用 NFS v3 访问 gluster

卷。广泛的测试已经对 GNU/Linux 客户端和 NFS

在其他的操作系统，如 FreeBSD 和 Mac OS X，执行
Windows 7 （专业和向上） 和 Windows Server 2003。
其他 NFS 客户端实现可能使用 gluster NFS 服务器。

您可以使用 CIFS 访问卷以及使用 Microsoft Windows 时
作为 SAMBA 客户端。对于这种访问方法，Samba 软件包需要

目前在客户端上。

##Gluster 本机客户端

Gluster 本机客户端是运行在用户空间中基于保险丝的客户端。
Gluster 本机客户端是用于访问卷的推荐的方法
当高并发性和高写入性能需要。

本节介绍了 Gluster 本机客户端，并说明如何
在客户机上安装软件。本节还介绍了如何

要在客户机上安装卷，（手动和自动） 和如何
验证已成功装入该卷。

# # #Installing Gluster 本机客户端

你开始安装 Gluster 本机客户端之前，您需要向
验证保险丝模块加载客户端上，并且有权访问
所需的模块，如下所示︰

1.将保险丝可加载内核模块 (LKM) 添加到 Linux 内核︰

    `# modprobe fuse`

2.验证已加载保险丝模块︰

    `# dmesg | grep -i fuse `
    `fuse init (API version 7.13)`

# # # 上红色的帽子程序包管理器 (RPM) 分布的安装

在 RPM 基于分布的系统上安装 Gluster 本机客户端

1.在使用下面的客户端上安装所需的系统必备组件
命令︰

    `$ sudo yum -y install openssh-server wget fuse fuse-libs openib libibverbs`

2.确保 TCP 和 UDP 端口 24007 和 24008，开放所有
Gluster 服务器。除了这些端口，您需要打开一个端口

从端口 49152 （而不是 24009 起作为开始每个砖
与以前的版本）。现就砖端口分配计划

符合 IANA 准则。例如︰ 如果你有

五个砖块，你需要有端口 49152 到 49156 开放。

您可以使用以下链与 iptables:

    `$ sudo iptables -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 24007:24008 -j ACCEPT `
    `$ sudo iptables -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 49152:49156 -j ACCEPT`

> * * 注意 * *
>
> 如果你已经有 iptable 链，确保上述
> 接受规则之前滴规则。这可以通过

> 提供较低的规则数比下降规则。

3.下载最新的 glusterfs、 glusterfs 熔断器和 glusterfs rdma
RPM 文件添加到每个客户端。Glusterfs 包包含 Gluster

本机客户端。Glusterfs 熔断器包包含保险丝

翻译所需的客户端系统上安装和
glusterfs rdma 软件包包含 OpenFabrics 动词 RDMA 模块
无限带宽。

你可以下载软件在 [GlusterFS 下载页面] [1]。

4.在客户端上安装 Gluster 本机客户端。

    `$ sudo rpm -i glusterfs-3.3.0qa30-1.x86_64.rpm `
    `$ sudo rpm -i glusterfs-fuse-3.3.0qa30-1.x86_64.rpm `
    `$ sudo rpm -i glusterfs-rdma-3.3.0qa30-1.x86_64.rpm`

> * * 注意 * *
>
> RDMA 模块只是使用无限带宽时所需。

# # # 上基于 Debian 的发行版的安装

要在基于 Debian 的发行版上安装 Gluster 本机客户端

1.使用以下命令每个客户端上安装 OpenSSH 服务器︰

    `$ sudo apt-get install openssh-server vim wget`

2.对每个客户端下载最新的 GlusterFS 的.deb 文件和校验和。

你可以下载软件在 [GlusterFS 下载页面] [1]。

3.对于每一个.deb 文件，得到的校验和 （使用下面的命令）
和比较中的 md5sum 文件的校验和
文件。

    `$ md5sum GlusterFS_DEB_file.deb `

包的 md5sum 是可用在: [GlusterFS 下载页面] [2]

4.从客户端卸载 GlusterFS 3.1 版 （或更早版本）
使用以下命令︰

    `$ sudo dpkg -r glusterfs `

    (Optional) Run `$ sudo dpkg -purge glusterfs `to purge the
配置文件。

5.在使用下面的客户端上安装 Gluster 本机客户端
命令︰

    `$ sudo dpkg -i GlusterFS_DEB_file `

例如︰

    `$ sudo dpkg -i glusterfs-3.3.x.deb `

6.确保 TCP 和 UDP 端口 24007 和 24008，开放所有
Gluster 服务器。除了这些端口，您需要打开一个端口

从端口 49152 （而不是 24009 起作为开始每个砖
与以前的版本）。现就砖端口分配计划

符合 IANA 准则。例如︰ 如果你有

五个砖块，你需要有端口 49152 到 49156 开放。

您可以使用以下链与 iptables:

    `$ sudo iptables -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 24007:24008 -j ACCEPT `
    `$ sudo iptables -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 49152:49156 -j ACCEPT`

> * * 注意 * *
>
> 如果你已经有 iptable 链，确保上述
> 接受规则之前滴规则。这可以通过

> 提供较低的规则数比下降规则。

# # # 执行源安装

生成并从源代码安装 Gluster 本机客户端

1.创建一个新的目录，使用下列命令︰

    `# mkdir glusterfs `
    `# cd glusterfs`

2.下载的源代码。

你可以下载源代码在 [] [1]。

3.解压缩源代码使用下面的命令︰

    `# tar -xvzf SOURCE-FILE `

4.运行配置实用程序使用下面的命令︰

    `# ./configure `

GlusterFS 配置摘要
===========================
保险丝客户︰ 是的
无限带宽动词︰ 是的
epoll 方面 IO 复用︰ 是的
argp 独立︰ 没有
fusermount︰ 没有
readline︰ 是的

配置摘要显示将生成的组件
与 Gluster 本机客户端。

5.建立使用以下的 Gluster 本机客户端软件
命令︰

    `# make `
    `# make install`

6.验证 Gluster 本机客户端的正确版本
使用下面的命令安装︰

    `# glusterfs –-version`

##Mounting 卷

安装后 Gluster 本机客户端，您需要安装 Gluster
若要访问数据的卷。有两种方法可以选择︰


-[手动安装 Volumes](#manual-mount)
-[自动挂载 Volumes](#auto-mount)

> * * 注意 * *
>
> 卷创建过程中选择的服务器名称应该是可解析
> 在客户端计算机。您可以使用适当/等/主机条目或

> DNS 服务器将服务器名称解析为 IP 地址。

<a name="manual-mount"></a>
# # # 手动装入卷

-要装入卷，请使用以下命令︰

    `# mount -t glusterfs HOSTNAME-OR-IPADDRESS:/VOLNAME MOUNTDIR`

例如︰

    `# mount -t glusterfs server1:/test-volume /mnt/glusterfs`

> * * 注意 * *
>
> 在 mount 命令中指定的服务器仅用于读取
> gluster 配置 volfile 描述的卷名。
> 随后，客户端将直接沟通
> 服务器所述 volfile (甚至可能不包括
> 一用于装载)。
>
> 如果你看到喜欢的用法信息"用法︰ mount.glusterfs"，装入
> 通常要求您创建一个目录来用作装载
> 点。在您尝试运行之前运行"mkdir/产妇和新生儿破伤风/glusterfs"

> 安装上面列出的命令。

* * 安装选项 * *

使用时，您可以指定以下选项
`mount -t glusterfs` command. Note that you need to separate all options
用逗号。

backupvolfile 服务器 = 服务器名称

volfile 最大读取次数 = 尝试次数

日志级别 = 日志级别

日志文件 = 日志文件

运输 = 运输类型

直接 io 模式 = [启用 | 禁用]

使用 readdirp = [是 | 没有]

例如︰

`# mount -t glusterfs -o backupvolfile-server=volfile_server2,use-readdirp=no,volfile-max-fetch-attempts=2,log-level=WARNING,log-file=/var/log/gluster.log server1:/test-volume /mnt/glusterfs`

If `backupvolfile-server` option is added while mounting fuse client,
当第一台 volfile 服务器失败了，然后在指定的服务器
`backupvolfile-server` option is used as volfile server to mount the
客户端。

In `volfile-max-fetch-attempts=X` option, specify the number of
试图装入卷同时提取卷文件。此选项是

有用装载具有多个 IP 地址的服务器时，或者当
循环复用 DNS 配置为服务器名称。

If `use-readdirp` is set to ON, it forces the use of readdirp
fuse 内核模块模式

<a name="auto-mount"></a>
# # # 自动挂载卷

您可以配置您的系统将自动装入 Gluster 卷
每次您的系统启动。

Mount 命令中指定的服务器仅用于读取
gluster 配置 volfile 描述的卷名。随后，

客户端将直接与所述服务器通信
volfile （这甚至可能不包含一个用于装载）。

-装入卷，编辑 /etc/fstab 文件中，并添加以下内容
行︰

    `HOSTNAME-OR-IPADDRESS:/VOLNAME MOUNTDIR glusterfs defaults,_netdev 0 0 `

例如︰

    `server1:/test-volume /mnt/glusterfs glusterfs defaults,_netdev 0 0`

* * 安装选项 * *

更新 /etc/fstab 文件时，您可以指定下列选项。
请注意，您需要用逗号分隔的所有选项。

日志级别 = 日志级别

日志文件 = 日志文件

运输 = 运输类型

直接 io 模式 = [启用 | 禁用]

使用 readdirp = 否

例如︰

`HOSTNAME-OR-IPADDRESS:/VOLNAME MOUNTDIR glusterfs defaults,_netdev,log-level=WARNING,log-file=/var/log/gluster.log 0 0 `

# # # 测试安装卷

若要测试已装入的卷

-使用下面的命令︰

    `# mount `

如果成功装入 gluster 卷的输出
mount 命令在客户端上的将类似于以下示例︰

    `server1:/test-volume on /mnt/glusterfs type fuse.glusterfs (rw,allow_other,default_permissions,max_read=131072`

-使用下面的命令︰

    `# df`

在客户端上的 df 命令的输出将显示聚合
从所有的砖块与此类似的卷中的存储空间
示例︰

    `# df -h /mnt/glusterfs Filesystem Size Used Avail Use% Mounted on server1:/test-volume 28T 22T 5.4T 82% /mnt/glusterfs`

-更改到的目录并列出其内容通过输入
之后︰

    `# cd MOUNTDIR `
    `# ls`

-例如，

    `# cd /mnt/glusterfs `
    `# ls`

#NFS

您可以使用 NFS v3 访问到 gluster 卷。广泛的测试了

在 GNU/Linux 客户端和 NFS 实现在其他业务上进行
FreeBSD，以及 Mac OS X 和 Windows 7 系统
（专业人员和向上），Windows Server 2003，和其他人，可能与工作
gluster NFS 服务器执行。

GlusterFS 现在包括网络锁管理器 (NLM) v4。NLM 启用

nfsv3 时客户端上的应用程序做记录锁定在 NFS 上的文件
服务器。它是自动启动，每当运行 NFS 服务器。


您必须在服务器和客户端 （仅限上安装 nfs 共同包
为基于 Debian 的) 分布。

本节介绍如何使用 NFS 挂载 Gluster 卷 （两个
手动和自动） 和如何验证该卷已
安装成功。

# #Using NFS 挂载卷
--------------------------

您可以使用下面的方法来装载 Gluster 卷之一︰

-[手动装入卷使用 NFS](#manual-nfs)
-[自动挂载卷使用 NFS](#auto-nfs)

* * 前提 * *: 在服务器和客户端上安装 nfs 共同包
（仅适用于基于 Debian 的通讯），使用下面的命令︰

`$ sudo aptitude install nfs-common `

<a name="manual-nfs"></a>
# # # 手动装入卷使用 NFS

* * 到手动装入 Gluster 卷使用 NFS * *

-要装入卷，请使用以下命令︰

    `# mount -t nfs -o vers=3 HOSTNAME-OR-IPADDRESS:/VOLNAME MOUNTDIR`

例如︰

    `# mount -t nfs -o vers=3 server1:/test-volume /mnt/glusterfs`

> * * 注意 * *
>
> Gluster NFS 服务器不支持 UDP。如果你是 NFS 客户端

> 使用连接使用 UDP，以下消息的默认值
> 出现︰
>
    > `requested NFS version or transport protocol is not supported`.

* * 到连接使用 TCP * *

-向 mount 命令添加下列选项︰

    `-o mountproto=tcp `

例如︰

    `# mount -o mountproto=tcp -t nfs server1:/test-volume /mnt/glusterfs`

* * 到装载 Gluster NFS 服务器从 Solaris 客户机 * *

-使用下面的命令︰

    `# mount -o proto=tcp,vers=3 nfs://HOSTNAME-OR-IPADDRESS:38467/VOLNAME MOUNTDIR`

例如︰

    ` # mount -o proto=tcp,vers=3 nfs://server1:38467/test-volume /mnt/glusterfs`

<a name="auto-nfs"></a>
# # # 自动挂载卷使用 NFS

您可以配置您的系统自动挂载 Gluster 卷
使用 NFS 每次系统启动时启动。

* * 到自动装入 Gluster 卷使用 NFS * *

-装入卷，编辑 /etc/fstab 文件中，并添加以下内容
行︰

    `HOSTNAME-OR-IPADDRESS:/VOLNAME MOUNTDIR nfs defaults,_netdev,vers=3 0 0`

例如，

    `server1:/test-volume /mnt/glusterfs nfs defaults,_netdev,vers=3 0 0`

> * * 注意 * *
>
> Gluster NFS 服务器不支持 UDP。如果你是 NFS 客户端

> 使用连接使用 UDP，以下消息的默认值
> 出现︰
>
    > `requested NFS version or transport protocol is not supported.`

要使用 TCP 连接

-在 /etc/fstab 文件中添加以下条目︰

    `HOSTNAME-OR-IPADDRESS:/VOLNAME MOUNTDIR nfs defaults,_netdev,mountproto=tcp 0 0`

例如，

    `server1:/test-volume /mnt/glusterfs nfs defaults,_netdev,mountproto=tcp 0 0`

* * 到自动挂载 NFS 装载 * *

Gluster 支持自动挂载 NFS 挂载 \*nix 的标准方法。
更新的 /etc/auto.master 和 /etc/auto.misc 并启动 autofs
服务。在那之后，每当用户或进程尝试访问

它会在后台安装的目录。

# # # 测试使用 NFS 挂载的卷

您可以确认 Gluster 目录越来越多的成功。

* * 到测试安装卷 * *

-使用 mount 命令输入下面的命令︰

    `# mount`

例如，在客户端上的 mount 命令的输出会
显示类似于以下内容的条目︰

    `server1:/test-volume on /mnt/glusterfs type nfs (rw,vers=3,addr=server1)`

-使用 df 命令输入下面的命令︰

    `# df`

例如，在客户端上的 df 命令的输出将显示
从卷中的所有砖块的聚合的存储空间。

# df-h/产妇和新生儿破伤风/glusterfs
使用文件系统大小的效用使用 %安装在
server1: / 测试卷 28T 22T 5.4T 82%/产妇和新生儿破伤风/glusterfs

-更改到的目录并列出其内容通过输入
之后︰

    `# cd MOUNTDIR`
    `# ls`

#CIFS

你可以使用 CIFS 访问到卷使用 Microsoft Windows 作为时
以及 SAMBA 客户端。对于这种访问方法，Samba 软件包需要

目前在客户端上。您可以导出作为 glusterfs 挂载点

桑巴出口，然后挂载它使用 CIFS 协议。

本节描述了如何在 Microsoft 上装载 CIFS 共享
基于 Windows 的客户端 （手动和自动） 和如何
验证已成功装入该卷。

> * * 注意 * *
>
> CIFS 访问使用 Mac OS X Finder 不受支持，但是，你
> 可以使用 Mac OS X 命令行访问 Gluster 卷使用
> CIFS。

# #Using CIFS 装入卷

您可以使用下面的方法来装载 Gluster 卷之一︰

-[导出通过 Samba](#export-samba) Gluster 卷
-[手动装入卷使用 CIFS](#cifs-manual)
-[自动挂载卷使用 CIFS](#cifs-auto)

你也可以使用 Samba 导出 Gluster 卷通过 CIFS
协议。

<a name="export-samba"></a>
# # # 通过 Samba 的出口 Gluster 卷。

我们推荐您使用 Samba 出口通过 Gluster 卷
CIFS 协议。

* * 到出口量通过 CIFS 协议 * *

1.装入 Gluster 卷。

2.安装 Samba 配置导出 Gluster 的挂载点
卷。

例如，如果一个 Gluster 卷装在产妇和新生儿破伤风/gluster，你
必须编辑 smb.conf 文件，以便通过 CIFS 出口这。开放

smb.conf 文件在编辑器中，添加一个简单的以下行
配置︰

[] glustertest

评论 = 测试 Gluster 卷通过 CIFS 出口

路径 = / 产妇和新生儿破伤风/glusterfs

阅读只 = 否

客人好 = 是

保存更改并开始使用您系统初始化的 smb 服务
脚本 (/etc/init.d/smb [是] 开始)。

> * * 注意 * *
>
> 到能装载从任何受信任的存储池中的服务器，你必须
> Gluster 的每个节点上重复这些步骤。实现更高级的

> 配置，请参阅 Samba 文档。

<a name="cifs-manual"></a>
# # # 手动装入卷使用 CIFS

您可以手动装入 Gluster 卷上微软使用 CIFS
基于 Windows 的客户端机器。

* * 到手动装入 Gluster 卷使用 CIFS * *

1.使用 Windows 资源管理器中，选择 * * 工具 \ > 映射网络驱动器......* * 从
菜单。* * 映射网络驱动器 * * 窗口出现。


2.选择驱动器字母使用 * * 驱动器 * * 下拉列表。

3.单击 * * 浏览 * * 中，选择要映射到网络驱动器的卷和
单击 * * 好 * *。

4.单击 * * Finish.* *

（映射到卷） 网络驱动器显示在计算机窗口。

或者，手动装入的去使用 CIFS Gluster 卷
* * 开始 \ > 运行 * * 和手动输入网络路径。

<a name="cifs-auto"></a>
# # # 自动挂载卷使用 CIFS

您可以配置您的系统自动挂载 Gluster 卷
使用 CIFS 微软基于 Windows 的客户端在每次系统
开始。

* * 到自动装入 Gluster 卷使用 CIFS * *

（映射到卷） 网络驱动器显示在计算机窗口
和每次系统启动的时重新连接。

1.使用 Windows 资源管理器中，选择 * * 工具 \ > 映射网络驱动器......* * 从
菜单。* * 映射网络驱动器 * * 窗口出现。


2.选择驱动器字母使用 * * 驱动器 * * 下拉列表。

3.单击 * * 浏览 * * 中，选择要映射到网络驱动器的卷和
单击 * * 好 * *。

4.单击 * * 重新连接 * * 在登录复选框。

5.单击 * * Finish.* *

# # # 测试卷使用 CIFS 装载

您可以确认 Gluster 目录由成功安装
导航到 Windows 资源管理器的目录。

[]: http://bits.gluster.com/gluster/glusterfs/3.3.0qa30/x86_64/
[1]: http://www.gluster.org/download/
[2]: http://download.gluster.com/pub/gluster/glusterfs
