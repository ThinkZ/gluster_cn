#Troubleshooting GlusterFS

本节介绍如何管理 GlusterFS 日志和最常见
与 GlusterFS 相关的故障排除方案。

# #Contents
* [管理 GlusterFS Logs](#logs)
* [疑难解答 Geo-replication](#georep)
* [疑难解答 POSIX ACLs](#posix-acls)
* [疑难解答 Hadoop 兼容 Storage](#hadoop)
* [疑难解答 NFS](#nfs)
* [疑难解答文件 Locks](#file-locks)

<a name="logs"></a>
##Managing GlusterFS 日志

# # #Rotating 日志

根据需要管理员可以旋转卷中的日志文件。

* * 到旋转日志文件 * *

    `# gluster volume log rotate `

例如，若要旋转测试卷上的日志文件︰

# gluster 卷日志旋转测试卷
日志旋转成功

> * * 注意 * *
> 日志文件旋转时，当前的日志文件的内容
> 移动到日志文件 name.epoch 时间戳。

<a name="georep"></a>
##Troubleshooting 土力工程处复制

本节介绍了相关的最常见的故障排除方案
对 GlusterFS Geo-复制。

# # #Locating 日志文件

为每个 Geo 复制会话，下列三个日志文件是
与之关联 （4 个，如果奴隶是 gluster 卷）︰

-* * 大师-日志-文件 * *-监视主机的进程的日志文件
卷
-* * 奴隶-日志-文件 * *-启动在变化的过程的日志文件
奴隶
-* * 大师-gluster-日志-文件 * *-维护挂载点的日志文件
土力工程处复制模块用于监视主音量
-* * 奴隶-gluster-日志-文件 * *-是它的奴隶对应

* * 主日志文件 * *

要获得土力工程处复制主日志文件，请使用以下命令
命令︰

`gluster volume geo-replication  config log-file`

例如︰

`# gluster volume geo-replication Volume1 example.com:/data/remote_dir config log-file `

* * 日志文件的奴隶 * *

要获取的日志文件为土力工程处复制上奴隶 （glusterd 必须是
奴隶在机器上运行），请使用以下命令︰

1.在主机上，运行以下命令︰

    `# gluster volume geo-replication Volume1 example.com:/data/remote_dir config session-owner 5f6e5200-756f-11e0-a1f0-0800200c9a66 `

显示会话所有者的详细信息。

2.关于奴隶，请运行以下命令︰

    `# gluster volume geo-replication /data/remote_dir config log-file /var/log/gluster/${session-owner}:remote-mirror.log `

3.替换会话所有者的详细信息 （步骤 1 的输出） 到输出
获取日志文件的位置的步骤 2。

    `/var/log/gluster/5f6e5200-756f-11e0-a1f0-0800200c9a66:remote-mirror.log`

# # #Rotating 土力工程处复制日志

管理员可以旋转特定的主人-奴隶的日志文件
session, as needed. When you run geo-replication's ` log-rotate`
命令，该日志文件被备份后缀的当前时间戳
文件名称和信号发送到 gsyncd，开始一个新的日志记录
日志文件。

* * 到旋转土力工程处复制日志文件 * *

-为特定的主人-奴隶会话使用旋转日志文件
下面的命令︰

    `# gluster volume geo-replication  log-rotate`

    For example, to rotate the log file of master `Volume1` and slave
    `example.com:/data/remote_dir` :

# gluster 卷土力工程处复制 Volume1 example.com:/data/remote_dir 日志旋转
日志旋转成功

-主音量使用的所有会话旋转日志文件
下面的命令︰

    `# gluster volume geo-replication  log-rotate`

    For example, to rotate the log file of master `Volume1`:

# gluster 卷土力工程处复制 Volume1 日志旋转
日志旋转成功

-旋转日志文件的所有会话使用下面的命令︰

    `# gluster volume geo-replication log-rotate`

例如，若要旋转所有会话的日志文件︰

# gluster 卷土力工程处复制日志旋转
日志旋转成功

# # #Synchronization 未完成

* * 说明 * *: GlusterFS 土力工程处复制不同步数据
完全但仍土力工程处复制显示状态是确定。

* * 解决方案 * *: 你可以强制执行完全同步的数据通过擦除
索引并重新启动 GlusterFS 土力工程处复制。重新启动后，

GlusterFS 土力工程处复制开始同步的所有数据。所有文件

使用可以长时间和高资源的校验和进行比较
利用对大型数据集的操作。


# # #Issues 中数据同步

* * 说明 * *: 土力工程处复制显示状态好，但文件一样
不会同步，只有目录和符号链接获取与同步
在日志中的以下错误消息︰

[2011年-05-02 13:42:13.467644]E [大师︰ 288:regjob] 呦︰ 失败

同步。 / some\_file\'

* * 解决方案 * *: 土力工程处复制调用 rsync v3.0.0 或更高版本的主机上
和远程计算机。您必须验证是否您已安装

所需的版本。

# Geo-复制状态显示故障非常经常

* * 说明 * *: 土力工程处复制状态显示为错误很多
与回溯跟踪类似于以下内容︰

2011-04-28 14:06:18.378859] E [syncdutils:131:log\_raise\_exception]
\ <top\>︰ 失败︰ 回溯 (最近最后调用): 文件
"/ usr/local/libexec/glusterfs/python/syncdaemon/syncdutils.py"，线
152，在 twraptf(\*aa) 文件中
"/ usr/local/libexec/glusterfs/python/syncdaemon/repce.py"，在行 118，
听着摆脱，exc，res = recv(self.inf) 文件
"/ usr/local/libexec/glusterfs/python/syncdaemon/repce.py"，在行 42，
recv 返回 pickle.load(inf) EOFError

* * 解决方案 * *: 此错误表明之间的 RPC 通信
主 gsyncd 模块和奴隶 gsyncd 模块坏了，这可
由于各种原因发生。检查它是否满足以下所有条件

先决条件︰

-无密码的 SSH 是正确设置主机和远程数据库之间
机。
-如果保险丝安装在这台机器，因为土力工程处复制模块
数据同步使用保险丝的 GlusterFS 卷装入。
-如果 * * 奴隶 * * 是一卷，检查如果启动该卷。
-如果奴隶是一个普通的目录，请验证是否目录已
已创建具有所需权限。
-如果不在默认位置安装了 GlusterFS 3.2 或更高
（硕士），已被作为前缀，在自定义安装
    location, configure the `gluster-command` for it to point to the
确切的位置。
-如果不在默认位置安装了 GlusterFS 3.2 或更高
（在奴隶） 和被前缀在自定义安装
    location, configure the `remote-gsyncd-command` for it to point to
gsyncd 所在的确切地点。

# # #Intermediate 大师进入故障状态

* * 说明 * *: 在一个级联的设置，中间的主去
与下面的日志的错误状态︰

提高 RuntimeError ("中止 uuid 变化从 %s%s"%？
RuntimeError︰ 中止 uuid 变化从 af07e07c-427f-4586-ab9f-
4bf7d299be81 至 de6b5040-8f4e-4575-8831-c4f55bd41154

* * 解决方案 * *: 在一个级联的设置中间主机是忠于
原始的主。上述日志意味着

土力工程处复制模块已在主检测更改。如果这是

所需的行为，在会话中删除选项卷 id 配置
从中间的主启动。

<a name="posix-acls"></a>
##Troubleshooting POSIX Acl

本节介绍有关的最常见的故障排除问题
POSIX Acl。

setfacl 命令失败，出现"setfacl: \<file 或目录 name\ >: 不支持的操作"错误

你可能会遇到此错误，当后端文件系统之一
服务器不装有"-o acl"选项。同样可以

通过在日志文件中查看下面的错误消息证实
服务器"Posix 不支持访问控制列表"。

* * 解决方案 * *: 重新装载与后端文件系统"-o acl"选项。

<a name="hadoop"></a>
##Troubleshooting Hadoop 兼容存储

# # #Time 同步

* * 如果时间不同步的问题 * *: 运行 MapReduce 作业可能会引发异常上
在群集中的主机。

* * 解决方案 * *: 使用 ntpd 程序的所有主机上的时间同步。

<a name="nfs"></a>
# #Troubleshooting NFS

本节介绍有关的最常见的故障排除问题
NFS。

NFS 客户端上的 # # #mount 命令失败，出现"RPC 错误︰ 程序未注册"

在 NFS 服务器上启动端口映射或 rpcbind 服务。

尚未正确启动服务器时，会遇到此错误。
对大多数 Linux 发行版，这被固定由开始端口映射︰

`$ /etc/init.d/portmap start`

对一些分布在哪里由 rpcbind，已更换端口映射
下面的命令是必需的︰

`$ /etc/init.d/rpcbind start `

启动后端口映射或 rpcbind，gluster NFS 服务器需要
重新启动。

# # #NFS 服务器启动失败"端口已被使用"错误日志文件中。

另一个 Gluster NFS 服务器运行在同一台机器上。

已存在一个 Gluster NFS 服务器的情况下，会出现此错误
在同一台机器上运行。这种情况可以证实从

如果存在以下错误行的日志文件︰

[2010年-05-26 23:40:49]E [rpc-socket.c:126:rpcsvc_socket_listen] rpc-套接字︰ 绑定套接字失败︰ 地址已在使用中

[2010年-05-26 23:40:49]E [rpc-socket.c:129:rpcsvc_socket_listen] rpc-套接字︰ 端口已被使用

[2010年-05-26 23:40:49][Rpcsvc.c:2636:rpcsvc_stage_program_register] E rpc 服务︰ 无法创建侦听连接

[2010年-05-26 23:40:49][Rpcsvc.c:2675:rpcsvc_program_register] E rpc 服务︰ 阶段注册程序失败

[2010年-05-26 23:40:49][Rpcsvc.c:2695:rpcsvc_program_register] E rpc 服务︰ 程序注册失败︰ MOUNT3、 Num: 100005，Ver: 3，端口︰ 38465

[2010年-05-26 23:40:49][Nfs.c:125:nfs_init_versions] E nfs︰ 程序初始化失败

[2010年-05-26 23:40:49]C [nfs.c:531:notify] nfs︰ 未能初始化协议


要解决此错误之一 Gluster NFS 服务器必须是
关机。在这个时候，Gluster NFS 服务器不支持运行

多个 NFS 服务器在同一台机器上。

# # #mount 命令失败，出现"rpc.statd"相关的错误消息

如果 mount 命令失败，出现以下错误消息︰

mount.nfs: rpc.statd 未运行，但需要为远程锁定。
mount.nfs︰ 使用 '-o nolock' 将锁保留在本地，或启动 statd。

对于 NFS 客户端装载 NFS 服务器，rpc.statd 服务必须是
在客户端上运行。通过运行以下命令来启动 rpc.statd 服务︰


`$ rpc.statd `

# # #mount 命令需要太长的时间才能完成。

* * 上的 NFS 客户端 * * 启动 rpcbind 服务

问题是 rpcbind 或端口映射服务不运行
NFS 客户端。解决方法这是启动这些服务之一

通过运行以下命令︰

`$ /etc/init.d/portmap start`

对一些分布在哪里由 rpcbind，已更换端口映射
下面的命令是必需的︰

`$ /etc/init.d/rpcbind start`

# # #NFS 服务器 glusterfsd 启动但初始化失败，出现"nfsrpc 服务︰ 端口映射注册的程序失败"错误消息日志中的。

NFS 启动可以成功，但初始化 NFS 服务可以
仍未防止客户端访问的挂载点。这种

可以从日志中的下列错误消息证实情况
文件︰

[2010年-05-26 23:33:47][Rpcsvc.c:2598:rpcsvc_program_register_portmap] E rpc 服务︰ 能与端口映射 notregister

[2010年-05-26 23:33:47][Rpcsvc.c:2682:rpcsvc_program_register] E rpc 服务︰ 端口映射注册的程序失败

[2010年-05-26 23:33:47][Rpcsvc.c:2695:rpcsvc_program_register] E rpc 服务︰ 程序注册失败︰ MOUNT3、 Num: 100005，Ver: 3，端口︰ 38465

[2010年-05-26 23:33:47][Nfs.c:125:nfs_init_versions] E nfs︰ 程序初始化失败

[2010年-05-26 23:33:47]C [nfs.c:531:notify] nfs︰ 未能初始化协议

[2010年-05-26 23:33:49][Rpcsvc.c:2614:rpcsvc_program_unregister_portmap] E rpc 服务︰ 无法注销与端口映射

[2010年-05-26 23:33:49][Rpcsvc.c:2731:rpcsvc_program_unregister] E rpc 服务︰ 端口映射注销的程序失败

[2010年-05-26 23:33:49][Rpcsvc.c:2744:rpcsvc_program_unregister] E rpc 服务︰ 程序注销失败︰ MOUNT3、 Num: 100005，Ver: 3，端口︰ 38465


1.* * 上的 NFS 服务器 * * 启动端口映射或 rpcbind 服务

在大多数的 Linux 发行版，端口映射可以开始使用
下面的命令︰

    `$ /etc/init.d/portmap start `

对一些分布在哪里由 rpcbind，已更换端口映射
运行以下命令︰

    `$ /etc/init.d/rpcbind start `

启动后端口映射或 rpcbind，gluster NFS 服务器需要
重新启动。

2.* * 停止另一个 NFS 服务器运行在同一台机器 * *

这种错误，也是另一个 NFS 服务器运行时
但它的同一台计算机上不是 Gluster NFS 服务器。在 Linux 上

系统，这可能是内核 NFS 服务器。决议涉及

停止其他 NFS 服务器或不运行 Gluster NFS 服务器
在这台机器。在停止之前内核 NFS 服务器，确保

没有关键的服务取决于该 NFS 服务器的出口准入。

在 Linux 上，内核 NFS 服务器可以停止使用的任一
根据分布在使用以下命令︰

    `$ /etc/init.d/nfs-kernel-server stop`

    `$ /etc/init.d/nfs stop`

3.* * 重新启动 Gluster NFS 服务器 * *

# # #mount 命令失败，出现 NFS 服务器失败错误。

mount 命令失败，出现以下错误

* 山︰ 装载失败的 NFS 服务器 '10.1.10.11': 超时 （重试）.*

请执行下列操作以解决此问题之一︰

1.* * 禁用名称查找请求从 NFS 服务器向 DNS 服务器 * *

NFS 服务器会尝试通过执行对 NFS 客户端进行身份验证
反向 DNS 查找匹配与卷文件中的主机名
客户端 IP 地址。可以有一种情况在 NFS 服务器

要么是不能够连接到 DNS 服务器或 DNS 服务器是
时间太长对 responsd 对 DNS 请求。这些延迟会导致

NFS 服务器到 NFS 客户端造成的延迟答复中
在上面看到的超时错误。

NFS 服务器提供了一个变通办法，禁用 DNS 请求，
而只依靠身份验证的客户端 IP 地址。
以下选项可以添加在这种成功安装
情况︰

    `option rpc-auth.addr.namelookup off `

> * * 注意 * *: 记住，禁用 NFS 服务器强制进行身份验证
> 的客户端使用唯一 IP 地址，如果身份验证
> 卷文件中的规则使用主机名，这些身份验证规则
> 将失败，并不允许这些客户端的安装。

* * 或 * *

2.* * NFS 客户端所使用的 NFS 版本是版本 3 * *

Gluster NFS 服务器支持 NFS 协议的版本 3。在最近

Linux 内核，默认的 NFS 版本 3 变为 4。
它很可能是无法连接到客户端计算机
Gluster NFS 服务器，因为它使用版本 4 的消息，
不 Gluster NFS 服务器所理解。可以通过解决超时

迫使 NFS 客户端使用版本 3。* * Vers * * 选项

为此目的使用 mount 命令︰

    `$ mount  -o vers=3 `

# # #showmount 与 clnt\_create 失败︰ RPC︰ 无法接收

检查您的防火墙设置为打开端口 111 端口映射
请求/答复和 Gluster NFS 服务器请求/应答。Gluster NFS

服务器运行在以下的端口号︰ 38465，38466，和
38467。

# # #Application 因"无效参数"或"价值太大定义的数据类型为"错误而失败。

这两个错误的产生通常是为 32 位 nfs 客户端或应用程序
不支持 64 位 inode 号码或大型文件。使用

从 CLI 将 Gluster NFS 返回 32 位 inode 的以下选项
相反的数字︰ nfs.enable ino32 \ <on|off\>

应用程序将受益是那些不外乎两种︰

-建 32 位和运行在 32 位机器这样他们不这样做
默认情况下支持大文件
-建 32 位，64 位系统上

此选项是默认情况下禁用，因此 NFS 返回 64 位 inode 号码
默认情况下。

建议应用程序，可以从源重建重建
用 gcc 使用以下标志︰

` -D_FILE_OFFSET_BITS=64`

<a name="file-locks"></a>
##Troubleshooting 文件锁定

In GlusterFS 3.3 you can use `statedump` command to list the locks held
上的文件。Statedump 输出还提供关于每个锁的信息

其范围，basename，持有锁的应用程序的 PID 和
等等。您可以分析要知道谁的有关锁的输出

所有者/应用程序不再是正在运行的或感兴趣那锁。后

确保没有应用程序正在使用该文件，您可以清除
lock using the following `clear lock` commands.

1.* * 在要查看锁定的文件的卷上执行 statedump
使用下面的命令: * *

    `# gluster volume statedump  inode`

例如，要显示的测试卷 statedump:

# gluster statedump 测试卷
成功的卷 statedump

    The statedump files are created on the brick servers in the` /tmp`
    directory or in the directory set using `server.statedump-path`
卷的选项。转储文件的命名约定是

    `<brick-path>.<brick-pid>.dump`.

以下是 statedump 文件的样本内容。它

表明 GlusterFS 已经进入一种状态那里有
进入锁 (entrylk) 和 inode 锁 (inodelk)。确保那些

陈旧的锁，没有资源拥有它们。

[xlator.features.locks.vol-] locks.inode
路径 = /
强制性 = 0
entrylk 计数 = 1
锁-dump.domain.domain=vol-复制-0
xlator.feature.locks.lock-dump.domain.entrylk.entrylk [0] （主动） = 类型上 basename = ENTRYLK_WRLCK = file1，pid = 714782904，所有者 = ffffff2a3c7f0000，运输 = 0x20e0670，，授予在星期一，也就是 2 月 27 日 16:01:01 2012年

conn.2.bound_xl./gfs/brick1.hashsize=14057
conn.2.bound_xl./gfs/brick1.name=/gfs/brick1/inode
conn.2.bound_xl./gfs/brick1.lru_limit=16384
conn.2.bound_xl./gfs/brick1.active_size=2
conn.2.bound_xl./gfs/brick1.lru_size=0
conn.2.bound_xl./gfs/brick1.purge_size=0

[] conn.2.bound_xl./gfs/brick1.active.1
gfid = 538a3d4a-01b0-4 d 03-9dc9-843cd8704d07
nlookup = 1
ref = 2
ia_type = 1
[xlator.features.locks.vol-] locks.inode
路径 = / file1
强制性 = 0
inodelk 计数 = 1
锁-dump.domain.domain=vol-复制-0
inodelk.inodelk[0] （主动） = 类型 = 写，何处 = 0，开始 = 0，len = 0，pid = 714787072，所有者 = 00ffff2a3c7f0000，运输 = 0x20e0670，，授予在星期一，也就是 2 月 27 日 16:01:01 2012年

2.* * 清除锁定使用下面的命令: * *

    `# gluster volume clear-locks`

    For example, to clear the entry lock on `file1` of test-volume:

# gluster 明确锁测试卷 / 种授予条目 file1
卷清除锁定成功
vol 锁︰ 进入阻塞锁 = 0 授予的锁 = 1

3.* * 清除 inode 锁使用下面的命令: * *

    `# gluster volume clear-locks`

    For example, to clear the inode lock on `file1` of test-volume:

# gluster 卷清除锁测试卷 /file1 种授予 inode 0，0-0
卷清除锁定成功
vol 锁︰ inode 阻塞锁 = 0 授予的锁 = 1

您可以对测试卷，再一次以确认执行 statedump
以上 inode 和进入锁将被清除。


