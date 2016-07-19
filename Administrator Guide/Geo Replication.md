#Managing 土力工程处复制

土力工程处复制提供连续、 异步和增量
到另一个通过局域网从一个站点复制服务
(Lan)、 广域网 (Wan)，和整个互联网。

土力工程处复制使用主-从模式，藉以复制和
镜像会发生以下的伙伴之间︰

-* * 主 * * — — GlusterFS 卷

-* * 奴隶 * * — — 奴隶可以是以下类型︰

-本地的目录，可以表示为文件 URL 像
        `file:///path/to/dir`. You can use shortened form, for example,
        ` /path/to/dir`.

-GlusterFS 卷-奴隶卷可以是任何一个地方的卷
        like `gluster://localhost:volname` (shortened form - `:volname`)
或由不同的主机，像卷
        `gluster://host:volname` (shortened form - `host:volname`).

> * * 注意 * *
>
> 可以远程使用 SSH 隧道访问两个以上的类型。
> 到使用 SSH，SSH 向添加前缀或文件的 URL 或 gluster 类型
    > URL. For example, ` ssh://root@remote-host:/path/to/dir`
    > (shortened form - `root@remote-host:/path/to/dir`) or
    > `ssh://root@remote-host:gluster://localhost:volname` (shortened
    > from - `root@remote-host::volname`).

本节介绍了土力工程处复制，阐述了各种
部署方案，并说明了如何配置到系统
提供复制和镜像您的环境中。

##Replicated 卷 vs 土力工程处复制

下表列出了复制的卷之间的区别和
土力工程处-复制︰

复制卷 |土力工程处复制

--- |---

镜像数据跨集群 |镜像数据跨地理位置分散的群集

提供高可用性 |确保支持了用于灾难恢复的数据

同步复制 （每个文件操作发送跨所有的砖块） |异步复制 （定期检查文件中的更改并同步他们检测差异）


# #Preparing 部署土力工程处复制

本节概述了土力工程处复制部署
方案中，描述了如何你可以检查的最低系统要求
并探讨了常见部署方案。

##Exploring 土力工程处复制部署方案

土力工程处复制提供增量复制服务在本地
局域网 (Lan)、 广域网 (Wan)，并在互联网上。
本部分阐述了最常见部署方案
土力工程处-复制，包括以下内容︰

-通过局域网的土力工程处复制
-上湾的土力工程处复制
-在互联网上的土力工程处复制
-多站点级联的土力工程处复制

* * 通过 LAN * * 土力工程处复制

您可以配置土力工程处复制到镜像数据在本地区域
网络。

![geo-] rep_lan() https://cloud.githubusercontent.com/assets/10970993/7412281/a542e724-ef5e-11e4-8207-9e018c1e9304.png


* * 上湾的土力工程处复制 * *

您可以配置 Geo-复制在较宽的区域复制数据
网络。

![geo-] rep_wan() https://cloud.githubusercontent.com/assets/10970993/7412292/c3816f76-ef5e-11e4-8daa-271f6efa1f58.png


* * 在互联网 * * 土力工程处复制

您可以配置土力工程处复制到镜像数据在互联网上。

![geo-] rep03_internet() https://cloud.githubusercontent.com/assets/10970993/7412305/d8660050-ef5e-11e4-9d1b-54369fb1e43f.png


* * 多站点级联 Geo-复制 * *

您可以配置土力工程处复制到镜像数据级联的方式
跨多个站点。

![geo-] rep04_cascading() https://cloud.githubusercontent.com/assets/10970993/7412320/05e131bc-ef5f-11e4-8580-a4dc592148ff.png


# # Geo-复制部署概述

部署土力工程处复制涉及以下步骤︰

1.确认您的环境符合最低系统要求。
2.确定适当的部署方案。
3.对主从系统，根据需要启动土力工程处复制。

##Checking 土力工程处复制最低要求

在部署之前 GlusterFS 土力工程处复制，请验证您的系统
匹配的最低要求。

下表概述了这两个大师的最低要求
和在您的环境的奴隶节点︰

组件 |Master                                                                |奴隶

---                      |---                                                                   |---

操作系统 |GNU/Linux                                                             |GNU/Linux

文件系统 |3.2 或更高的 GlusterFS |GlusterFS 3.2 或更高 (GlusterFS 需要安装，但不是需要运行)，ext3，ext4，或 XFS （任何其他 POSIX 兼容的文件系统会工作，但尚未经过广泛测试）

Python |Python 2.4 （与 ctypes 外部模块），或者 Python 2.5 （或更高） |Python 2.4 （与 ctypes 外部模块），或者 Python 2.5 （或更高）

安全壳 |OpenSSH 版本 4.0 （或更高） |SSH2 兼容守护进程

远程同步 |rsync 3.0.7 或更高 |rsync 3.0.7 或更高

FUSE                     |GlusterFS 支持版本 |GlusterFS 支持版本


# #Setting 组成的环境地质复制

* * 同步时间 * *

-对砖土力工程处复制主音量，所有的服务器时间
必须是统一的。建议您设置 NTP （网络时间

协议） 服务，使砖同步的时间，避免
--时间同步的效果。

例如︰ 在复制的卷在 brick1 的主人在哪里
处于 12.10 12.20 小时和大师的砖 2 小时 10 分钟
时滞，在 brick2 之间这一时期的所有更改都可能都去
在文件与奴隶同步过程中被忽视。

* * 到 SSH * * 设置土力工程处复制

无密码登录不得不之间主机设置 （在哪里
土力工程处复制启动命令将印发） 和远程计算机
（在那里奴隶过程应该发起通过 SSH）。

1.在土力工程处复制会话在哪里设置的节点上，运行
下面的命令︰

# ssh 凯基-f /var/lib/glusterd/geo-replication/secret.pem

按 enter 键两次，以避免密码短语。

2.下面的命令在主机上运行的所有奴隶主机︰

# ssh-副本-id-i /var/lib/glusterd/geo-replication/secret.pem.pub @

# #Setting 组成的环境安全的土力工程处复制的奴隶

您可以配置一个安全的奴隶使用 SSH，所以授予硕士学位
受限制的访问。使用 GlusterFS，您不需要指定配置

关于奴隶的主人端配置参数。为

示例，主人不需要 rsync 程序的位置
在奴隶，而奴隶必须确保，rsync 是在用户的路径
其中大师使用 SSH 连接。唯一的信息主

和有洽谈的奴隶是奴隶端用户帐户，奴隶
掌握使用作为奴隶的资源和主人的资源的公共
关键。可以使用下面的代码建立对奴隶的安全访问

选项：

-限制远程执行命令

-   Using `Mountbroker` for Slaves

-使用 IP 基于访问控制

* * 向后兼容性 * *

您现有的土力工程处复制环境将工作与 GlusterFS，
除下列︰

-安全重构过程影响只有 glusterfs
对奴隶的实例。所做的更改是对大师与透明

您可能需要更改 SSH 的异常靶向
对奴隶的非特权的帐户。

-以下是一些例外这可能无效︰

-土力工程处复制 Url，指定奴隶资源时
配置主机将包括以下特别
字符︰ 空间，\ *，？，[;

-奴隶必须正在运行的实例的 glusterd，即使有
没有 gluster 卷装入的奴隶资源之间 （即，
文件树奴隶都只使用）。

# # # 限制远程执行命令

如果你限制远程执行命令，然后奴隶审核命令
来自大师和命令有关给定
土力工程处复制会话被允许。奴隶还提供访问只

奴隶资源内的文件，可以读取或操作
由船长。

若要限制远程执行命令︰

1.标识上奴隶的 gsyncd 助手实用程序的位置。这

    utility is installed in `PREFIX/libexec/glusterfs/gsyncd`, where
前缀是 glusterfs 编译时间参数。例如，

    `--prefix=PREFIX` to the configure script with the following common
    values` /usr, /usr/local, and /opt/glusterfs/glusterfs_version`.

2.确保调用从主人的命令，奴隶通过
奴隶的 gsyncd 实用程序。

您可以使用以下两个选项之一︰

-设置为绝对路径，gsyncd 作为帐户壳
而大师通过 SSH 连接。如果您需要使用

特权的帐户，然后通过设置它创建新用户
UID 为 0。

-安装程序密钥身份验证与命令执法改为 gsyncd。你

必须前缀中奴隶的主人的公钥的副本
        account's `authorized_keys` file with the following command:

        `command=<path to gsyncd>`.

例如，
        `command="PREFIX/glusterfs/gsyncd" ssh-rsa AAAAB3Nza....`

# # # 使用 Mountbroker 为奴隶

`mountbroker` is a new service of glusterd. This service allows an
非特权的进程拥有 GlusterFS 装载通过注册一个标签
（和 DSL （领域特定语言） 选项） 与通过 glusterd
glusterd volfile。使用 CLI，您可以发送到 glusterd 到装入请求

收到的已装入卷的别名 （符号链接）。

从代理的请求，对弱势群体的奴隶代理使用
mountbroker 服务的 glusterd 设置为辅助 gluster 山
在这个特殊的环境，以确保代理只是代理
允许访问提供行政的特殊参数以
对特定卷的级别访问。

* * 到安装辅助 gluster 山为代理 * *:

1.  In all Slave nodes, create a new group. For example, `geogroup`.

2.  In all Slave nodes, create a unprivileged account. For example, ` geoaccount`. Make it a
    member of ` geogroup`.

3.在奴隶的所有节点，创建新的目录拥有由 root 用户与权限 * 0711.*
例如，创建一个创建的 mountbroker 根目录
    `/var/mountbroker-root`.

4.在任何一个奴隶节点，运行以下命令以将选项添加到 glusterd 卷
file(`/etc/glusterfs/glusterd.vol`)
    in rpm installations and `/usr/local/etc/glusterfs/glusterd.vol` in Source installation.

    ```sh
gluster 系统︰ 执行 mountbroker 选择 mountbroker 根 /var/mountbroker-root
gluster 系统︰ 执行 mountbroker 选择土力工程处复制日志组 geogroup
gluster 系统︰ 执行 mountbroker 选择 rpc-身份验证-允许-不安全上
    ```

5.在任何一个奴隶节点，将 Mountbroker 用户添加到 glusterd vol 文件使用，

    ```sh
gluster 系统︰ 执行 mountbroker 用户 geoaccount slavevol
    ```

其中 slavevol 是奴隶卷名称

如果您承载多个奴隶卷上的奴隶，为每个并添加下列选项
对 volfile 的使用，

    ```sh
gluster 系统︰ 执行 mountbroker 用户 geoaccount2 slavevol2
gluster 系统︰ 执行 mountbroker 用户 geoaccount3 slavevol3
    ```

若要添加多个卷，每个 mountbroker 用户，

    ```sh
gluster 系统︰ 执行 mountbroker 用户 geoaccount1 slavevol11、 slavevol12、 slavevol13
gluster 系统︰ 执行 mountbroker 用户 geoaccount2 slavevol21，slavevol22
gluster 系统︰ 执行 mountbroker 用户 geoaccount3 slavevol31
    ```
6.  Restart `glusterd` service on all Slave nodes.

7.安装 passwdless 从主节点之一 SSH 用户对奴隶节点之一。
例如，到 geoaccount。

8.通过运行创建主设备和从设备到用户的土力工程处复制关系
在主节点上的以下命令︰

    ```sh
gluster 卷土力工程处-复制 <master_volume> <mountbroker_user>@<slave_host>::<slave_volume>
创建推送 pem [力]
    ```

9.  In the slavenode, which is used to create relationship, run `/usr/libexec/glusterfs/set_geo_rep_pem_keys.sh`
作为根用户名称、 主音量的名字，与奴隶卷的名称作为参数。

    ```sh
/usr/libexec/glusterfs/set_geo_rep_pem_keys.sh <mountbroker_user> <master_volume> <slave_volume>
    ```

# # # 使用 IP 基于访问控制

您可以使用基于 IP 的访问控制方法提供的访问控制
使用 IP 地址的奴隶资源。你可以为这两个奴隶使用方法

和文件树的奴隶，但在部分中，我们正集中文件树
使用此方法的奴隶。

设置访问控制基于 IP 地址的文件树奴隶︰

1.设置辅助功能的文件树资源一般限制︰

# gluster 卷土力工程处复制 ' / *' 配置允许网络:: 1,127.0.0.1

这会拒绝产卵主从代理除外的所有请求
本地启动的请求。

2.  If you want the to lease file tree at `/data/slave-tree` to Master,
输入以下命令︰

# gluster 卷土力工程处 replicationconfig 允许-网络

    `MasterIP` is the IP address of Master. The slave agent spawn
从主服务器请求将被接受，如果它在执行
    `/data/slave-tree`.

如果主侧网络配置不会启用的奴隶
认识到大师的确切 IP 地址，您可以使用 CIDR 表示法对
指定一个子网，而不是单个 IP 地址为 MasterIP 或甚至
CIDR 子网的以逗号分隔的列表。

如果您想要扩展到 gluster 奴隶的基于 IP 的访问控制，请使用
下面的命令︰

# gluster 卷土力工程处复制 ' *' 配置允许网络:: 1,127.0.0.1

##Starting 土力工程处复制

本节描述如何配置和启动 Gluster
土力工程处复制在您的存储环境，并验证它是否
正常工作。

# # #Starting 土力工程处复制

要启动 Gluster 土力工程处复制

-使用下面的命令，开始在主机之间的土力工程处复制︰

# gluster 卷土力工程处复制开始

例如︰

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 开始
启动土力工程处复制会话之间 Volume1
example.com:/data/remote_dir 已成功

> * * 注意 * *
>
> 您可能需要将服务配置为在启动 Gluster 之前
> 土力工程处复制。

# # #Verifying 成功部署

你可以使用 gluster 命令验证 Gluster 的状态
土力工程处复制您的环境中。

* * 到验证状态 Gluster Geo-复制 * *

-通过在主机上执行以下命令验证状态︰

# gluster 卷土力工程处复制状态

例如︰

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 状态
# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 状态
主人的奴隶状态
______    ______________________________   ____________
Volume1 root@example.com: / 数据/remote_dir 开始......

# # #Displaying 土力工程处复制状态信息

您可以显示有关特定的土力工程处复制的状态信息
主会话，或特定的主人-奴隶会话，或所有
土力工程处复制会话，根据需要。

* * 到显示土力工程处复制状态信息 * *

-使用下面的命令来显示信息的所有土力工程处复制会话︰

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 状态

-使用以下命令来显示信息的一个特定的主从会话︰

# gluster 卷土力工程处复制状态

例如，要显示信息的 Volume1 和
example.com:/data/remote\_dir

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 状态

Geo-之间的复制 Volume1 状态和
example.com:/data/remote\_dir 被显示。

-显示所有属于的土力工程处复制会话信息
硕士

# gluster 卷土力工程处复制掌握状况

例如，要显示信息的 Volume1

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 状态

会话的状态可能是下列之一︰

-* * 初始化 * *: 这是土力工程处复制会话; 的初始阶段
它保持此状态一分钟以确保没有异常情况都存在。

-* * 不启动 * *: 土力工程处复制会话是创建，但尚未开始。

-* * 活动 * *: 此节点中的 gsync 守护进程处于活动状态和同步数据。

-* * 被动 * *: 一副本对主动节点。数据同步处理的主动节点。

因此，此节点不能同步的任何数据。

-* * * * 故障︰ 土力工程处复制会话经历了一个问题，并且问题需要
进一步的调查。

-* * 停止 * *: 土力工程处复制会话已停止，但未被删除。

爬网状态可以是以下值之一︰

-* * 更新日志爬网 * *: 更新日志翻译产生了更新日志，那被消耗
由 gsyncd 守护进程同步数据。

-* * * * 混合爬网︰ gsyncd 守护进程是爬行 glusterFS 文件系统和产生伪
要同步的数据的更新日志。

-* * 检查点地位 * *: 显示检查点的状态，如果设置。否则，它将显示为 n/A。


##Configuring 土力工程处复制

若要配置 Gluster 土力工程处复制

-在 Gluster 命令行中使用以下命令︰

# gluster 卷土力工程处复制配置 [选项]

例如︰

使用以下命令来查看所有选项/值对的列表︰

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 配置

# # #Configurable 选项

下表概述了土力工程处复制设置的可配置选项︰

Option                        |描述

---                           |---

gluster 日志文件日志文件 |土力工程处复制 glusterfs 日志文件的路径。

gluster 日志级别 LOGFILELEVEL |Glusterfs 进程日志级别。

日志文件日志文件 |土力工程处复制日志文件的路径。

日志级别 LOGFILELEVEL |土力工程处复制日志级别。

ssh 命令命令 |SSH 命令以连接到远程计算机 （默认值是 SSH）。

rsync 命令命令 |Rsync 命令用于同步文件 （默认为 rsync）。

使用 tarssh true |使用 tarssh 命令通过安全 Shell 协议允许焦油。使用此选项来处理工作负载的未经编辑的文件。

volume_id = UID |要删除现有的主机 UID 为中间体/奴隶节点的命令。

超时秒数 |以秒为单位的超时时间。

同步作业 N |同时可以同步的文件/目录的数目。

忽略删除 |如果此选项设置为 1，在母版上删除的文件不会触发对奴隶的删除操作。结果，奴隶将仍然是主人的一个超集，并可以用来恢复大师在崩溃和/或意外删除。

检查点 [标签 | 现在] |设置检查点与给定的选项标签。如果像现在一样，那么当前时间，则设置此选项将用作标签。


##Stopping 土力工程处复制

你可以使用 gluster 命令停止 Gluster 土力工程处复制 （同步
数据从主人到奴隶） 在您的环境。

* * 到停止 Gluster Geo-复制 * *

-使用下面的命令来停止在主机之间的土力工程处复制︰

# gluster 卷土力工程处复制停止

例如︰

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 停止
停止土力工程处复制会话之间 Volume1 和
example.com:/data/remote_dir 已成功

##Restoring 数据从奴隶

您可以从主音量，奴隶还原数据时
主音量变得像硬件故障原因故障。

本节中的示例假定您使用的主音量
(Volume1) 具有以下配置︰

machine1 # gluster 卷信息
类型︰ 分发
状态︰ 开始
砖的数目︰ 2
传输类型︰ tcp
砖头︰
Brick1: machine1: / 出口/dir16
Brick2︰ 机 2: / 出口/dir16
重新配置的选项︰
geo-replication.indexing︰ 上

数据从主音量 (Volume1) 以奴隶目录同步
(example.com:/data/remote\_dir)。若要查看的状态

土力工程处复制会话在主机上运行以下命令︰

# gluster 卷土力工程处复制 Volume1 root@example.com: / 数据/remote_dir 状态

* * 前失败 * *

假设主卷 100 档案，安装在
产妇和新生儿破伤风/gluster 对客户端计算机 （客户端） 之一。运行以下命令

客户端机器上的命令以查看的文件的列表︰

客户端 # ls/产妇和新生儿破伤风/gluster |wc – l

100

奴隶目录 （链接） 会有相同的数据，如大师
可以通过在奴隶上运行下面的命令查看卷和相同︰

example.com# ls/数据/remote_dir / |wc – l

100

* * 后失败 * *

如果一个砖头 (机 2) 失败了，然后的现状
土力工程处复制会话转为"故障"的"确定"。若要查看

这土力工程处复制会话的状态上运行下面的命令
师父︰

# gluster 卷土力工程处复制 Volume1 root@example.com: / 数据/remote_dir 状态

机 2 失败，现在你可以看到差异中的文件数
主人与奴隶。几个文件将会丢失从主服务器

卷，但他们将可只在奴隶，如下所示。

在客户端上运行以下命令︰

客户端 # ls/产妇和新生儿破伤风/gluster |wc – l

52

在奴隶 (example.com) 上运行以下命令︰

Example.com# # ls/数据/remote_dir / |wc – l

100

* * 到从奴隶机 * * 还原数据

1.使用以下命令来停止所有硕士土力工程处复制会话︰

# gluster 卷土力工程处复制停止

例如︰

machine1 # gluster 卷土力工程处复制 Volume1
example.com:/data/remote_dir 站

停止土力工程处复制会话之间 Volume1 &
example.com:/data/remote_dir 已成功

> * * 注意 * *
>
    > Repeat `# gluster volume geo-replication  stop `command on all
> 活动的土力工程处复制会话的主音量。

2.使用以下命令来替换故障砖的主人︰

# gluster 卷替换砖犯力

例如︰

machine1 # gluster 卷替换砖 Volume1 机 2: / 出口/dir16 machine3: / 出口/dir16 提交力
替换砖提交成功

3.使用以下命令来验证迁移的砖通过查看卷信息︰

# gluster 卷信息

例如︰

machine1 # gluster 卷信息
卷名︰ Volume1
类型︰ 分发
状态︰ 开始
砖的数目︰ 2
传输类型︰ tcp
砖头︰
Brick1: machine1: / 出口/dir16
Brick2: machine3: dir16/出口 /
重新配置的选项︰
geo-replication.indexing︰ 上

4.运行 rsync 命令手动同步数据从奴隶对主人
卷的客户端 （挂载点）。

例如︰

example.com# rsync — — PavhS — — xattrs — — 忽略现有客户端/数据/remote_dir /: / 产妇和新生儿破伤风/gluster

验证数据同步通过使用以下命令︰

上主音量，请运行以下命令︰

客户端 # ls |wc – l

100

在奴隶上运行以下命令︰

example.com# ls/数据/remote_dir / |wc – l

100

现在掌握体积和奴隶同步目录。

5.使用以下命令重新启动从主人，奴隶的土力工程处复制会话︰

# gluster 卷土力工程处复制开始

例如︰

machine1 # gluster 卷土力工程处复制 Volume1
example.com:/data/remote_dir 开始
启动土力工程处复制会话之间 Volume1 &
example.com:/data/remote_dir 已成功

##Best 做法

* * 手动设置时间 * *

如果您要手动更改您的砖上的时间，然后你必须
在所有砖块上设置统一的时间。设置时间落后腐化

土力工程处复制指数，所以手动设置时间的建议的方法是︰

1.停止土力工程处复制之间的主人和奴隶使用
下面的命令︰

# gluster 卷土力工程处复制停止

2.停止土力工程处复制索引使用下面的命令︰

# gluster 音量设置的 geo-replication.indexing

3.在所有砖块上设置统一的时间。

4.使用以下命令来重新启动你的土力工程处复制会话︰

# gluster 卷土力工程处复制开始

* * 在一个系统 * * 运行土力工程处复制命令

最好在砖之一中运行土力工程处复制命令
在受信任的存储池。这是因为日志文件为

土力工程处复制会话将存储在 \*Server\* 中在哪里
土力工程处复制开始启动。因此它会更容易地找到

日志-文件时所需。

* * 隔离 * *

土力工程处复制奴隶操作沙盒现在并不跑作为
特权的服务。因此，出于安全的原因，它建议

创建一个沙箱环境 （专用机 / 专用虚拟
机器 / chroot/容器类型解决方案) 由管理员运行
土力工程处复制奴隶在它。加强在这方面会

在后续的次要版本中可用。
