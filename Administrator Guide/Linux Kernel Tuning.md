Linux 内核调整为 GlusterFS
---------------------------------

时不时，问题到这里来内部和与许多
在 Gluster 有什么要说关于内核优化，如果任何的爱好者。

稀有的内核优化是基于 Linux 内核做
在大多数工作负荷的不错。但对此的另一面

设计。Linux 内核历史上急切地吃光了大量的 RAM，

提供有是有一些，或驱动对缓存的主要方式
提高性能。

对于大多数情况下，这是很好，但作为的工作量的增加量
随时间和群集的负载是嫁给了服务器，否极泰来
非常麻烦，导致工作等的灾难性故障。

有了相当的经验看在大内存系统
负载过重的回归，是 CAD、 EDA 或类似的工具，我们已经
有时遇到 Gluster 的稳定性问题。我们到了

仔细分析磁盘的等待时间的数量与内存占用
在天。这给了我们一个相当非凡盘捣毁，巨大的故事

iowaits、 内核哎呀，磁盘挂起等。

这篇文章是我很多的经验与优化选项的结果
这在许多网站上执行了。调优不仅帮助

总体响应能力，但它极大地稳定群集
整体。

当它来到内存调整旅程入手 VM'
有大量奇怪的选项，可以造成很大的子系统
混乱。

# # # vm.swappiness

vm.swappiness 是一个可调节的内核参数，控制多少
内核比 RAM 更受偏爱互换。源代码级别的代码，它也被定义

作为偷映射的内存的趋势。一个高的 swappiness 值是指

内核将更倾向于取消映射映射页。低的 swappiness

值的意思相反，内核将不太容易取消映射映射
页面。换句话说，vm.swappiness 值越高，越多

系统将互换。

高系统交换有着非常不良的影响，当有巨大
正在交换 ram 的数据块。许多人都认为，

要设置较高，但以我的经验，将值设置为"0"值
会引起性能增加。

符合的细节在这里-< http://lwn.net/Articles/100978/ >

但这些变化又应该由测试和适当尽职调查
从自己的应用程序的用户。负载过重，流媒体应用程序

应设置此值为 '0'。通过更改此值为 '0'，

系统的响应速度提高了。

# # # vm.vfs\_cache\_pressure

此选项控制内核倾向回收内存
这用来缓存目录和 inode 的对象。

在 vfs\_cache\_pressure 的默认值 = 的 100 内核将尝试
以回收 dentries 和 inode 遵守的"公平"速度
pagecache 和 swapcache 的回收。降低 vfs\_cache\_pressure 原因

要倾向于保留 dentry 和 inode 的内核缓存。当

vfs\_cache\_pressure = 0，内核将不会收回 dentries 和
由于内存压力和这的 inode，很容易导致出的内存
条件。增加 vfs\_cache\_pressure 超越 100 会使内核

愿意去回收 dentries 和 inode。

与 GlusterFS，大量的存储很多用户和多个小文件
很容易落得在服务器端由于使用大量的 RAM
'inode/dentry' 缓存，导致性能下降时内核
保持在 40 GB RAM 的系统上爬行通过数据结构。更改

此值高于 100 已经帮助很多用户实现公平缓存
和更多的响应，从内核。

# # # vm.dirty\_background\_ratio

# # # vm.dirty\_ratio

这两个第一 (vm.dirty\_background\_ratio) 定义
背景刷新之前可以变脏的内存的百分比
页面到磁盘启动。直到这一百分比达到没有页面

被刷新到磁盘。然而当冲洗开始，然后它就能

而无需中断任何运行的后台程序中
前景色。

现在这两个参数 (vm.dirty\_ratio) 的第二个定义
内存可以占领的脏页的百分比
强制刷新开始。如果脏页的百分比达到这

阈值，然后所有进程同步的并且他们是不
允许继续，直到他们有要求的 io 操作
实际执行，数据在磁盘上。在高性能的情况下

I/O 的机器，这导致了一个问题因为数据缓存把砍掉了和
所有做 I/O 的进程阻塞以等待 I/O。这将

导致一大批挂过程，导致高负荷
从而导致系统不稳定和蹩脚的性能。

从标准值降低他们导致一切要刷新到
磁盘而不是存储在 RAM 中的很多。它可以帮助大内存系统

这通常会刷新到磁盘，造成巨大的 45-90 G pagecache
等待时间为前端应用程序，降低总体响应能力
和交互性。

# # #"1"\ > /proc/sys/vm/pagecache

页面缓存是持有数据从文件和可执行文件的磁盘缓存
程序中，即与文件或块设备的实际内容的页面。
页面缓存 （磁盘高速缓存） 用来减少磁盘读取次数。A

值为 '1' 表示 RAM 的 1%用于此目的，所以，大多数的
他们从磁盘而不是 RAM 读取。此值是有点腥

后上面的值已更改。更改此选项不是

有必要，但如果你是关于控制仍偏执
pagecache，此值应该帮助。

# # #"最后期限"\ > /sys/block/sdc/queue/scheduler

I/O 调度程序是一个组件的 Linux 内核，决定如何
读取和写入缓冲区将排队等待的基础设备。
从理论上讲 'noop' 是与智能的 RAID 控制器更好因为
（物理） 磁盘几何参数对 Linux 一无所知，因此它可以是
高效让控制器，深知磁盘几何处理
请尽快。但是最后期限似乎增强

性能。你可以阅读更多关于他们在 Linux 内核源代码

文件︰ linux/Documentation/block/\*iosched.txt。我也有

看到 '读' 吞吐量增加混合操作 （许多写入） 期间。

# # #"256"\ > /sys/block/sdc/queue/nr\_requests

这是之前他们是缓存的 I/O 请求的大小
传达给调度程序磁盘。内部队列大小

一些控制器 (queue\_depth) 大于 I/O 调度
nr\_requests，I/O 调度程序不会得到太多的机会
正确排序和合并请求。截止日期或 CFQ 调度程序喜欢

有 nr\_requests 要设置值的 queue\_depth，2 倍的
默认值为给定的控制器。合并命令和请求

帮助计划程序在大负载期间能够响应能力更强。

# # # 回声"16"\ > /proc/sys/vm/page-cluster

页面群集控制被写到交换中的页数
单一的尝试。它定义交换 I/O 大小，在上面的示例

添加 '16' 根据 RAID 条带大小为 64k。这不会有意义

你在使用 swappiness 后 = 0，但如果你定义 swappiness = 10 或
20，然后使用此值有助于当你有 RAID 条带大小为
64 k。

# # # blockdev — — setra 4096 /dev/ <devname>(如:-康体局，hdc 或 dev\_mapper)

默认块设备设置经常导致糟糕的性能
许多的 RAID 控制器。添加上面的选项，设置预读到

4096 \ * 512 字节的部门，对于流复制，增加
速度，饱和高清集成缓存由期间提前阅读
使用的内核编写 I/O 的期。它可以放在缓存数据

这将由下一个读取请求。太多预读可能会杀死

巨大的文件，如果它使用可能有用的驱动时间上的随机 I/O 或
加载数据缓存之外。

几个其他杂项的变化，在文件系统建议
水平但没有测试如下。请确保您

文件系统所知道关于的条带大小和阵列中的磁盘数目。
例如，条带大小为 64k 和 6 磁盘的 raid5 阵列
(有效 5，因为在每一个带区集有一个磁盘做
奇偶校验）。这些都是建立在理论假设基础上，从收集

各种其他博客/文章 RAID 专家提供的。

-\ > ext4 fs，5 个磁盘，64 K 条纹、 4 K 大厦单位

mkfs-文本 4-E stride=\$((64/4))

-\ > xfs，5 个磁盘，64 K 条纹、 512 字节扇区的单位

mkfs-txfs-d sunit=\$((64\*2))-d swidth=\$((5\*64\*2))

你可能想要考虑增加流的上述条带大小
大型文件。

警告︰ 上述变化都是高度主观的某些类型的
应用程序。这篇文章并不能保证任何任何好处

事先未经奉用户为其各自的尽职调查
应用程序。只应在预期的授意下

在整体的系统响应速度的增加或如果它解决正在进行
问题。

更丰富和有趣的文章/电子邮件/博客阅读

-< http://dom.as/2008/02/05/linux-io-schedulers/ >
-< http://www.nextre.it/oracledocs/oraclemyths.html >
-< http://article.gmane.org/gmane.linux.raid/17546 >
-< https://lkml.org/lkml/2006/11/15/40 >
-< http://misterd77.blogspot.com/2007/11/3ware-hardware-raid-vs-linux-software.html >
-< http://www.jejik.com/articles/2008/04/benchmarking_linux_filesystems_on_software_raid_1/ >

`   Last updated by: `[`User:y4m4`](User:y4m4 "wikilink")

# # # 评论︰ jdarcy

一些附加的调优建议︰

`   * The choice of scheduler is *really* hardware- and workload-dependent, and some schedulers have unique features other than performance.  For example, last time I looked cgroups support was limited to the cfq scheduler.  Different tests regularly do best on any of cfq, deadline, or noop.  The best advice here is not to use a particular scheduler but to try them all for a specific need.`

`   * It's worth checking to make sure that /sys/.../max_sectors_kb matches max_hw_sectors_kb.  I haven't seen this problem for a while, but back when I used to work on Lustre I often saw that these didn't match and performance suffered.`

`   * For read-heavy workloads, experimenting with /sys/.../readahead_kb is definitely worthwhile.`

`   * Filesystems should be built with -I 512 or similar so that more xattrs can be stored in the inode instead of requiring an extra seek.`

`   * Mounting with noatime or relatime is usually good for performance.`

# # # 答复︰ y4m4

`   Agreed i was about write those parameters you mentioned. I should write another elaborate article on FS changes. `

y4m4

# # # 评论︰ 生态

`       1 year ago`\
`   This article is the model on which all articles should be written.  Detailed information, solid examples and a great selection of references to let readers go more in depth on topics they choose.  Great benchmark for others to strive to attain.`\
`       Eco`\

# # # 评论︰ y4m4

`   sysctl -w net.core.{r,w}mem_max = 4096000 - this helped us to Reach 800MB/sec with replicated GlusterFS on 10gige  - Thanks to Ben England for these test results. `\
`       y4m4`

# # # 评论︰ bengland

`   After testing Gluster 3.2.4 performance with RHEL6.1, I'd suggest some changes to this article's recommendations:`

`   vm.swappiness=10 not 0 -- I think 0 is a bit extreme and might lead to out-of-memory conditions, but 10 will avoid just about all paging/swapping.  If you still see swapping, you need to probably focus on restricting dirty pages with vm.dirty_ratio.`

`   vfs_cache_pressure > 100 -- why?   I thought this was a percentage.`

`   vm.pagecache=1 -- some distros (e.g. RHEL6) don't have vm.pagecache parameter. `

`   vm.dirty_background_ratio=1 not 10 (kernel default?) -- the kernel default is a bit dependent on choice of Linux distro, but for most workloads it's better to set this parameter very low to cause Linux to push dirty pages out to storage sooner.    It means that if dirty pages exceed 1% of RAM then it will start to asynchronously write dirty pages to storage. The only workload where this is really bad: apps that write temp files and then quickly delete them (compiles) -- and you should probably be using local storage for such files anyway. `

`   Choice of vm.dirty_ratio is more dependent upon the workload, but in other contexts I have observed that response time fairness and stability is much better if you lower dirty ratio so that it doesn't take more than 2-5 seconds to flush all dirty pages to storage. `

`   block device parameters:`

`   I'm not aware of any case where cfq scheduler actually helps Gluster server.   Unless server I/O threads correspond directly to end-users, I don't see how cfq can help you.  Deadline scheduler is a good choice.  I/O request queue has to be deep enough to allow scheduler to reorder requests to optimize away disk seeks.  The parameters max_sectors_kb and nr_requests are relevant for this.  For read-ahead, consider increasing it to the point where you prefetch for longer period of time than a disk seek (on order of 10 msec), so that you can avoid unnecessary disk seeks for multi-stream workloads.  This comes at the expense of I/O latency so don't overdo it.`

`   network:`

`   jumbo frames can increase throughput significantly for 10-GbE networks.`

`   Raise net.core.{r,w}mem_max to 540000 from default of 131071  (not 4 MB above, my previous recommendation).  Gluster 3.2 does setsockopt() call to use 1/2 MB mem for TCP socket buffer space.`\
`       bengland`\

# # # 评论︰ hjmangalam

`   Thanks very much for noting this info - the descriptions are VERY good.. I'm in the midst of debugging a misbehaving gluster that can't seem to handle small writes over IPoIB and this contains some useful pointers.`

`   Some suggestions that might make this more immediately useful:`

`   - I'm assuming that this discussion refers to the gluster server nodes, not to the gluster native client nodes, yes?  If that's the case, are there are also kernel parameters or recommended settings for the client nodes?`\
`   -  While there are some cases where you mention that a value should be changed to a particular # or %, in a number of cases you advise just increasing/decreasing the values, which for something like  a kernel parameter is probably not a useful suggestion.  Do I raise it by 10?  10%  2x? 10x?  `

`   I also ran across a complimentary page, which might be of  interest - it explains more of the vm variables, especially as it relates to writing.`\
`   "Theory of Operation and Tuning for Write-Heavy Loads" `\
`      `<http://www.westnet.com/~gsmith/content/linux-pdflush.htm>\
`   and refs therein.`

`       hjmangalam`

# # # 评论︰ bengland

`   Here are some additional suggestions based on recent testing:`\
`   - scaling out number of clients -- you need to increase the size of the ARP tables on Gluster server if you want to support more than 1K clients mounting a gluster volume.  The defaults for RHEL6.3 were too low to support this, we used this:`

`   net.ipv4.neigh.default.gc_thresh2 = 2048`\
`   net.ipv4.neigh.default.gc_thresh3 = 4096`

`   In addition, tunings common to webservers become relevant at this number of clients as well, such as netdev_max_backlog, tcp_fin_timeout, and somaxconn.`

`   Bonding mode 6 has been observed to increase replication write performance, I have no experience with bonding mode 4 but it should work if switch is properly configured, other bonding modes are a waste of time.`

`       bengland`\
`       3 months ago`