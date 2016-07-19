#Managing GlusterFS 卷

本节介绍如何执行常见的 GlusterFS 管理
操作，包括以下内容︰

-[调整卷 Options](#tuning-options)
-[为 Volume](#configuring-transport-types-for-a-volume) 配置的运输类型
-[扩大 Volumes](#expanding-volumes)
-[收缩 Volumes](#shrinking-volumes)
-[替换 Bricks](#replace-brick)
-[迁移 Volumes](#migrating-volumes)
-[平衡 Volumes](#rebalancing-volumes)
-[停止 Volumes](#stopping-volumes)
-[删除 Volumes](#deleting-volumes)
-[触发对 Replicate](#triggering-self-heal-on-replicate) 的自愈
-[非均匀文件 Allocation(NUFA)](#non-uniform-file-allocation)

<a name="tuning-options"></a>
##Tuning 卷选项

您可以调整音量选项，根据需要虽然群集是在线和
可用。

> * * 注意 * *
>
> 这建议，你将 server.allow 不安全选项设置为 ON，如果
> 每个卷中有太多砖块或如果有太多
> 已经利用中的所有特权的端口的服务
> 系统。在打开此选项允许端口，以便接受拒绝消息

> 从不安全的端口。因此，使用此选项仅当您的部署

> 需要它。

调整音量选项使用下面的命令︰

# gluster 卷集

例如，要指定测试卷的性能高速缓存大小︰

# gluster 音量设置测试卷 performance.cache 大小 256 MB
集的卷成功

下表列出了卷选项随其
说明和默认值︰

> * * 注意 * *
>
> 这里给出的默认选项是随时会有变动
> 给予时间和可能不是相同的所有版本。

选项 |描述 |默认值 |可用的选项

--- |--- |--- |---

auth.allow |IP 地址的客户端应该被允许访问该卷。|\ * （允许所有） |有效的 IP 地址，其中包括通配符模式包括 \ *，如 192.168.1.\*

auth.reject |IP 地址的客户端应该拒绝访问该卷。|无 （拒绝无） |有效的 IP 地址，其中包括通配符模式包括 \ *，如 192.168.2.\*

client.grace 超时 |指定网络断开后在客户端维护锁状态的持续时间。|10 |10-1800 秒

cluster.self 治愈窗口大小 |指定每个文件同时发生自愈将块的最大数目。|16 |0-1025年块

cluster.data 自我治愈算法 |指定的类型的自愈。如果你设置为"已满"的选项，将整个文件从复制源到目的地。如果该选项设置为"diff"并不是同步的文件块复制到目的地。重置使用启发式模型。如果上的 subvolumes，一个不存在的文件或一个零字节文件存在 （创建的条目自愈） 的整个内容有无论如何，复制从使用"diff"算法没有任何好处。如果文件大小是关于页大小相同，整个文件是可以读取和写入几个操作，将会更快比"diff"已读校验和，然后读取和写入的。|重置 |全/diff/重置

cluster.min 免费磁盘 |指定必须保持自由的磁盘空间的百分比。可能用于非均匀砖 |10 %|所需的最小可用磁盘空间的百分比

cluster.stripe 块大小 |指定的大小，将读取或写入的条纹单位。|（对于所有的文件） 的 128 KB |大小 （字节）

cluster.self 治愈守护程序 |允许你关断主动自疗上复制 |关于 |打开关闭

cluster.ensure 耐久性 |此选项可确保数据/元数据是持久跨突然关机的砖。|关于 |打开关闭

diagnostics.brick 级日志 |更改日志级别的砖。|信息 |调试/警告/错误/关键/没有/跟踪

diagnostics.client 级日志 | 更改日志级别的客户端。|信息 |调试/警告/错误/关键/没有/跟踪

diagnostics.latency 测量 |将跟踪对每个操作的延迟与相关统计信息。|关闭 |打开关闭

diagnostics.dump-fd-统计 |将跟踪有关文件操作的统计。|关闭 |关于

只有 features.read |使您能够将整个卷以只读模式装载为所有的客户端 （包括 NFS 客户端） 访问它。|关闭 |打开关闭

features.lock 治愈 |当网络断开连接时可以自愈的锁。|关于 |打开关闭

features.quota 超时 |出于性能原因，配额将缓存在客户端上的目录大小。您可以设置超时指示在缓存中的目录大小最长持续时间，从时间来填充，在此期间，他们被视为有效 |0 | 0-3600 秒

geo-replication.indexing |使用此选项可以自动同步到奴隶从主文件系统的变化。|关闭 |打开关闭

network.frame 超时 |时限之后操作有要声明为死了，如果服务器没有响应特定操作。|1800 （30 分钟） |1800 秒

network.ping 超时 |为其客户端等待检查服务器是否反应的持续时间。当 ping 超时发生时，还有网络断开客户端和服务器之间的连接。由服务器代表客户端持有的所有资源去都梳洗一下。重新连接的时候，所有的资源将需要重新获得之前客户端可以恢复其服务器上的操作。此外，将获取的锁和锁表更新。此重新连接是非常昂贵的操作，应避免。|42 秒 |42 秒

nfs.enable ino32 |对于 32 位 nfs 客户端或应用程序不支持 64 位 inode 号码或大的文件，请使用此选项从 CLI 使 Gluster NFS 返回 32 位 inode 号码而不是 64 位 inode 号码。|关闭 |打开关闭

nfs.volume 访问 |设置指定的子卷的访问类型。|读写 | 读写/只读

nfs.trusted 写 |如果从客户端写的不稳定，会返回稳定标志强制客户端不发送一个提交请求。在某些环境中，结合一个复制的 GlusterFS 设置，此选项可以提高写入性能。此标志允许用户同步数据到磁盘的信任 Gluster 复制逻辑和恢复时所需。如果收到的提交请求将由 fsyncing 处理采用默认方式。稳定的写入仍然以同步的方式处理。|关闭 |打开关闭

nfs.trusted 同步 |所有的写操作和提交请求被视为异步。这意味着保证没有写入请求时写答复在 NFS 客户端要在服务器的磁盘上。受信任的同步包括信任写行为。|关闭 |打开关闭

nfs.export-迪尔 |此选项可以用于导出指定以逗号分隔子目录卷中。路径必须是绝对路径。路径允许列表中的 ip 地址/主机名可以与每个子目录。如果提供的连接将允许仅从这些 IPs。格式: \ <dir\> [(hostspec[hostspec...])][,...].在哪里 hostspec 可以是 IP 地址、 主机名或 IP 范围在 CIDR 表示法。* * 注意 * *: 必须小心配置此选项作为无效条目时和/或无法到达 DNS 服务器可以采用不需要的延迟装载的所有调用。|没有子目录导出。|绝对路径以允许列表中的 IP/主机名

nfs.export 卷 |启用/禁用导出整个卷，相反如果 nfs3.export dir 配合使用，可以允许作为出口只有子目录的设置。|关于 |打开关闭

nfs.rpc-身份验证-unix |启用/禁用 AUTH\_UNIX 身份验证类型。此选项是默认启用的更好的互操作性。然而，如果需要你可以禁用它。|关于 |打开关闭

nfs.rpc auth 空 |启用/禁用 AUTH\_NULL 身份验证类型。它不被建议更改此选项的默认值。|关于 |打开关闭

nfs.rpc-身份验证-allow\ < Addresses\ IP-> |允许地址和/或主机名来连接到服务器的逗号分隔的列表。默认情况下，所有客户端都是不允许的。这允许您定义导出的所有卷的一般规则。|拒绝所有 |IP 地址或主机名

nfs.rpc-身份验证-reject\ < Addresses\ IP-> |拒绝以逗号分隔列表的地址和/或从连接到服务器的主机名。默认情况下，所有连接都是不都允许的。这允许您定义导出的所有卷的一般规则。|拒绝所有 |IP 地址或主机名

nfs.ports 不安全 |允许客户端从非特权端口连接。默认情况下允许只有特权的端口。这是全局设置，以防不安全端口要启用所有出口使用一个单独的选项。|关闭 |打开关闭

nfs.addr namelookup |关断名称查找为传入客户端连接，使用此选项的。在一些系统中，名称服务器可以太长，无法回复装入请求的超时导致的 DNS 查询。使用此选项可以在地址身份验证期间关闭名称查找。请注意，关闭这将阻止您在 rpc-auth.addr.\* 过滤器中使用主机名。|关于 |打开关闭

nfs.register 与 portmap |对于需要运行多个 NFS 服务器的系统，您需要防止多个从注册端口映射服务。使用此选项可以关闭端口映射注册 Gluster nfs。|关于 |打开关闭

nfs.port \<PORT-NUMBER\ > |使用此选项需要 Gluster NFS 要与一个非默认端口号相关联的系统上。|NA |38465-38467

nfs.disable |关断卷正在通过 NFS 导出 |关闭 |打开关闭

performance.write 隐藏窗口大小 |每个文件后写缓冲区的大小。|1 MB |后写式高速缓存大小

performance.io 线程计数 |IO 线程翻译中的线程数。|16 |0-65

performance.flush-隐藏 |如果此选项设置为 ON，则指示后写翻译通过向应用程序返回成功 （或任何的错误，如果任何以前写入失败），冲洗被发送到后端文件系统之前，甚至在背景下，冲洗执行。|关于 |打开关闭

performance.cache 最大文件大小 |设置由 io 缓存译者缓存的最大文件大小。可以使用正常大小的描述符的 KB、 MB、 GB、 TB 或铅 (例如，6 GB)。最大大小 uint64。|2 \ ^64-1 字节 |大小 （字节）

performance.cache 最小文件大小 |设置由 io 缓存译者缓存的最小文件大小。作为"最大"上述值相同 |0B |大小 （字节）

performance.cache 刷新超时 |文件的缓存的数据将被保留到缓存刷新超时 ' 秒之后, 执行数据重新验证。|1s |0 61

performance.cache 大小 |读缓存的大小。|32 MB | 大小 （字节）

server.allow 不安全 |允许客户端从非特权端口连接。默认情况下允许只有特权的端口。这是全局设置，以防不安全端口要启用所有出口使用一个单独的选项。|关于 |打开关闭

server.grace 超时 |指定要在网络连接断开后加以保留在服务器上的锁状态的持续时间。|10 |10-1800 秒

server.statedump 路径 |状态转储文件的位置。|砖的 tmp 目录 | 新的目录路径

storage.health 检查间隔 |健康检查用于 brick(s) 的文件系统上做之间的秒数。默认值为 30 秒，设置为 0 来禁用。|砖的 tmp 目录 | 新的目录路径


您可以查看已更改的卷选项使用命令︰

# gluster 卷信息

<a name="configuring-transport-types-for-a-volume"></a>
##Configuring 运输类型的卷

卷可以支持一个或多个运输类型为客户和砖进程之间的通信。
有三种类型的支持的传输，tcp，rdma，和 tcp，rdma。

若要更改支持的传输类型的卷，请按照步骤︰

1.所有在客户端上使用下面的命令卸载卷︰

# umount 挂载点

2.停止卷使用下面的命令︰

# gluster 卷站 volname

3.改变的传输类型。例如，若要启用 tcp 和 rdma 执行宣读命令︰


# gluster 音量设置 volname config.transport tcp，rdma 或 tcp 或 rdma

4.装入卷上的所有客户端。例如，要使用 rdma 运输的装载，请使用以下命令︰


# 山-t glusterfs-o 运输 = rdma server1: / 测试卷/产妇和新生儿破伤风/glusterfs

<a name="expanding-volumes"></a>
##Expanding 卷

您可以展开卷，必要时群集是在线和
可用。例如，你可能想要对分布式加上一块砖

体积，从而提高分布和添加的能力
GlusterFS 卷。

同样，您可能希望将砖组添加到分布式
复制的卷，增加 GlusterFS 卷的容量。

> * * 注意 * *
>
> 当扩展分布式复制和分发带区的卷
> 您需要添加大量的砖是副本的倍数
> 或条纹计数。例如，扩展分布式复制

> 2 复制副本计数卷，您需要添加砖的倍数
> 2 的 (4、 6、 8，等.)。

* * 对扩展卷 * *

1.在群集中第一个服务器上，探测到的服务器你
要添加新的砖使用下面的命令︰

    `# gluster peer probe `

例如︰

# gluster 同行探头 server4
探头成功

2.加一块砖，使用以下命令︰

    `# gluster volume add-brick `

例如︰

# gluster 卷添加砖测试卷 server4: / exp4
加一块砖成功

3.检查的卷信息，使用以下命令︰

    `# gluster volume info `

该命令将显示类似于以下内容的信息︰

卷名︰ 测试卷
类型︰ 分发
状态︰ 开始
砖的数目︰ 4
砖头︰
Brick1: server1: / exp1
Brick2: server2: / exp2
Brick3: server3: / exp3
Brick4: server4: / exp4

4.平衡要确保所有文件都分发到的卷
新砖。

[再平衡 Volumes](#rebalancing-volumes) 中所述，可以使用再平衡命令

<a name="shrinking-volumes"></a>
##Shrinking 卷

根据需要群集联机时，您可以缩小卷，和
可用。例如，您可能需要删除已成为一块砖

在一个分布式的卷，由于硬件或网络发生故障无法访问。

> * * 注意 * *
>
> 驻留在您要删除的砖上的数据将不再
> 访问在 Gluster 挂载点。然而，只有注意

> 配置信息将被删除，您可以继续访问
> 直接从砖，作为必要的数据。

当收缩分布式复制和分发带区卷，
您需要删除的砖数是副本的倍数
或条纹计数。例如，若要收缩分布式的带区的卷

2 条带数，你需要删除砖在 2 的倍数
(4、 6、 8，等.)。此外，砖您试着

删除必须从同一子卷 （同一副本或条纹
设置）。

* * 到收缩卷 * *

1.删除砖使用下面的命令︰

    `# gluster volume remove-brick ` `start`

例如，若要删除 server2: / exp2:

# gluster 卷删除砖测试卷 server2: / exp2 力

删除 brick(s) 可以导致数据丢失。你想继续吗？(y /) n


2.输入"y"确认该操作。此命令将显示

以下消息指示删除砖操作
成功启动︰

删除砖成功

3.（可选） 查看删除砖操作使用的状态
下面的命令︰

    `# gluster volume remove-brick  status`

例如，若要查看删除砖操作的状态
server2: / exp2 砖︰

# gluster 卷删除砖测试卷 server2: / exp2 状态
节点 Rebalanced 文件体积扫描状态
---------  ----------------  ----  -------  -----------
617c923e-6450-4065-8e33-865e28d9428f 34 340 162 在进步

4.检查的卷信息，使用以下命令︰

    `# gluster volume info `

该命令将显示类似于以下内容的信息︰

# gluster 卷信息
卷名︰ 测试卷
类型︰ 分发
状态︰ 开始
砖的数目︰ 3
砖头︰
Brick1: server1: / exp1
Brick3: server3: / exp3
Brick4: server4: / exp4

5.平衡要确保所有文件都分发到的卷
新砖。

[再平衡 Volumes](#rebalancing-volumes) 中所述，可以使用再平衡命令

<a name="replace-brick"></a>
##Replace 故障砖

* * 更换一块砖 * 纯 * 分发卷 * *

若要替换分配唯一的卷上的砖，用户需要删除使用删除砖的砖。然后添加新砖和平衡群集。


注︰ 更换砖在 gluster 中使用 '替换砖' 命令仅支持分布式复制或 * 纯 * 复制的卷。

删除步骤砖 Server1: / 回家/gfs/r2_1 和添加 Server1: / 回家/gfs/r2_2:

1.这里是初始音量配置︰

卷名︰ r2
类型︰ 分发
卷 ID: 25b4e313-7b36-445d-b524-c3daebb91188
状态︰ 开始
砖的数目︰ 2
传输类型︰ tcp
砖头︰
Brick1: Server1: / 首页/gfs/r2_0
Brick2: Server1: / 首页/gfs/r2_1


2.这里是装载存在的文件︰


⚡ ls
1 10 2 3 4 5 6 7 8 9

3.添加新砖-Server1: / 首页/gfs/r2_2 现在︰

⚡ gluster 卷添加砖 r2 Server1: / 首页/gfs/r2_2
卷添加砖︰ 成功

4.启动删除砖使用下面的命令︰

⚡ gluster 卷删除砖 r2 Server1︰ 家/gfs/r2_1 开始
卷删除砖开始︰ 成功
ID: fba0a488-21a4-42b7-8a41-b27ebaa8e5f4

5.等到完成删除砖。一旦删除砖完成它

显示以下输出︰


⚡ gluster 卷删除砖 r2 Server1: / 首页/gfs/r2_1 状态
节点 Rebalanced 文件体积扫描失败跳过状态运行时间秒
---------      -----------   -----------   -----------   -----------   -----------         ------------     --------------
本地主机 5 20Bytes 15 0 0 完成 0.00


6.现在我们可以安全地删除旧砖，所以提交的更改︰

⚡ gluster 卷删除砖 r2 Server1: / 首页/gfs/r2_1 提交
删除 brick(s) 可以导致数据丢失。你想继续吗？(y/n) y

卷删除砖提交︰ 成功

7.这里是如何配置卷后，删除砖。

卷名︰ r2
类型︰ 分发
卷 ID: 25b4e313-7b36-445d-b524-c3daebb91188
状态︰ 开始
砖的数目︰ 2
传输类型︰ tcp
砖头︰
Brick1: Server1: / 首页/gfs/r2_0
Brick2: Server1: / 首页/gfs/r2_2

8.检查装载的内容︰

⚡ ls
1 10 2 3 4 5 6 7 8 9

* * 取代砖在复制/分布式复制卷 * *

This section of the document contains how brick: `Server1:/home/gfs/r2_0` is replaced with brick: `Server1:/home/gfs/r2_5` in volume `r2` with replica count `2`.

卷名︰ r2
类型︰ 分布式复制
卷 ID: 24a0437a-daa0-4044-8acf-7aa82efd76fd
状态︰ 开始
砖的数目︰ 2 x 2 = 4
传输类型︰ tcp
砖头︰
Brick1: Server1: / 首页/gfs/r2_0
Brick2: Server2: / 首页/gfs/r2_1
Brick3: Server1: / 首页/gfs/r2_2
Brick4: Server2: / 首页/gfs/r2_3


步骤︰

1.确保在 Server1 新砖有没有数据: / 首页/gfs/r2_5
2.检查所有的砖块正在运行。没关系，如果要被替换的砖是下来。

3.使要被替换下来，如果不是已经砖。

-通过执行得到的 pid 砖 ' gluster 卷 <volname>状态的

⚡ gluster 卷状态
卷状态︰ r2
Gluster 进程端口在线 Pid
------------------------------------------------------------------------------
砖 Server1: / 首页/gfs/r2_0 49152 Y 5342
砖 Server2: / 首页/gfs/r2_1 49153 Y 5354
砖 Server1: / 首页/gfs/r2_2 49154 Y 5365
砖 Server2: / 首页/gfs/r2_3 49155 Y 5376

-登录到机砖正在运行，并且杀了砖。

⚡ 杀-15 5342

-确认砖不会再奔跑和其他砖都运行良好。

⚡ gluster 卷状态
卷状态︰ r2
Gluster 进程端口在线 Pid
------------------------------------------------------------------------------
砖 Server1: / 首页/gfs/r2_0 n/A N 5342 < <---砖不运行，其他人都运行良好。
砖 Server2: / 首页/gfs/r2_1 49153 Y 5354
砖 Server1: / 首页/gfs/r2_2 49154 Y 5365
砖 Server2: / 首页/gfs/r2_3 49155 Y 5376

4.  Using the gluster volume fuse mount (In this example: `/mnt/r2`) set up metadata so that data will be synced to new brick (In this case it is from `Server1:/home/gfs/r2_1` to `Server1:/home/gfs/r2_5`)

-创建一个目录尚不存在的装载点上。然后删除该目录，同样为元数据的更新日志做 setfattr。此操作将标记挂起的更新日志，它会告诉自愈达蒙/坐骑要执行的自愈从 /home/gfs/r2_1 到 /home/gfs/r2_5。


mkdir /mnt/r2/<name-of-nonexistent-dir>
rmdir /mnt/r2/<name-of-nonexistent-dir>
setfattr-n trusted.non 存在的关键字-v abc/产妇和新生儿破伤风/r2
setfattr-x trusted.non 存在的关键字/产妇和新生儿破伤风/r2

-有挂起的 xattrs 被取代的砖的复制副本的检查︰

getfattr-d-下午 e 十六进制 /home/gfs/r2_1
# 文件︰ 家/gfs/r2_1
security.selinux=0x756e636f6e66696e65645f753a6f626a6563745f723a66696c655f743a733000
trusted.afr.r2-客户端-0 = 0x000000000000000300000002 < <---xattrs 标记从源砖 Server2: / 首页/gfs/r2_1
trusted.afr.r2-客户端-1 = 0x000000000000000000000000
trusted.gfid=0x00000000000000000000000000000001
trusted.glusterfs.dht=0x0000000100000000000000007ffffffe
trusted.glusterfs.volume id = 0xde822e25ebd049ea83bfaa3c4be2b440

5.容量治愈信息将表明，'/' 需要愈合。（可能有更多的条目基于工作负载。但是 '/' 必须存在)


⚡ gluster 卷治愈 r2 信息
砖 Server1: / 首页/gfs/r2_0
状态︰ 未连接传输端点

砖 Server2: / 首页/gfs/r2_1
/
条目数︰ 1

砖 Server1: / 首页/gfs/r2_2
条目数︰ 0

砖 Server2: / 首页/gfs/r2_3
条目数︰ 0

6.砖替换 '犯力' 选项。请注意，其他变体砖替换命令不支持。


-执行替换砖命令

⚡ gluster 卷替换砖 r2 Server1: / 首页/gfs/r2_0 Server1: / 首页/gfs/r2_5 犯下力
卷替换砖︰ 成功︰ 替换砖提交成功

-检查新砖是现在在线

⚡ gluster 卷状态
卷状态︰ r2
Gluster 进程端口在线 Pid
------------------------------------------------------------------------------
砖 Server1: / 首页/gfs/r2_5 49156 Y 5731 <<<---新砖是在线
砖 Server2: / 首页/gfs/r2_1 49153 Y 5354
砖 Server1: / 首页/gfs/r2_2 49154 Y 5365
砖 Server2: / 首页/gfs/r2_3 49155 Y 5376

-用户可以跟踪进度的自我修复使用:"gluster 卷治愈 <volname>信息"。
自我修复完成后更新历史将被删除。

⚡ getfattr-d-下午 e 十六进制 /home/gfs/r2_1
getfattr︰ 删除前导 '/' 从绝对路径名称
# 文件︰ 家/gfs/r2_1
security.selinux=0x756e636f6e66696e65645f753a6f626a6563745f723a66696c655f743a733000
trusted.afr.r2-客户端-0 = 0x000000000000000000000000 < <----挂起更新历史将被清除。
trusted.afr.r2-客户端-1 = 0x000000000000000000000000
trusted.gfid=0x00000000000000000000000000000001
trusted.glusterfs.dht=0x0000000100000000000000007ffffffe
trusted.glusterfs.volume id = 0xde822e25ebd049ea83bfaa3c4be2b440

-gluster 卷治愈 <volname>信息会显示没有愈合是需要。
⚡ gluster 卷治愈 r2 信息
砖 Server1: / 首页/gfs/r2_5
条目数︰ 0

砖 Server2: / 首页/gfs/r2_1
条目数︰ 0

砖 Server1: / 首页/gfs/r2_2
条目数︰ 0

砖 Server2: / 首页/gfs/r2_3
条目数︰ 0

<a name="rebalancing-volumes"></a>
##Rebalancing 卷

后扩大或收缩卷 (使用添加砖和
删除砖分别命令），你需要重新平衡数据
分配到服务器。扩大或缩小后创建的新目录

卷的均匀分布自动。为所有

可以通过重新平衡固定现有目录，分布
布局和/或数据。

本节描述了如何重新平衡 GlusterFS 卷在你
存储环境中，使用下面的常见情形︰

-* * 修复布局 * *-修复布局更改，以便这些文件实际上可以
转到新添加的节点。

-* * 修复布局和迁移数据 * *-平衡容量由固定布局
变化和迁移现有数据。

# # #Rebalancing 卷以修复布局更改

固定布局是必要的因为布局结构是静态
对于一个给定的目录。在哪里新砖已添加到场景中

现有的卷，在现有目录中新创建的文件将
仍然是只分配给旧砖。的

`# gluster volume rebalance fix-layout start `command will fix the
这样的文件也可以转到新添加节点，布局信息。
当发出此命令，所有的文件统计信息
已缓存将得到重新验证。

截至 GlusterFS 3.6 转让的文件到砖会考虑
砖的大小。 例如，20 TB 砖将分配的两倍

作为 10 TB 砖的许多文件。 在之前 3.6 版本中，两个砖块是

无论规模大小，平等对待，将被分配平等
共享的文件。

固定布局再平衡将只修复布局更改也不会
迁移数据。如果您想迁移现有的数据，

use`# gluster volume rebalance  start ` command to rebalance data among
服务器。

* * 到平衡一个卷来修复布局更改 * *

-开始重新平衡操作上使用的服务器的任何一个
下面的命令︰

    `# gluster volume rebalance fix-layout start`

例如︰

# gluster 卷再平衡测试卷修复布局开始
在测试卷上启动再平衡已成功

# # #Rebalancing 卷修复布局和迁移数据

后扩大或收缩卷 (使用添加砖和
删除砖分别命令），你需要重新平衡数据
分配到服务器。

* * 到平衡卷修复布局和迁移现有数据 * *

-开始重新平衡操作上使用的服务器的任何一个
下面的命令︰

    `# gluster volume rebalance start`

例如︰

# gluster 卷再平衡测试卷开始
开始重新平衡上卷测试卷已成功

-开始有力地在任何一个服务器迁移操作
使用以下命令︰

    `# gluster volume rebalance start force`

例如︰

# gluster 卷再平衡测试卷启动力
开始重新平衡上卷测试卷已成功

# # #Displaying 操作状态的平衡

您可以显示有关平衡卷操作的状态信息
根据需要。

-检查再平衡操作，使用下面的代码的状态
命令︰

    `# gluster volume rebalance  status`

例如︰

# gluster 卷再平衡测试卷的状态
节点 Rebalanced 文件体积扫描状态
---------  ----------------  ----  -------  -----------
617c923e-6450-4065-8e33-865e28d9428f 进展 416 1463年 312

完成重新平衡操作的时间取决于数量
相应文件的大小以及卷上的文件。
继续检查再平衡状态，确认的数量
再平衡或总文件扫描还在不断上升。

例如，再次运行状态命令可能显示结果
类似于以下内容︰

# gluster 卷再平衡测试卷的状态
节点 Rebalanced 文件体积扫描状态
---------  ----------------  ----  -------  -----------
617c923e-6450-4065-8e33-865e28d9428f 进展 498 1783年 378

再平衡状态显示以下时再平衡是
完成︰

# gluster 卷再平衡测试卷的状态
节点 Rebalanced 文件体积扫描状态
---------  ----------------  ----  -------  -----------
617c923e-6450-4065-8e33-865e28d9428f 完成的 502 1873年 334

# # #Stopping 平衡操作

根据需要您可以停止重新平衡操作。

-停止再平衡操作使用下面的命令︰

    `# gluster volume rebalance  stop`

例如︰

# gluster 卷再平衡测试卷停止
节点 Rebalanced 文件体积扫描状态
---------  ----------------  ----  -------  -----------
617c923e-6450-4065-8e33-865e28d9428f 59 590 244 停止
上卷测试卷停止再平衡过程

<a name="stopping-volumes"></a>
##Stopping 卷

1.停止卷使用下面的命令︰

    `# gluster volume stop `

例如，要停止测试卷︰

# gluster 停止测试卷
停止卷将使其数据不可访问。你想要继续吗？(y /) n


2.  Enter `y` to confirm the operation. The output of the command
显示以下内容︰

已成功停止卷测试卷

<a name="deleting-volumes"></a>
##Deleting 卷

1.删除卷使用下面的命令︰

    `# gluster volume delete `

例如，若要删除测试卷︰

# gluster 卷删除测试卷
卷将删除该卷的所有信息。你想要继续吗？(y /) n


2.  Enter `y` to confirm the operation. The command displays the
之后︰

删除卷测试卷已成功

<a name="triggering-self-heal-on-replicate"></a>
对复制的 ##Triggering 自愈

在复制的模块中，以前您必须手动触发自愈
当砖脱机并重新上线，使所有
复制副本同步。现在主动自我修复守护进程运行

背景，诊断问题并自动启动自愈
需要的文件上每隔 10 分钟 * 愈合 *。

您可以查看需要的文件的列表 * 愈合 *，文件的列表
目前以前有哪些 * 愈合 *，列表中的文件
裂的状态，和你可以手动触发对整个的自愈
卷或仅在需要的文件 * 愈合 *。

-触发只在文件上的要求的自愈 * 愈合 *:

    `# gluster volume heal `

例如，要在文件上的触发器自愈，需要 * 愈合 *
测试卷︰

# gluster 卷治愈测试卷
愈合手术卷上测试卷已成功

-触发自愈的卷的所有文件︰

    `# gluster volume heal full`

例如，若要触发的所有文件的自愈
测试卷︰

# gluster 卷治愈测试卷全
愈合手术卷上测试卷已成功

-查看需要的文件的列表 * 愈合 *:

    `# gluster volume heal info`

例如，若要查看所需的测试卷上的文件的列表
* 愈合 *:

# gluster 卷治愈测试卷信息
砖: / gfs/测试-volume_0
条目数︰ 0

砖: / gfs/测试-volume_1
条目数︰ 101
/95.txt
/32.txt
/66.txt
/35.txt
/18.txt
/26.txt
/47.txt
/55.txt
/85.txt
...

-查看的自我疗愈的文件的列表︰

    `# gluster volume heal info healed`

例如，若要查看测试卷上的文件的列表
自我疗愈:

# gluster 卷愈合愈合的测试卷信息
砖: / gfs/测试-volume_0
条目数︰ 0

砖: / gfs/测试-volume_1
条目数︰ 69
/99.txt
/93.txt
/76.txt
/11.txt
/27.txt
/64.txt
/80.txt
/19.txt
/41.txt
/29.txt
/37.txt
/46.txt
...

-查看特定卷的文件列表中的自愈
失败︰

    `# gluster volume heal info failed`

例如，若要查看的不是测试卷的文件列表
自我疗愈:

# gluster 卷治愈失败的测试卷信息
砖: / gfs/测试-volume_0
条目数︰ 0

砖 server2: / gfs/测试-volume_3
条目数︰ 72
/90.txt
/95.txt
/77.txt
/71.txt
/87.txt
/24.txt
...

-查看特定卷中的文件的列表
裂的状态︰

    `# gluster volume heal info split-brain`

例如，若要查看的测试卷中的文件的列表
裂的状态︰

# gluster 卷治愈测试卷信息裂
砖 server1: / gfs/测试-volume_2
条目数︰ 12
/83.txt
/28.txt
/69.txt
...

砖: / gfs/测试-volume_2
条目数︰ 12
/83.txt
/28.txt
/69.txt
...

<a name="non-uniform-file-allocation"></a>
##Non 统一文件分配

NUFA 翻译或非均匀的文件访问转换器被设计给更高的首选
到时候 HPC 类型的环境中使用的本地驱动器。它可应用于分布式和副本的翻译人员;

在后者的情况下确保 * 一个 * 是本地如果空间允许的副本。

当客户端在服务器上的创建的文件时，文件被分配到砖量基于文件的名称。
此配置可能不理想，正高延迟和不必要的网络通信量，读/写操作。
到非本地砖或出口目录。NUFA 可确保在当地出口目录中创建的文件

服务器上，并作为一个结果，该服务器访问该文件减少延迟和节省带宽。
这也可以有用上装入点存储服务器上运行应用程序。

如果当地砖空间耗尽或达到最低磁盘自由的限制，而不是分配文件
到当地的砖，NUFA 分布到其他砖在同一个卷中的文件是否有
这些砖上的可用空间。

NUFA 应在卷中创建的任何数据之前启用。

使用以下命令启用 NUFA:

# gluster 卷上设置 VOLNAME cluster.nufa 启用

* * 重要 * *

NUFA 被支持在下列条件下︰

-卷与只与每个服务器的一个砖。
-为与保险丝客户端一起使用。NUFA 不支持 NFS 或 SMB。

-安装 NUFA 启用卷客户必须存在受信任的存储池中。

NUFA 调度程序还存在，用统一的翻译;见下文。


卷砖
键入群集/nufa
选择本地卷名称 brick1
subvolumes brick1 brick2 brick3 brick4 brick5 brick6 brick7
末期容积

# # #NUFA 附加选项

-查找未散列

这是一个高级的选项在哪里文件在中查找所有 subvolumes 如果他们缺少对匹配文件名的哈希值的子宗卷。默认值为上。


-本地卷名称

要考虑当地的和喜欢文件创作上的卷名称。默认设置是系统的搜索匹配的主机名卷。


-subvolumes

此选项列出了 subvolumes 这个 '群集/nufa' 卷的一部分。此翻译人员需要不止一个子宗卷。


##BitRot 检测

在 Gluster 白送检测，就可以确定"阴险"类型的磁盘
错误数据，没有迹象显示从磁盘到存储的默默地损坏在哪里
软件层比错误发生。这也有助于捕捉"后端"修修补补

（在这里直接操纵数据都需经过保险丝，砖的砖的
NFS 或任何其他访问协议。

白送检测禁用默认情况下，需要使能够使用其他
子命令。

1.为了使某一卷 <VOLNAME>白送检测︰

# gluster 卷白送 <VOLNAME>启用

和同样要禁用白送使用︰

# gluster 卷白送 <VOLNAME>禁用

注意︰ 启用白送 spanws 签名 & 洗涤器守护进程每个节点。签名者是负责

用于签名 （每个文件计算校验和） 对象和洗涤器验证
对对象数据的计算校验和。

2.洗涤塔守护进程有三 3 节流模式，调整的对象的速率
进行了验证。

# 卷白送 <VOLNAME>磨砂膏油门懒

# 卷白送 <VOLNAME>磨砂膏油门正常

# 卷白送 <VOLNAME>磨砂膏油门咄咄逼人

3.默认情况下洗涤器两周一次擦洗文件系统。它是可能的调子它擦洗

基于预定义的频率，每月，等。这可以做如下所示︰


# 卷白送 <VOLNAME>每天擦洗频率

# 卷白送 <VOLNAME>磨砂膏频率每周

# 卷白送 <VOLNAME>磨砂膏频两周一次

# 卷白送 <VOLNAME>磨砂膏频率每月

注︰ 每日擦洗不会与 GA 版本可用。

4.洗涤塔守护进程可以暂停和后来恢复时所需。这可以作为完成

如下所示︰

# 卷白送 <VOLNAME>擦洗暂停

并恢复洗涤

# 卷白送 <VOLNAME>擦洗简历

注意︰ 签名不能暂停 （或继续） 和将始终处于活动状态，只要
为该特定卷启用白送。
