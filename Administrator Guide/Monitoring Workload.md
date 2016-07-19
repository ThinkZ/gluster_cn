#Monitoring 你的 GlusterFS 工作量

您可以监视上不同参数的 GlusterFS 卷。
监测卷有助于容量规划和性能调优
GlusterFS 卷的任务。使用这些信息，您可以确定

和解决的问题。

您可以使用卷顶和配置文件命令查看性能和
识别瓶颈/热点的每一块砖的体积。这有助于

系统管理员以获取重要的性能信息时
性能有待确认。

您还可以执行砖过程和 nfs 服务器的 statedump
卷，和也查看卷状态和卷信息的过程。

##Running GlusterFS 卷配置文件命令

GlusterFS 卷配置文件命令提供一个接口来获取
每砖 I/O 信息为每个文件操作 (FOP) 的卷。的

每砖信息有助于确定在存储瓶颈
系统。

本节描述如何运行由 GlusterFS 卷配置文件命令
执行以下操作︰

-[开始 Profiling](#start-profiling)
-[显示 I / 0 Information](#displaying-io)
-[停止 Profiling](#stop-profiling)

<a name="start-profiling"></a>
# # #Start 性能分析

您必须启动性能分析要查看的文件操作信息
一砖一瓦。

若要启动性能分析，请使用以下命令︰

`# gluster volume profile  start `

例如，若要启动性能分析测试卷上︰

# gluster 卷配置文件测试卷开始
分析开始测试卷上

分析卷上启动时，下列附加
选项显示在卷信息︰

diagnostics.count-fop-点击︰ 关于
diagnostics.latency 测量︰ 上

<a name="displaying-io"></a>
# # #Displaying I / 0 信息

可以通过使用以下命令来查看每个砖的 I/O 信息︰

`# gluster volume profile  info`

例如，要查看 I/O 信息测试卷上︰

# gluster 卷配置文件测试卷信息
砖︰ 测试︰ 出口/2
累积的统计数字︰

阻止 1b + 32b + 64b +
大小︰
Read:                0              0              0
写︰ 908 28 8

阻止 128b + 256b + 512b +
大小︰
Read:                0               6             4
Write:               5              23            16

阻止 1024b + 2048b + 4096b +
大小︰
Read:                 0              52           17
写︰ 15 120 846

阻止 8192b + 16384b + 32768b +
大小︰
Read:                52               8           34
写︰ 234 134 286

阻止 65536b + 131072b +
大小︰
Read:                               118          622
写︰ 1341年 594


Fop %延迟 Avg-Min Max 调用
滞后时间滞后时间延迟
___________________________________________________________
4.82 1132.28 21.00 800970.00 4575 写
5.70 156.47 9.00 665085.00 39163 READDIRP
11.35 315.02 9.00 1433947.00 38698 查找
11.88 1729.34 21.00 2569638.00 7382 FXATTROP
47.35 104235.02 2485.00 7789367.00 488 多次的 FSYNC

------------------

------------------

持续时间︰ 335

BytesRead: 94505058

BytesWritten: 195571980

<a name="stop-profiling"></a>
# # #Stop 性能分析

你可以停止分析卷，如果你不需要分析
信息了。

停止分析使用下面的命令︰

    `# gluster volume profile  stop`

例如，若要停止分析测试卷上︰

    `# gluster volume profile  stop`

    `Profiling stopped on test-volume`

##Running GlusterFS 卷顶部命令

GlusterFS 卷 Top 命令允许您查看 glusterfs 砖
读取、 写入、 文件打开调用等性能指标，读取文件的调用，
文件写入调用、 目录打开电话和目录实际通话。的

top 命令显示达 100 条结果。

本节描述如何运行和查看以下的结果
GlusterFS 顶部的命令︰

-[查看打开 fd 计数和最大的 fd Count](#open-fd-count)
-[查看高文件读取 Calls](#file-read)
-[查看高文件写 Calls](#file-write)
-[在 Directories](#open-dir) 上查看最高的 Open 调用
-[在 Directory](#read-dir) 上查看最高读取调用
-[查看列表的读取性能，对每个 Brick](#read-perf)
-[查看列表写的性能，对每个 Brick](#write-perf)

<a name="open-fd-count"></a>
# # #Viewing 开放 fd 计数、 最大的 fd 计数

您可以查看这两个当前开放 fd 计数 （有的文件列表
当前最打开和计数） 砖和最大值
打开 fd 计数 （当前打开的文件计数和计数
在任何给定时间点的因为打开的文件的最大数目的
服务器正在运行）。如果不指定的砖的名称，然后

打开的 fd 指标属于该卷的所有砖块将
显示。

-查看开放 fd 计数和最大的 fd 计数，使用以下命令︰

    `# gluster volume top  open [brick ] [list-cnt ]`

例如，若要查看打开 fd 计数和最大的 fd 指望砖
服务器: / 出口测试卷和目录顶 10 打开的呼叫︰

    `# gluster volume top  open brick  list-cnt `

    `Brick: server:/export/dir1 `

    `Current open fd's: 34 Max open fd's: 209 `

= = = 打开文件统计信息 = = =

打开的文件名称
调用计数

2 用/client0 / /
课程。DB


11 用/client0 / /
注册。DB


11 用/client0 / /
学生。DB


10 用/client0 / /
小贴士。PPT


10 用/client0 / /
PCBENCHM。PPT


9 用/client7 / /
学生。DB


9 用/client1 / /
学生。DB


9 用/客户端 2 / /
学生。DB


9 用/client0 / /
学生。DB


9 用/client8 / /
学生。DB


<a name="file-read"></a>
# # #Viewing 最高文件读取调用

您可以查看最高读取的调用上一砖一瓦。如果砖的名字不是

指定，则默认情况下，将显示 100 个文件的列表。

-查看最高文件读取调用使用下面的命令︰

    `# gluster volume top  read [brick ] [list-cnt ] `

例如，若要查看最高读取调用砖服务器上︰ 进出口
测试卷︰

    `# gluster volume top  read brick  list-cnt `

    `Brick:` server:/export/dir1

= = = 读取文件状态 = = =

读取文件名
调用计数

116 /clients/client0/~dmtmp/SEED/LARGE.FIL

64 /clients/client0/~dmtmp/SEED/MEDIUM.FIL

54 /clients/client2/~dmtmp/SEED/LARGE.FIL

54 /clients/client6/~dmtmp/SEED/LARGE.FIL

54 /clients/client5/~dmtmp/SEED/LARGE.FIL

54 /clients/client0/~dmtmp/SEED/LARGE.FIL

54 /clients/client3/~dmtmp/SEED/LARGE.FIL

54 /clients/client4/~dmtmp/SEED/LARGE.FIL

54 /clients/client9/~dmtmp/SEED/LARGE.FIL

54 /clients/client8/~dmtmp/SEED/LARGE.FIL

<a name="file-write"></a>
# # #Viewing 最高文件编写调用

您可以查看具有最高的文件编写调用每个文件的列表
砖。如果不指定砖名称，然后在默认情况下，列表 100

文件将会显示。

-查看最高文件写入调用使用下面的命令︰

    `# gluster volume top  write [brick ] [list-cnt ] `

例如，若要查看最高写入调用砖服务器上︰ 进出口
测试卷︰

    `# gluster volume top  write brick  list-cnt `

    `Brick: server:/export/dir1 `

= = = 写文件统计信息 = = =
写调用计数文件名

83 /clients/client0/~dmtmp/SEED/LARGE.FIL

59 /clients/client7/~dmtmp/SEED/LARGE.FIL

59 /clients/client1/~dmtmp/SEED/LARGE.FIL

59 /clients/client2/~dmtmp/SEED/LARGE.FIL

59 /clients/client0/~dmtmp/SEED/LARGE.FIL

59 /clients/client8/~dmtmp/SEED/LARGE.FIL

59 /clients/client5/~dmtmp/SEED/LARGE.FIL

59 /clients/client4/~dmtmp/SEED/LARGE.FIL

59 /clients/client6/~dmtmp/SEED/LARGE.FIL

59 /clients/client3/~dmtmp/SEED/LARGE.FIL

<a name="open-dir"></a>
# # #Viewing 高公开呼吁目录

您可以查看具有最高的 open 调用目录上的文件的列表
每一块砖。如果不指定砖名称，然后所有的度量

将显示属于该卷的砖块。

-查看的 open 调用上使用以下每个目录列表
命令︰

    `# gluster volume top  opendir [brick ] [list-cnt ] `

例如，若要查看打开的呼叫砖服务器上: / 出口/的
测试卷︰

    `# gluster volume top  opendir brick  list-cnt `

    `Brick: server:/export/dir1 `

= = = 目录打开统计 = = =

Opendir 计数目录名称

1001 用/client0 /

454 用/client8 /

454 用/客户端 2 /
         
454 用/client6 /

454 用/client5 /

454 用/client9 /

443 用/client0 /

408 用/client1 /

408 用/client7 /

402 用/client4 /

<a name="read-dir"></a>
# # #Viewing 最高读呼吁目录

您可以查看已读调用上的最高目录的文件列表
一砖一瓦。如果不指定砖名称，然后所有的度量

将显示属于该卷的砖。

-查看列表的最高目录读取每个砖使用要求
下面的命令︰

    `# gluster volume top  readdir [brick ] [list-cnt ] `

例如，若要查看最高目录读取呼吁砖
服务器︰ 进出口测试卷︰

    `# gluster volume top  readdir brick  list-cnt `

    `Brick: `

= = = Readdirp 统计数据目录 = = =

readdirp 计数目录名称

1996 用/client0 /

1083 用/client0 /

904 用/client8 /

904 用/客户端 2 /

904 用/client6 /

904 用/client5 /

904 用/client9 /

812 用/client1 /

812 用/client7 /

800 用/client4 /

<a name="read-perf"></a>
# # #Viewing 列表中的每一块砖上的读取性能

您可以查看每个砖上的文件的读的吞吐量。如果砖名称

是未指定，然后所有的度量标准砖属于那
将显示卷。输出将读取的吞吐量。


= = = 读取吞吐量文件状态 = = =

读取文件名时间
通过
把 (MBp
) s
     
2570.00 用/client0 / /-2011年-01-31
TRIDOTS。壶 15:38:36.894610 

2570.00 用/client0 / /-2011年-01-31
PCBENCHM。PPT 15:38:39.815310                                                            

2383.00 用/客户端 2 / /-2011年-01-31
MEDIUM.FIL 15:52:53.631499
                                                  
2340.00 用/client0 / /-2011年-01-31
MEDIUM.FIL 15:38:36.926198
                                                  
2299.00 用/client0 / /-2011年-01-31
LARGE.FIL 15:38:36.930445
                                                 
2259.00 用/client0 / /-2011年-01-31
课程。X04 15:38:40.549919


2221.00 用/client0 / /-2011年-01-31
学生。VAL 15:52:53.298766

                                                  
2221.00 用/client3 / /-2011年-01-31
课程。DB 15:39:11.776780


2184.00 用/client3 / /-2011年-01-31
MEDIUM.FIL 15:39:10.251764
                                                  
2184.00 用/client5 / /-2011年-01-31
BASEMACH。DOC 15:39:09.336572                                         


此命令将启动指定的计数和块大小为 dd
和相应的吞吐量的措施。

-查看列表的读取性能上使用以下每个砖
命令︰

    `# gluster volume top  read-perf [bs  count ] [brick ] [list-cnt ]`

例如，到视图阅读砖服务器的性能: / 出口/的
测试卷，256 块大小的 1，计数和清单 10:

    `# gluster volume top  read-perf bs 256 count 1 brick list-cnt `

    `Brick: server:/export/dir1 256 bytes (256 B) copied, Throughput: 4.1 MB/s `

= = = 读取吞吐量文件状态 = = =

读取文件名时间
通过
把 (MBp
) s

2912.00 用/client0 / /-2011年-01-31
TRIDOTS。壶 15:38:36.896486

                                                   
2570.00 用/client0 / /-2011年-01-31
PCBENCHM。PPT 15:38:39.815310

                                                   
2383.00 用/客户端 2 / /-2011年-01-31
MEDIUM.FIL 15:52:53.631499
                                                   
2340.00 用/client0 / /-2011年-01-31
MEDIUM.FIL 15:38:36.926198

2299.00 用/client0 / /-2011年-01-31
LARGE.FIL 15:38:36.930445
                                                              
2259.00 用/client0 / /-2011年-01-31
课程。X04 15:38:40.549919

                                                   
2221.00 用/client9 / /-2011年-01-31
学生。VAL 15:52:53.298766

                                                   
2221.00 用/client8 / /-2011年-01-31
课程。DB 15:39:11.776780

                                                   
2184.00 用/client3 / /-2011年-01-31
MEDIUM.FIL 15:39:10.251764
                                                   
2184.00 用/client5 / /-2011年-01-31
BASEMACH。DOC 15:39:09.336572

                                                   
<a name="write-perf"></a>
# # #Viewing 列表中的每一块砖上的写入性能

您可以查看列表中的每一块砖上的文件的写吞吐量。如果砖

未指定名称，然后度量属于的所有砖块
将显示该卷。输出将写入吞吐量。


此命令将启动指定的计数和块大小为 dd
和相应的吞吐量的措施。若要查看列表的写

对每个砖的性能︰

-查看写入性能上使用以下每个砖的列表
命令︰

    `# gluster volume top  write-perf [bs  count ] [brick ] [list-cnt ] `

例如，若要查看砖服务器上的写入性能: / 出口/的
测试卷，256 块大小的 1，计数和清单 10:

    `# gluster volume top  write-perf bs 256 count 1 brick list-cnt `

    `Brick`: server:/export/dir1

    `256 bytes (256 B) copied, Throughput: 2.8 MB/s `

= = = 写吞吐量文件统计信息 = = =

写文件名时间
吞吐量
() MBps
         
1170.00 用/client0 / /-2011年-01-31
SMALL.FIL 15:39:09.171494

1008.00 用/client6 / /-2011年-01-31
LARGE.FIL 15:39:09.73189

949.00 用/client0 / /-2011年-01-31
MEDIUM.FIL 15:38:36.927426

936.00 用/client0 / /-2011年-01-31
LARGE.FIL 15:38:36.933177
897.00 用/client5 / /-2011年-01-31
MEDIUM.FIL 15:39:09.33628

897.00 用/client6 / /-2011年-01-31
MEDIUM.FIL 15:39:09.27713

885.00 用/client0 / /-2011年-01-31
SMALL.FIL 15:38:36.924271

528.00 用/client5 / /-2011年-01-31
LARGE.FIL 15:39:09.81893

516.00 用/client6 / /-2011年-01-31
紧固件。MDB 15:39:01.797317


##Displaying 卷信息

可以显示有关特定卷或所有卷的信息
需要。

-显示有关特定卷使用以下的信息
命令︰

    `# gluster volume info ``VOLNAME`

例如，要显示有关测试卷的信息︰

# gluster 卷信息测试卷
卷名︰ 测试卷
类型︰ 分发
状态︰ 创建
砖的数目︰ 4
砖头︰
Brick1: server1: / exp1
Brick2: server2: / exp2
Brick3: server3: / exp3
Brick4: server4: / exp4

-显示所有卷使用下面的命令的信息︰

    `# gluster volume info all`

# gluster 卷信息所有

卷名︰ 测试卷
类型︰ 分发
状态︰ 创建
砖的数目︰ 4
砖头︰
Brick1: server1: / exp1
Brick2: server2: / exp2
Brick3: server3: / exp3
Brick4: server4: / exp4

卷名︰ 镜子
类型︰ 分布式复制
状态︰ 开始
砖的数目︰ 2 X 2 = 4
砖头︰
Brick1: server1: / brick1
Brick2: server2: / brick2
Brick3: server3: / brick3
Brick4: server4: / brick4

卷名︰ 卷
类型︰ 分发
状态︰ 开始
砖的数目︰ 1
砖头︰
砖︰ 服务器: / brick6

卷上的 ##Performing Statedump

Statedump 是一个机制，通过它你可以得到所有的详细信息
内部变量和 glusterfs 进程时的状态
发出的命令。您可以执行砖过程的 statedumps

和卷的 nfs 服务器进程使用 statedump 命令。的

可以使用下列选项来确定什么样的信息就是
甩了︰

-* * mem * *-转储的内存使用率和内存池详细信息
砖块。

-* * iobuf * *-转储 iobuf 细节的砖。

-* * 上一页 * *-转储加载翻译人员私人信息。

-* * callpool * *-转储卷的挂起的调用。

-* * fd * *-转储卷的打开的 fd 表。

-* * inode * *-转储卷的 inode 表。

* * 到显示卷 statedump * *

-显示 statedump 的卷或使用以下的 NFS 服务器
命令︰

    `# gluster volume statedump  [nfs] [all|mem|iobuf|callpool|priv|fd|inode]`

例如，要显示的测试卷 statedump:

# gluster statedump 测试卷
成功的卷 statedump

    The statedump files are created on the brick servers in the` /tmp`
    directory or in the directory set using `server.statedump-path`
卷的选项。转储文件的命名约定是

    `<brick-path>.<brick-pid>.dump`.

-由零八，statedump 的输出存储在
    ` /tmp/<brickname.PID.dump>` file on that particular server. Change
目录中的 statedump 文件，使用下面的命令︰

    `# gluster volume set  server.statedump-path `

例如，若要更改的 statedump 文件的位置
测试卷︰

# gluster 音量设置测试卷路径 server.statedump /usr/local/var/log/glusterfs/dumps /
集的卷成功

你可以查看 statedump 文件使用的已更改的路径
下面的命令︰

    `# gluster volume info `

##Displaying 卷状态

您可以显示有关特定卷，砖的状态信息或
所有的卷，根据需要。状态信息可以用于了解

砖、 nfs 进程和整个文件系统的当前状态。
状态信息也可以用于监视和调试卷
信息。您可以查看以及下列卷状态

详细信息︰

-* * 详细 * *-显示有关砖的附加信息。

-* * 客户 * *-显示列表的客户端连接到该卷。

-* * mem * *-显示的内存使用率和内存池详细信息
砖块。

-* * inode * *-显示卷的 inode 表。

-* * fd * *-显示打开的 fd （文件描述符） 表的
卷。

-* * callpool * *-显示卷的挂起的调用。

* * 到显示卷状态 * *

-显示有关特定卷使用以下的信息
命令︰

    `# gluster volume status [all| []] [detail|clients|mem|inode|fd|callpool]`

例如，要显示有关测试卷的信息︰

# gluster 卷状态测试卷
状态的卷︰ 测试卷
砖端口在线 PID
--------------------------------------------------------
拱︰ 出口/1 24009 Y 22445
--------------------------------------------------------
拱︰ 出口/2 24010 Y 22450

-显示所有卷使用下面的命令的信息︰

    `# gluster volume status all`

# gluster 卷状态所有
状态的卷︰ 卷测试
砖端口在线 PID
--------------------------------------------------------
拱︰ 出口/4 24010 Y 22455

状态的卷︰ 测试卷
砖端口在线 PID
--------------------------------------------------------
拱︰ 出口/1 24009 Y 22445
--------------------------------------------------------
拱︰ 出口/2 24010 Y 22450

-使用以下的砖显示附加信息
命令︰

    `# gluster volume status  detail`

例如，若要显示有关的砖的其他信息
测试卷︰

# gluster 卷状态测试卷详细信息
状态的卷︰ 测试卷
-------------------------------------------
砖︰ 拱︰ 出口/1
端口︰ 24009
在线︰ Y
Pid: 16977
文件系统︰ rootfs
设备︰ rootfs
装载选项︰ rw
13.8 GB 的可用磁盘空间︰
磁盘总空间︰ 46.5 GB
Inode 大小︰ n/A
Inode 计数︰ n/A
免费的 Inode: n/A

砖的数目︰ 1
砖头︰
砖︰ 服务器: / brick6

-显示客户端访问使用的卷列表
下面的命令︰

    `# gluster volume status  clients`

例如，若要显示列表的客户端连接到
测试卷︰

# gluster 卷状态测试卷客户
砖︰ 拱︰ 出口/1
连接的客户端︰ 2
主机名字节读取 BytesWritten
--------          ---------    ------------
127.0.0.1:1013 776 676
127.0.0.1:1012 50440 51200

-显示内存使用情况和砖使用的内存池详细信息
下面的命令︰

    `# gluster volume status  mem`

例如，要显示的内存使用率和内存池详细信息
砖的测试卷︰

卷的内存状态︰ 测试卷
----------------------------------------------
砖︰ 拱︰ 出口/1
Mallinfo
--------
竞技场︰ 434176
Ordblks: 2
Smblks: 0
Hblks: 12
Hblkhd: 40861696
Usmblks: 0
Fsmblks: 0
Uordblks: 332416
Fordblks: 101760
Keepcost: 100400

内存统计信息
-------------
名称 HotCount ColdCount PaddedSizeof AllocCount MaxAlloc
----                               -------- --------- ------------ ---------- --------
测试-体积-服务器︰ fd_t 0 16384 92 57 5
测试-体积-服务器︰ dentry_t 59 965 84 59 59
测试-体积-服务器︰ inode_t 60 964 148 60 60
测试-体积-服务器︰ rpcsvc_request_t 0 525 6372 351 2
glusterfs:struct saved_frame 0 4096 124 2 2
glusterfs:struct rpc_req 0 4096 2236年 2 2
glusterfs:rpcsvc_request_t 1 524 6372 2 1
glusterfs:call_stub_t 0 1024年 1220年 288 1
glusterfs:call_stack_t 0 8192 2084年 290 2
glusterfs:call_frame_t 0 16384 172 1728年 6

-显示 inode 表的卷使用下面的命令︰

    `# gluster volume status  inode`

例如，要显示的测试卷的 inode 表︰

# gluster 卷状态测试卷 inode
测试卷的 inode 表
----------------------------------------------
砖︰ 拱︰ 出口/1
积极的 inode:
GFID 查找 Ref IA 型
----                                            -------            ---   -------
6f3fe173-e07a-4209-abb6-484091d75499 1 9 2
370d35d7-657e-44dc-bac4-d6dd800ec3d3 1 1 2

LRU inode:
GFID 查找 Ref IA 型
----                                            -------            ---   -------
80f98abe-cdcf-4c1d-b917-ae564cf55763 1 0 1
3a58973d-d549-4ea6-9977-9aa218f233de 1 0 1
2ce0197d-87a9-451b-9094-9baa38121155 1 0 2

-显示打开的 fd 表使用以下的卷
命令︰

    `# gluster volume status  fd`

例如，要显示的测试卷打开的 fd 表︰

# gluster 卷状态测试卷 fd

音量控制测试-FD 表
----------------------------------------------
砖︰ 拱︰ 出口/1
连接 1:
使 = 0 MaxFDs = 128 FirstFree = 4
FD 条目 PID 使标志
--------            ---                 --------            -----
0                   26311               1                   2
1                   26310               3                   2
2                   26310               1                   2
3                   26311               3                   2
         
连接 2:
使 = 0 MaxFDs = 128 FirstFree = 0
没有打开的 fds
         
连接 3:
使 = 0 MaxFDs = 128 FirstFree = 0
没有打开的 fds

-显示挂起的调用的卷使用下面的命令︰

    `# gluster volume status  callpool`

每次调用已包含调用框架调用堆栈。

例如，要显示的测试卷挂起的调用︰

# gluster 卷状态测试卷

挂起的调用为卷测试卷
----------------------------------------------
砖︰ 拱︰ 出口/1
挂起的调用︰ 2
电话 Stack1
UID: 0
GID: 0
PID: 26338
独特︰ 192138
帧︰ 7
第 1 帧
Ref 计数 = 1
翻译 = 测试卷服务器
完成 = 否
框架 2
引用计数 = 0
翻译 = 测试卷 posix
完成 = 否
父 = 测试卷访问控制
风从 = default_fsync
风对-主席之友-多次的 fsync > = FIRST_CHILD(this)
3 帧
Ref 计数 = 1
翻译 = 测试卷访问控制
完成 = 否
父 = repl 锁
风从 = default_fsync
风对-主席之友-多次的 fsync > = FIRST_CHILD(this)
4 帧
Ref 计数 = 1
翻译 = 测试卷锁
完成 = 否
父 = 测试卷 io 线程
风从 = iot_fsync_wrapper
风对-主席之友-多次的 fsync > = FIRST_CHILD （这）
5 帧
Ref 计数 = 1
翻译 = 测试卷 io 线程
完成 = 否
父 = 测试卷标记
风从 = default_fsync
风对-主席之友-多次的 fsync > = FIRST_CHILD(this)
6 帧
Ref 计数 = 1
翻译 = 测试卷标记
完成 = 否
父 = 出口/1
风从 = io_stats_fsync
风对-主席之友-多次的 fsync > = FIRST_CHILD(this)
框架 7
Ref 计数 = 1
翻译 = 出口/1
完成 = 否
父 = 测试卷服务器
风从 = server_fsync_resume
风对 = bound_xl-主席之友-多次的 fsync >


