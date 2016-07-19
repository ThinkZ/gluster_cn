#Setting GlusterFS 服务器卷起来

卷是的砖每块砖哪里出口逻辑集合
在受信任的存储池的服务器上的目录。大部分的 gluster

卷上执行管理操作。

若要在您的存储环境中创建新的卷，请指定砖
这构成卷。您已经创建了一个新的卷后，你必须

在尝试装入它之前启动它。

# # #Formatting 和安装砖

# # #Creating 精简调配的逻辑卷

若要创建一个精简调配的逻辑卷，请继续执行下列步骤︰

1.通过使用 pvcreate 命令创建物理体积 （pv）。
例如︰

      `pvcreate --dataalignment 1280K /dev/sdb`

在这里，/dev/sdb 是一个存储设备。
使用正确的 dataalignment 选项基于您的设备。

> * * 注意 * *
>
> 的设备名称和对齐值取决于您正使用的设备。

2.从光伏用 vgcreate 命令创建卷组 (VG):

例如︰

  `vgcreate --physicalextentsize 128K gfs_vg /dev/sdb`

它被建议从一个存储设备必须创建只有一个 VG。

3.创建一个薄池使用下列命令︰

1.创建 LV 作为元数据设备使用以下命令︰

      `lvcreate -L metadev_sz --name metadata_device_name VOLGROUP`

例如︰

      `lvcreate -L 16776960K --name gfs_pool_meta gfs_vg`

2.创建 LV 作为数据设备使用以下命令︰

      `lvcreate -L datadev_sz --name thin_pool VOLGROUP`

例如︰

      `lvcreate -L 536870400K --name gfs_pool gfs_vg`

3.创建一个薄的池从 LV 数据和元数据 LV 使用下面的命令︰

      `lvconvert --chunksize STRIPE_WIDTH --thinpool VOLGROUP/thin_pool --poolmetadata VOLGROUP/metadata_device_name`

例如︰

      `lvconvert --chunksize 1280K --thinpool gfs_vg/gfs_pool --poolmetadata gfs_vg/gfs_pool_meta`

> * * 注意 * *
>
> 在默认情况下，新调配的块在精简池归零，防止泄漏不同的块设备之间的数据。

  `lvchange --zero n VOLGROUP/thin_pool`

例如︰

      `lvchange --zero n gfs_vg/gfs_pool`

4.从先前创建的池，使用 lvcreate 命令创建精简调配的卷︰

例如︰

  `lvcreate -V 1G -T gfs_vg/gfs_pool -n gfs_lv`

它建议应精简池中创建只有一个 LV。

设置格式砖使用支持的 XFS 配置，装载砖，以及验证正确安装砖。

  1. Run `# mkfs.xfs -f -i size=512 -n size=8192 -d su=128K,sw=10 DEVICE` to format the bricks to the supported XFS file system format. Here, DEVICE is the thin LV. The inode size is set to 512 bytes to accommodate for the extended attributes used by GlusterFS.

  Run `# mkdir /mountpoint` to create a directory to link the brick to.

在 /etc/fstab 中添加一个条目︰

      `/dev/gfs_vg/gfs_lv    /mountpoint  xfs rw,inode64,noatime,nouuid      1 2`

  Run `# mount /mountpoint` to mount the brick.

  Run the `df -h` command to verify the brick is successfully mounted:

' # df-h
/dev/gfs_vg/gfs_lv 16 G 1.2 G 15 G 7%/exp1'

-下列类型的卷可以创建在您的存储
环境︰

-* * 分布式 * *-分布式的卷分布整个文件
砖的体积。你可以使用分布式的卷在哪里

要求是扩展存储和冗余是要么
不重要或由其他硬件和软件层提供。

-* * 复制 * * — — 复制卷复制文件跨砖
在该卷。你可以在环境中使用复制的卷

在这里，高可用性和高可靠性是关键。

-* * 条纹 * * — — 跨砖的带区的卷的条纹数据
卷。为了获得最佳结果，您应该使用带区的卷只在

高并发环境中访问非常大的文件。

-* * 分布式条纹 * *-分布带区的卷带区数据
跨两个或更多节点群集中。您应该使用

分布式带区的卷在哪里的要求是向规模
存储和访问非常高并发环境中
大型文件是至关重要的。

-* * 分布式复制 * *-分布式复制卷
在卷中复制砖来分发文件。你

可以使用分布式复制环境中的卷在哪里
要求是扩展存储和高可靠性是
关键。分布式复制的卷也提供更完善

读取性能在大多数环境中。

-* * 分布式条纹复制 * * — — 分布式条纹复制
卷条带化的数据分布在复制砖
群集。为了获得最佳结果，您应该使用分布式条纹

复制在高并发环境中的卷在哪里
并行存取的非常大的文件和性能是至关重要的。
在此版本中，支持这个卷类型的配置
只为 Map Reduce 工作负载。

-* * 条纹复制 * * — — 条纹复制卷条纹数据
在群集中的复制砖。为获得最佳效果，你

应使用条纹复制卷在高并发
环境那里有非常大的文件的并行存取
和性能是至关重要的。在此版本中，配置的

此卷类型仅支持地图减少工作量。

-* * 分散 * *-分散的卷基于纠删码
提供空间有效保护防止磁盘或服务器故障。
它存储到每个砖的原始文件的编码的片段
恢复所需的一个子集的片段是的方式
原始文件。可以吉凶未卜的砖的数量

失去对数据的访问是由管理员在卷上配置
创建时间。

-* * 分布分散 * *-分布分散的卷分发
在分散的 subvolumes 文件。这具有相同的优点

分发复制的卷，但使用分散存储的数据
成砖。

* * 到创建新的卷 * *

-创建新的卷︰

    `# gluster volume create [stripe  | replica | disperse] [transport tcp | rdma | tcp,rdma] `

例如，要创建卷称为包含的测试卷
server3: / exp3 和 server4: / exp4:

# gluster 卷创建测试卷 server3: / exp3 server4: / exp4
创建测试卷已成功
请开始访问的数据量。

# # 创建分布式卷

在分布式的卷文件随机分布在砖的
该卷。使用分布式的卷，您需要扩展存储和

冗余是要么不重要或由其他提供
硬件/软件层。

> * * 注意 * *:
> 在分布式卷磁盘/服务器故障可以导致严重
> 数据丢失因为目录内容随机交叉传播
> 砖的体积。

![] distributed_volume() https://cloud.githubusercontent.com/assets/10970993/7412364/ac0a300c-ef5f-11e4-8599-e7d06de1165c.png


* * 到创建分布式的卷 * *

1.创建一个受信任的存储池。

2.创建分布式的卷︰

    `# gluster volume create  [transport tcp | rdma | tcp,rdma] `

例如，要创建一个分布式的卷与四个存储
使用 tcp 服务器︰

# gluster 卷创建测试卷 server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4
创建测试卷已成功
请开始访问的数据量。

（可选）您可以显示的卷信息︰


# gluster 卷信息
卷名︰ 测试卷
类型︰ 分发
状态︰ 创建
砖的数目︰ 4
传输类型︰ tcp
砖头︰
Brick1: server1: / exp1
Brick2: server2: / exp2
Brick3: server3: / exp3
Brick4: server4: / exp4

例如，要创建一个分布式的卷与四个存储
在无限带宽的服务器︰

# gluster 卷创建测试卷运输 rdma server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:
> 确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

# # 创建复制卷

复制的卷跨越多个砖在创建文件的副本
卷。你可以在环境中使用复制的卷在哪里

高可用性和高可靠性是关键。

> * * 注意 * *:
> 的砖数应等于的复制副本计数
> 复制卷。为了防止服务器和磁盘故障，它是

> 推荐卷的砖是从不同的服务器。

![] replicated_volume() https://cloud.githubusercontent.com/assets/10970993/7412379/d75272a6-ef5f-11e4-869a-c355e8505747.png


* * 到创建复制的卷 * *

1.创建一个受信任的存储池。

2.创建复制的卷︰

    `# gluster volume create  [replica ] [transport tcp | rdma | tcp,rdma] `

例如，要创建具有两个存储服务器复制的卷︰

# gluster 卷创建测试卷副本 2 运输 tcp server1: / exp1 server2: / exp2
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:

>-确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

>-GlusterFS 将无法创建复制的卷副本集的多个砖上是否存在相同的同行。为 eg。四节点复制副本集的一个砖是同一对等方上存在更多的卷。

    > ```
# gluster 卷创建 <volname>副本 4 server1: / brick1 server1: / brick2 server2: / brick3 server4: / brick4
    volume create: <volname>: failed: Multiple bricks of a replicate volume are present on the same server. This setup is not optimal. Use 'force' at the end of the command if you want to override this behavior.```

    >  Use the `force` option at the end of command if you want to create the volume in this case.

# # # 仲裁配置为副本卷的

仲裁者卷是副本 3 卷在哪里第三砖充当仲裁者砖。此配置有防止分裂大脑发生的机制。


它可以创建使用以下命令︰

# gluster 卷创建 <VOLNAME>副本 3 仲裁 1 host1:brick1 host2:brick2 host3:brick3

有关此配置的详细信息，可以发现在 * 功能︰ 空燃比-仲裁者-卷 *

请注意，可以使用副本 3 的仲裁配置创建分布式复制卷以及。

# # 创建带区卷

带区的卷中跨卷中砖，对数据进行条带。最佳

结果，您应该使用带区的卷只在高并发性
环境访问非常大的文件。

> * * 注意 * *:
> 的砖的数量应得到的条纹计数等于
> 条纹卷。

![] striped_volume() https://cloud.githubusercontent.com/assets/10970993/7412387/f411fa56-ef5f-11e4-8e78-a0896a47625a.png


* * 到创建带区卷 * *

1.创建一个受信任的存储池。

2.创建带区的卷︰

# gluster 卷创建 [条纹] [运输 tcp | rdma | tcp，rdma]

例如，要创建带区的卷在两个存储服务器︰

# gluster 卷创建测试卷条纹 2 运输 tcp server1: / exp1 server2: / exp2
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:
> 确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

# # 创建分布式带区的卷

带区的卷的条纹文件分布在两个或更多节点在
群集。为了获得最佳结果，您应该使用分布式条纹

卷到规模存储和高要求在哪里
并发环境中访问非常大的文件是至关重要的。

> * * 注意 * *:
> 的砖的数量应得到多的条纹计数
> 分布带区的卷。

![] distributed_striped_volume() https://cloud.githubusercontent.com/assets/10970993/7412394/0ce267d2-ef60-11e4-9959-43465a2a25f7.png


* * 到创建分布式条纹卷 * *

1.创建一个受信任的存储池。

2.创建分布式的带区的卷︰

    `# gluster volume create  [stripe ] [transport tcp | rdma | tcp,rdma] `

例如，要创建分布式的带区的卷八
存储服务器︰

# gluster 卷创建测试卷条纹 4 运输 tcp server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4 server5: / exp5 server6: / exp6 server7: / exp7 server8: / exp8
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:
> 确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

# # 创建分布式复制卷

在卷中复制砖来分发文件。您可以使用

分布式复制的卷中要求的环境
扩展存储和高可靠性是至关重要的。分发

复制的卷还提供改进的读取的性能，在大多数
环境。

> * * 注意 * *:
> 的砖的数量应得到的副本数的倍数
> 分布式复制的卷。此外，顺序砖

> 指定关于数据保护的影响很大。每个 replica\_count

> 你给的列表中的连续砖会形成一个副本集，
> 所有副本集都组合成一卷全分布式套。要使

> 确保副本集成员不放在同一节点上，列出
> 最早的砖在每个服务器上，然后在每个服务器上的第二个砖
> 中以相同的顺序和等等。

![] distributed_replicated_volume() https://cloud.githubusercontent.com/assets/10970993/7412402/23a17eae-ef60-11e4-8813-a40a2384c5c2.png


* * 到创建分布式复制卷 * *

1.创建一个受信任的存储池。

2.创建分布式复制的卷︰

    `# gluster volume create  [replica ] [transport tcp | rdma | tcp,rdma] `

例如，四个节点的分布式 （复制） 的卷
双向镜像︰

# gluster 卷创建测试卷副本 2 运输 tcp server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4
创建测试卷已成功
请开始访问的数据量。

例如，要创建六个节点分布 （复制） 的卷
与双向镜像︰

# gluster 卷创建测试卷副本 2 运输 tcp server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4 server5: / exp5 server6: / exp6
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:
>-确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

>-GlusterFS 将无法创建分发复制卷的副本集的多个砖上是否存在相同的同行。为 eg。四节点分发 （复制） 的卷副本多个砖集是存在于同一对等方。

    > ```
# gluster 卷创建 <volname>副本 2 server1: / brick1 server1: / brick2 server2: / brick3 server4: / brick4
    volume create: <volname>: failed: Multiple bricks of a replicate volume are present on the same server. This setup is not optimal. Use 'force' at the end of the command if you want to override this behavior.```

    >  Use the `force` option at the end of command if you want to create the volume in this case.


# # 创建分布式复制带区的卷

分布式的带区复制的卷分布在条带化的数据
在群集中的复制的砖。为了获得最佳结果，您应该使用

在高并发环境中分布式条纹复制的卷
在平行的非常大的文件访问和性能是至关重要的。
在此版本中，此卷类型配置仅支持
映射减少工作量。

> * * 注意 * *:
> 的砖的数量应得到条纹计数数目的倍数
> 和计数为分布式的条纹复制卷的副本。

* * 到创建分布式条纹复制卷 * *

1.创建一个受信任的存储池。

2.创建一个分布式的条纹复制的卷，使用下面的代码
命令︰

    `# gluster volume create  [stripe ] [replica ] [transport tcp | rdma | tcp,rdma] `

例如，若要创建分布式复制带区的卷
跨八个存储服务器︰

# gluster 卷创建测试卷条纹 2 副本 2 运输 tcp server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4 server5: / exp5 server6: / exp6 server7: / exp7 server8: / exp8
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:
>-确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

>-GlusterFS 将无法创建分发复制卷的副本集的多个砖上是否存在相同的同行。为 eg。四节点分发 （复制） 的卷副本多个砖集是存在于同一对等方。

    > ```
# gluster 卷创建 <volname>条纹 2 副本 2 server1: / brick1 server1: / brick2 server2: / brick3 server4: / brick4
    volume create: <volname>: failed: Multiple bricks of a replicate volume are present on the same server. This setup is not optimal. Use 'force' at the end of the command if you want to override this behavior.```

    >  Use the `force` option at the end of command if you want to create the volume in this case.

# # 创建带区复制的卷

条纹复制的卷条纹数据跨复制的又一块砖
群集。为了获得最佳结果，您应该使用条纹复制的卷中

高并发环境那里很有并行访问
大型文件和性能是至关重要的。在此版本中，配置

此卷的类型是仅支持地图减少工作量。

> * * 注意 * *:
> 的砖的数量应得到多复制计数和
> 条纹条纹复制的卷数。

![] striped_replicated_volume() https://cloud.githubusercontent.com/assets/10970993/7412405/3563ac48-ef60-11e4-823f-ee6100c65ad7.png


* * 复制到创建带区卷 * *

1.创建受信任的存储池组成的存储服务器，
将构成卷。

2.创建带区复制的卷︰

    `# gluster volume create  [stripe ] [replica ] [transport tcp | rdma | tcp,rdma] `

例如，要创建带区卷跨复制四
存储服务器︰

# gluster 卷创建测试卷条纹 2 副本 2 运输 tcp server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4
创建测试卷已成功
请开始访问的数据量。

若要创建带区跨所有六个存储服务器复制卷︰

# gluster 卷创建测试卷条纹 3 副本 2 运输 tcp server1: / exp1 server2: / exp2 server3: / exp3 server4: / exp4 server5: / exp5 server6: / exp6
创建测试卷已成功
请开始访问的数据量。

如果未指定的传输类型，* tcp * 用作
默认情况。您还可以设置其他选项，如果需要，如

auth.allow 或 auth.reject。

> * * 注意 * *:
>-确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

>-GlusterFS 将无法创建分发复制卷的副本集的多个砖上是否存在相同的同行。为 eg。四节点分发 （复制） 的卷副本多个砖集是存在于同一对等方。

    > ```
# gluster 卷创建 <volname>条纹 2 副本 2 server1: / brick1 server1: / brick2 server2: / brick3 server4: / brick4
    volume create: <volname>: failed: Multiple bricks of a replicate volume are present on the same server. This setup is not optimal. Use `force` at the end of the command if you want to override this behavior.```

    >  Use the `force` option at the end of command if you want to create the volume in this case.

# # 创建分散卷

分散的卷都基于纠删码。它条纹编码的数据的

具有一些冗余 addedd，跨多个砖卷中的文件。你

可以使用分散的卷具有可靠性与可配置级别
最小的太空垃圾。

* * 冗余 * *

每个分散的卷已定义卷时的冗余值
创建。此值确定用多少块砖可失去了

中断的卷的操作。它还确定的金额

使用此公式的卷的可用空间︰

< 可用大小 > = < 砖大小 > * (#Bricks-冗余)

所有砖块的分散染料集应都具有相同的容量，否则为当
小砖已满，没有额外的数据将允许在
分散设置。

很重要的地注意到，与 3 砖和冗余 1 配置
将有更少的可用空间 （总的物理空间 66.7%) 比
配置 10 砖和冗余 1 （90%)。然而第一个

将会比第二个安全之一 （大致的失效概率
第二种配置如果比第一个大 4.5 倍）。

例如，分散的卷由 4 TB 和冗余的 6 砖
2 将完全运作，甚至两个砖无法访问。然而

第三次访问砖必使卷下，因为它不会
要读取或写入它的可能。卷的可用空间会平等

到 16 TB。

在 GlusterFS 的纠删码的执行限制到冗余
值小于 #Bricks / 2 (或等价地，冗余 * 2 < #Bricks)。
几乎有一半的砖数等于冗余会
相当于一个副本 2 卷，和可能已复制的卷将
在这种情况下表现得更好。

* * 最优卷 * *

最糟糕的事情擦除码具有在性能方面之一就是
RMW （读-修改-写） 周期。纠删码操作中的某块

大小和它不能使用较小的。这意味着，如果用户问题

写的不会填满一个完整的块文件的一部分，它需要对
从文件的当前内容读取的剩余部分，合并它们，
计算更新编码的块，最后，写由此产生的数据。

这会增加延迟，降低性能，当发生这种情况。一些 GlusterFS

性能 xlators 可以帮助减少或甚至消除此问题
当使用分散时，应考虑到某些工作负载，但它
卷内的特定用例。

当前实现分散卷的使用取决于大小的块
对砖和冗余的数量︰ 512 * (#Bricks-冗余) 字节。
此值是条带大小。

使用组合的 #Bricks/冗余，给的两个电源
条带大小将使分散卷在大多数工作负荷中更好地执行
因为它是更典型的是在多个块中写入信息的
两根 （针对示例数据库、 虚拟机和许多应用程序）。

被认为是这些组合 * 最优 *。

例如，配置 6 砖和冗余 2 会有条纹
大小为 512 * (6-2) = 2048 个字节，所以它被认为是最佳。配置

7 砖和冗余 2 会有条带大小为 2560 字节，需要
RMW 周期为多的写入操作 （当然这总是取决于使用情况）。

* * 到创建分散的卷 * *

1.创建一个受信任的存储池。

2.创建分散的卷︰

    `# gluster volume create [disperse [<count>]] [redundancy <count>] [transport tcp | rdma | tcp,rdma]`

可以通过指定的砖的数量创建一个分散的卷
分散设置，通过指定的数目的冗余砖，或两者。

如果 * 分散 * 未指定，则或 &lt; 计数 &gt; _ _ 是缺掉的
整个卷将被视为由所有组成的单分散集
在命令行中所列举的砖头。

如果 * 冗余 * 不指定，它自动计算要
最优值。如果此值不存在，它假定为 '1' 和

会显示警告消息︰

# gluster 卷创建测试卷分散 4 server{1..4}:/bricks/test-volume
没有为此配置最优冗余价值。你想要创建的卷与冗余 1 吗？(y /) n


在所有情况下在哪里 * 冗余 * 自动计算并不是
等于 '1'，将显示一条警告消息︰

# gluster 卷创建测试卷分散 6 server{1..6}:/bricks/test-volume
此配置的最优冗余是 2。你想要创建的卷与此值吗？(y /) n


_redundancy_ 必须大于 0，和砖的总数必须
大于 2 * _redundancy_。这意味着，一个分散的卷必须

有一个最低的 3 砖。

如果未指定的传输类型，* tcp * 用作默认值。你

可以设置附加选项如果需要，还要在另一个卷
类型。

> * * 注意 * *:

>-确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

>-GlusterFS 将无法创建分散的卷分散集的多个砖上是否存在相同的同行。

    > ```
# gluster 卷创建 <volname>分散 3 server1:/brick{1..3}
卷创建︰ <volname>︰ 失败︰ 复制卷的多个砖是一种存在于同一服务器上。这个设置不是最优的。

    Do you still want to continue creating the volume? (y/n)```

    >  Use the `force` option at the end of command if you want to create the volume in this case.

# # 创建分布式分散卷

分布式的分散的卷相当于分发复制
卷，但使用分散而不是复制的 subvolumes。

* * 到创建分布式分散卷 * *

1.创建一个受信任的存储池。

2.创建分布式分散的卷︰

    `# gluster volume create disperse <count> [redundancy <count>] [transport tcp | rdma | tcp,rdma]`

若要创建一个分布式的分散的卷，* 分散 * 关键字和
&lt; 计数 &gt; 是强制性的并在指定的砖数
命令行必须必须是分散计数的倍数。

* 冗余 * 正是分散卷相同。

如果未指定的传输类型，* tcp * 用作默认值。你

可以设置附加选项如果需要，还要在另一个卷
类型。

> * * 注意 * *:

>-确保您开始您的卷之前您尝试装入他们或
> 其他客户端操作后山将挂起。

>-GlusterFS 将不能创建一个分布式的分散的卷分散集的多个砖上是否存在相同的同行。

    > ```
# gluster 卷创建 <volname>分散 3 server1:/brick{1..6}
卷创建︰ <volname>︰ 失败︰ 复制卷的多个砖是一种存在于同一服务器上。这个设置不是最优的。

    Do you still want to continue creating the volume? (y/n)```

    > Use the `force` option at the end of command if you want to create the volume in this case.

# # 开始卷

在您尝试装载它们之前，您必须启动您的卷。

* * 到开始卷 * *

-启动卷︰

    `# gluster volume start `

例如，若要启动测试卷︰

# gluster 卷开始测试卷
开始测试卷已成功
