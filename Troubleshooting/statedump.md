#Statedump
Statedump 是由 glusterfs 进程与不同的数据结构的状态，其中可能包含积极的 inode，fds，mempools，iobufs，参每 xlator 等不同类型的内存分配统计数据生成一个文件。

# #How 如何生成 statedump
我们可以找到在哪里使用 'gluster — — 打印 statedumpdir' 创建 statedump 文件的目录命令。
如果尚未目前基于的安装类型，请创建该目录。
Lets call this directory `statedump-directory`.

我们可以生成 statedump，使用 ' 杀-USR1 <pid-of-gluster-process>'。
glusterd/glusterfs/glusterfsd 过程只不过是 gluster 过程。

还有一些命令来生成为砖过程/nfs 服务器/quotad statedumps

For bricks:     `gluster volume statedump <volname>`

For nfs server: `gluster volume statedump <volname> nfs`

For quotad:     `gluster volume statedump <volname> quotad`

For brick-processes files will be created in `statedump-directory` with name of the file as `hyphenated-brick-path.<pid>.dump.timestamp`. For all other processes it will be `glusterdump.<pid>.dump.timestamp`.

# #How 如何阅读 statedump
我们将看到每种类型的 statedump 的片段。

文件的第一个和最后一个行有写作 statedump 文件的起始和结束时间。时间将会在 UTC 时区。


mallinfo 返回状态打印以下格式。请阅读有关每个字段的含义的详细信息的人 mallinfo。

# # #Mallinfo
```
[] mallinfo
mallinfo_arena = 100020224 / * 非 mmapped 分配空间 （字节数） * /
mallinfo_ordblks = 69467 / * 空闲块数目 * /
mallinfo_smblks = 449 / * 免费 fastbin 块的数量 * /
mallinfo_hblks = 13 / * mmapped 地区的数量 * /
mallinfo_hblkhd = 20144128 / * 空间分配在 mmapped 地区 （字节数） * /
mallinfo_usmblks = 0 / * 最大总分配空间 （字节数） * /
mallinfo_fsmblks = 39264 / * 空间中释放的 fastbin 块 （字节数） * /
mallinfo_uordblks = 96710112 / * 共分配空间 （字节数） * /
mallinfo_fordblks = 3310112 / * 总可用空间 （字节数） * /
mallinfo_keepcost = 133712 / * 最顶层，可发布空间 （字节数） * /
```

# # #Data 结构分配统计
对于每个 xlator 数据结构翻译加载调用关系图中的每个内存按以下格式显示︰

Xlator 与名称为︰ glusterfs
```
[global.glusterfs-内存使用量] #[global.xlator-名称-内存使用]
num_types = 119 #It 显示的数据类型，它使用数量
```

现在每个数据类型，它输出的内存使用情况。

```
[使用类型 gf_common_mt_gf_timer_t memusage-global.glusterfs]
#[global.xlator-名称-使用类型 < 与数据类型相关联的标记 > memusage]
大小 = 112 #num_allocs 倍即 num_allocs sizeof(data-type) * sizeof （数据类型）
num_allocs = 2 #Number 的分配的数据类型，它以 statedump 次处于活动状态。
max_size = 168 #max_num_allocs 倍即 max_num_allocs sizeof(data-type) * sizeof （数据类型）
max_num_allocs = 在进程的生存期内任何一点的活动分配 3 #Maximum 次数。
total_allocs = 7 #Number 次此数据在生活过程中分配。
```

# # #Mempools

Mempools 是优化，以减少数据类型分配的数量。如果我们创建让 mem 池说 1024年元素的数据类型，只有如果池中的所有 1024年元素都都在积极使用，将从堆使用系统调用像 calloc，拨出的新元素。


内存池分配的每个 xlator 显示在下面的格式︰

```
[内存] #Section 名称
-----=-----
池名称 = #pool 名称保险丝︰ fd_t = <xlator-name>: <data-type>
热计数 = 1 #number 的正在使用的内存元素。即为该池它的数目是 'fd_t' s 处于使用中。

冷计数 = 1023年 #number 内存中的元素不在使用中。如果新的分配是需要它将可以作为从这里直到池中的所有元素都是使用即冷计数变为 0 时。

padded_sizeof = 108 #Each 内存元素用于填充双链表 + ptr 的内存 + 是使用信息操作元素的游泳池，这个尺寸元素大小后填充
池未命中 = 0 #Number 的次元不得不从堆分配，因为从池中的所有元素都都在积极地利用。
alloc 计数 = 314 #Number 的时代这种类型的数据通过分配出这一进程的生命。这可能包括池未命中以及。

最大分配 = 3 #Maximum 从数量的元素中积极利用在进程的生存期内任何一点的池。这确实 * 不 * 包括池未命中。

恶狗 stdalloc = 0 #Denotes 的分配由堆，一旦冷计数达到 0，它们不能通过 mem_put() 发布的数量。
max stdalloc = 0 #Maximum 数目从堆正在使用中的进程的生存期内任一点的分配。
```

# # #Iobufs
```
[] iobuf.global
iobuf_pool = iobufs 0x1f0d970 #The 内存池
iobuf_pool.default_page_size=131072 #The 默认大小为 iobuf （如果没有 iobuf 大小指定分配的默认大小）
#iobuf_arena︰ 一个舞台上表示一组特定大小的 iobufs
iobuf_pool.arena_size=12976128 # iobuf 池的初始大小 (不包括 stdalloc 蹭内存或新增的竞技场)
iobuf_pool.arena_cnt=8 #Total 的阿里纳斯池中的线程数
（由于他们超过由 iobuf_pool 提供的默认最大页面大小），将会被 stdalloc 的 iobufs iobuf_pool.request_misses=0 #The 号码。
```

有的阿里纳斯 3 名单

1.舞台列表︰ 阿里纳斯在 iobuf 池的创建和舞台上，在使用期间分配 (active_cnt ！ = 0) 将此列表的一部分。
2.清除列表︰ 可以清除的阿里纳斯 (不活跃的 iobufs，active_cnt = = 0)。
3.填充列表︰ 阿里纳斯没有免费的 iobufs。

```
[purge.1] #purge。<S.No.>

舞台上结构 purge.1.mem_base=0x7fc47b35f000 #The 地址
在那个舞台上活跃的 iobufs 的 purge.1.active_cnt=0 #The 数
在舞台上的未使用 iobufs 的 purge.1.passive_cnt=1024 #The 数
purge.1.alloc_cnt=22853 #Total 分配此池中 （iobuf 从这个舞台上分配的次数）
purge.1.max_active=7 #Max 积极 iobufs 从这个舞台上，在这个进程的生存期内任何一点。
在这一领域的所有 iobufs 的 purge.1.page_size=128 #Size。

[arena.5] #arena。<S.No.>

arena.5.mem_base=0x7fc47af1f000
arena.5.active_cnt=0
arena.5.passive_cnt=64
arena.5.alloc_cnt=0
arena.5.max_active=0
arena.5.page_size=32768
```

如果任何舞台上的 active_cnt 为非零，然后 statedump 还将 iobuf 目录。
```
[arena.6.active_iobuf.1] #arena。<S.No>.active_iobuf。<iobuf.S.No.>

iobuf arena.6.active_iobuf.1.ref=1 #refcount
iobuf arena.6.active_iobuf.1.ptr=0x7fdb921a9000 #address

[] arena.6.active_iobuf.2
arena.6.active_iobuf.2.ref=1
arena.6.active_iobuf.2.ptr=0x7fdb92189000
```

在任何给定时间点，如果有很多的填充阿里纳斯，那么这可能是一个迹象的 iobuf 泄漏。

# # #Call 堆栈
使用调用堆栈处理收到的 gluster 落物保护结构。调用堆栈包含执行 fop 的过程有关的 uid/gid/pid 等的信息。每个调用堆栈包含不同的调用帧每处理那 fop 的 xlator。


```
[global.callpool.stack.3] #global.callpool.stack。<Serial-Number>

堆栈 = 0x7fc47a44bbe0 #Stack 地址
uid = 0 #Uid 的执行 fop 的过程
gid = 0 #Gid 的执行 fop 的过程
pid = 6223 #Pid 的执行 fop 的过程
独特 = 2778年 #Xlators 像空燃比做 copy_frame 和不同的堆栈中执行的操作，这个 id 是有用来找出是复制帧间相关的堆栈
lk 所有者 = 0000000000000000 #Some 保险丝 fops 有 lk 的所有者。
op = 查找 #Fop
类型 = 1 #Type 任择议定书 》 即 FOP/管理-OP
cnt = 9 #Number 此堆栈中的帧。
```
# Call 框架
每一帧都有哪些 xlator 框架属于，它对从伤口并将展开到的功能是什么的信息。它还提到如果展开发生与否。如果我们观察挂起系统中，想要找出哪个 xlator 造成它。看 statedump，看看什么是最后的 xlator，尚有待解开。


```
[global.callpool.stack.3.frame.2]#global.callpool.stack。<stack-serial-number>.frame。<frame-serial-number>

帧 = 0x7fc47a611dbc #Frame 地址
ref_count = 0 #Incremented 在时间的风和递减时的放松。
翻译 = r2-客户端-1 #Xlator 此帧属于
完成 = 0 #if 此值为 1，意味着此帧已经解除。0，如果现在还不放松。

父 = r2-复制-0 #Parent xlator 此帧
wind_from = afr_lookup #Parent xlator 函数从中风发生
wind_to =-儿童 [i]-主席之友-查找 > 上一页
unwind_to = afr_lookup_cbk #Parent xlator 函数对它放松发生
```

# # #History 在保险丝的行动

保险丝维护操作发生在引信中的历史记录。

```
[] xlator.mount.fuse.history
时间 = 2014年-07-09 16:44:57.523364
消息 = [0] fuse_release: release （): 4590:，fd: 0x1fef0d8，gfid: 3afb4968-5100-478d-91e9-76264e634c9f

时间 = 2014年-07-09 16:44:57.523373
消息 = [0] send_fuse_err︰ 发送手术成功的 18 inode 3afb4968-5100-478d-91e9-76264e634c9f

时间 = 2014年-07-09 16:44:57.523394
消息 = [0] fuse_getattr_resume: 4591，STAT，路径: (/ iozone.tmp)，gfid: (3afb4968-5100-478d-91e9-76264e634c9f)
```

# # #Xlator 配置
```
[群集/replicate.r2-复制-0] #Xlator 类型，名称信息
child_count = 2 #Number 的 xlator 的孩子
下面的 #Xlator 具体配置
child_up [0] = 1
pending_key [0] =trusted.afr.r2-客户端-0
child_up [1] = 1
pending_key [1] =trusted.afr.r2-客户端-1
data_self_heal =
metadata_self_heal = 1
entry_self_heal = 1
data_change_log = 1
metadata_change_log = 1
进入 change_log = 1
read_child = 1
favorite_child = 1
wait_count = 1
```

# Graph/inode 表
```
[活动图-1]

conn.1.bound_xl./data/brick01a/homegfs.hashsize=14057
conn.1.bound_xl./data/brick01a/homegfs.name=/data/brick01a/homegfs/inode
conn.1.bound_xl./data/brick01a/homegfs.lru_limit=16384 #Least 最近使用过的大小限制
conn.1.bound_xl./data/brick01a/homegfs.active_size=690 #Number 经历某种 fop 要有精确的是 inode 的至少一个 ref。
conn.1.bound_xl./data/brick01a/homegfs.lru_size=183 #Number 的 inode lru 列表中存在
conn.1.bound_xl./data/brick01a/homegfs.purge_size=0 #Number 的 inode 目前在清除列表
```

# # #Inode
```
[conn.1.bound_xl./data/brick01a/homegfs.active.324] # 第 324 inode 活跃 inode 列表中
gfid = e6d337cf-97eb-44b3-9492-379ba3f6ad42 #Gfid 的 inode
nlookup = 13 #Number 的时间查找发生了从客户端或保险丝内核
fd 计数 = 4 #Number fds 软件在 inode 上打开
ref = 11 #Number 的 inode 上采取的裁判
ia_type = 1 #Type 的 inode。这应改为一些字符串:-(


[conn.1.bound_xl./data/brick01a/homegfs.lru.1] # 1 inode lru 列表中的。请注意，引用计数是这些 inode 的零。

gfid = 5114574e-69bc-412b-9e52-f13ff087c6fc
nlookup = 5
fd 计数 = 0
ref = 0
ia_type = 2
```
# # #Inode 上下文
对于每个 inode 每 xlator 可以存储一些上下文。这种情况下还可以打印在 statedump。这里是锁 xlator inode ctx

```
[xlator.features.locks.homegfs-] locks.inode
path=/homegfs/users/dfrobins/gfstest/r4/SCRATCH/fort.5102-文件的路径
强制性 = 0
inodelk 计数 = 5 #Number 的 inode 锁
锁-dump.domain.domain=homegfs-复制-0:self-治愈 #Domain 名称在哪里的自我康复带锁，以防止多个疗愈作用相同的文件
inodelk.inodelk[0] （主动） = 类型 = 写，何处 = 0，开始 = 0，len = 0，pid = 18446744073709551615，所有者 = 080b1ada117f0000，客户端 = 0xb7fc30，connection-id=compute-30-029.com-3505-2014/06/29-14:46:12:477358-homegfs-client-0-0-1，在孙俊授予 29 11:01 2014年 #Active 锁信息

inodelk.inodelk[1] （阻止） = 类型 = 写，何处 = 0，开始 = 0，len = 0，pid = 18446744073709551615，所有者 = c0cb091a277f0000，客户端 = 0xad4f10，connection-id=gfs01a.com-4080-2014/06/29-14:41:36:917768-homegfs-client-0-0-0，被阻挡在太阳君 29 11:04:44 2014年 #Blocked 锁信息

锁-dump.domain.domain=homegfs-复制-0:metadata #Domain 名称在哪里元数据操作采用锁维护复制的一致性
锁-dump.domain.domain=homegfs-复制-0 #Domain 名称条目/数据操作在锁维护复制的一致性
inodelk.inodelk[0] （主动） = 类型 = 写，何处 = 0，开始 = 11141120，len = 131072，pid = 18446744073709551615，所有者 = 080b1ada117f0000，客户端 = 0xb7fc30，connection-id=compute-30-029.com-3505-2014/06/29-14:46:12:477358-homegfs-client-0-0-1，在太阳君 29 授予 11:10:36 2014年 #Active 锁信息
```

# #FAQ
使用 statedump # # #How 如何调试内存泄漏？

# # #Using 内存会计功能︰

`https://bugzilla.redhat.com/show_bug.cgi?id=1120151` is one of the bugs which was debugged using statedump to see which data-structure is leaking. Here is the process used to find what the leak is using statedump. According to the bug the observation is that the process memory usage is increasing whenever one of the bricks is wiped in a replicate volume and a `full` self-heal is invoked to heal the contents. Statedump of the process is taken using kill -USR1 `<pid-of-gluster-self-heal-daemon>`.
```
grep-w num_allocs glusterdump.5225.dump.1405493251
num_allocs = 77078
num_allocs = 87070
num_allocs = 117376
....

grep 热计数 glusterdump.5225.dump.1405493251
热计数 = 16384
热计数 = 16384
热计数 = 4095
....

在 statedump 文件中找出标签查找匹配项。
```
grep 的 statedump 透露太多拨款下复制，下面的数据类型

1.gf_common_mt_asprintf
2.gf_common_mt_char
3.gf_common_mt_mem_pool。

After checking afr-code for allocations with tag `gf_common_mt_char` found `data-self-heal` code path does not free one such allocated memory. `gf_common_mt_mem_pool` suggests that there is a leak in pool memory. `replicate-0:dict_t`, `glusterfs:data_t` and `glusterfs:data_pair_t` pools are using lot of memory, i.e. cold_count is `0` and too many allocations. Checking source code of dict.c revealed that `key` in `dict` is allocated with `gf_common_mt_char` i.e. `2.` tag and value is created using gf_asprintf which in-turn uses `gf_common_mt_asprintf` i.e. `1.`. Browsing the code for leak in self-heal code paths lead to a line which over-writes a variable with new dictionary even when it was already holding a reference to another dictionary. After fixing these leaks, ran the same test to verify that none of the `num_allocs` are increasing even after healing 10,000 files directory hierarchy in statedump of self-heal daemon.
请检查 http://review.gluster.org/8316 有关修补程序/代码的详细信息。

# # #Debugging 泄漏的内存池︰
Statedump 输出的内存池用于测试和验证修复到 https://bugzilla.redhat.com/show_bug.cgi?id=1134221。代码分析，找到 dict_t 对象泄漏 （对于不被 unref 有足够多次数，期间名称自愈。测试所涉及的创建 100 个平原复制卷上的文件，删除它们从一个砖的后端，然后触发上他们从装载点的查找。Statedump 装载过程被采取在执行测试用例之前和之后后编译 glusterfs 使用-DDEBUG 标志 （有冷计数被设置为 0，默认情况下）。


Statedump 输出的保险丝装载过程之前执行测试用例︰

```

池名称 = glusterfs:dict_t
热计数 = 0
冷计数 = 0
padded_sizeof = 140
alloc 计数 = 33
最大分配 = 0
池未命中 = 33
恶狗 stdalloc = 14
max stdalloc = 18

```
Statedump 输出的保险丝装载过程后执行测试用例︰

```

池名称 = glusterfs:dict_t
热计数 = 0
冷计数 = 0
padded_sizeof = 140
alloc 计数 = 2841年
最大分配 = 0
池未命中 = 2841年
恶狗 stdalloc = 214
max stdalloc = 220

```
在这里，冷的计数为 0，默认情况下，电流 stdalloc 表示被使用 mem_get()，堆中分配的 dict_t 对象的数目和尚未被释放，使用 mem_put() （https://github.com/gluster/glusterfs/blob/master/doc/data-structures/mem-pool.md 有关内存是如何工作的更多详细信息请参阅）。测试用例 （名称夏枯草的 100 个文件） 之后, 是 dict_t （14 日至 214） 的恶狗 stdalloc 值上升。


这些泄漏被固定后，用-DDEBUG 标志，再编译的 glusterfs 和相同的步骤进行再次 statedump 被之前和之后执行测试用例的山。这样做的目的是为了确定该修补程序的有效性。结果如下︰


Statedump 输出的保险丝装载过程之前执行测试用例︰

```
池名称 = glusterfs:dict_t
热计数 = 0
冷计数 = 0
padded_sizeof = 140
alloc 计数 = 33
最大分配 = 0
池未命中 = 33
恶狗 stdalloc = 14
max stdalloc = 18

```
Statedump 输出的保险丝装载过程后执行测试用例︰

```
池名称 = glusterfs:dict_t
热计数 = 0
冷计数 = 0
padded_sizeof = 140
alloc 计数 = 2837年
最大分配 = 0
池未命中 = 2837年
恶狗 stdalloc = 14
max stdalloc = 119

```
恶狗 stdalloc 的值仍然 14 之前和之后的测试，表明该修补程序的确它了该怎么办。

# # #How 如何调试挂由于帧丢失？
`https://bugzilla.redhat.com/show_bug.cgi?id=994959` is one of the bugs where statedump was helpful in finding where the frame was lost. Here is the process used to find where the hang is using statedump.
当观察到挂时，statedumps 是采取的所有进程。在山的 statedump 上显示下面的堆栈︰

```
[] global.callpool.stack.1.frame.1
ref_count = 1
翻译 = 保险丝
完成 = 0

[] global.callpool.stack.1.frame.2
ref_count = 0
翻译 = r2-客户端-1
完成 = 1 < <---客户端 xlator 完成 readdirp 调用，并展开到空燃比
父 = r2-复制-0
wind_from = afr_do_readdir
wind_to--readdirp > fops > = 儿童 [call_child]
unwind_from = client3_3_readdirp_cbk
unwind_to = afr_readdirp_cbk

[] global.callpool.stack.1.frame.3
ref_count = 0
翻译 = r2-复制-0
完成 = 0 < <---空燃比 xlator 不解除由于某种原因。
父 = r2 dht
wind_from = dht_do_readdir
wind_to = xvol-主席之友-readdirp >
unwind_to = dht_readdirp_cbk

[] global.callpool.stack.1.frame.4
ref_count = 1
翻译 = r2 dht
完成 = 0
父 = r2 io 缓存
wind_from = ioc_readdirp
wind_to=FIRST_CHILD(this)--readdirp > fops >
unwind_to = ioc_readdirp_cbk

[] global.callpool.stack.1.frame.5
ref_count = 1
翻译 = r2 io 缓存
完成 = 0
父 = r2 快速阅读
wind_from = qr_readdirp
wind_to = FIRST_CHILD （这）-主席之友-readdirp >
unwind_to = qr_readdirp_cbk

```
`unwind_to` shows that call was unwound to `afr_readdirp_cbk` from client xlator.
检查功能显示那空燃比不堆栈展开时 fop 失败。
检查 http://review.gluster.org/5531 有关修补程序/代码更改的详细信息。
