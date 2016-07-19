仲裁者卷和 gluster 的仲裁选项
=============================================
仲裁者卷是特殊子集的副本 3 卷，旨在
防止分裂大脑和提供相同的一致性保证作为一种常态
副本 3 卷而无需消耗 3 x 空间。之前我们看看如何创建一个

和如何他们工作等，值得详述的裂类型
在 gluster 的说法和如何仲裁服务器和客户端仲裁等功能帮助
减少的发生一定程度的分裂 — — 大脑。

在副本卷的分裂 — — 大脑
================================
当文件处于裂，有不一致的地方，在任何数据或
元数据 （权限、 uid/gid、 扩展的属性等）。其中的文件

副本的砖 * 和 * 我们并没有足够的信息对权威
选择作为原始副本和愈合到坏的副本，尽管所有砖块
正在启动并联机。对于那里的目录，

也是在它里面的文件有不同的 gfids 条目裂脑 /
文件类型 （比如一个是文件，另一个是具有相同名称的目录）
对面的一个副本的砖块。

这 [文件] (https://github.com/gluster/glusterfs-specs/blob/master/done/Features/heal-info-and-split-brain-resolution.md)
描述如何解决在裂使用 gluster cli 的文件或
装入点。几乎总是，分裂大脑发生由于网络断开连接 （在何处

客户端暂时失去连接到砖） 和很少由于
gluster 砖过程下去或者返回一个错误信息。

服务器仲裁和一些陷阱
================================
这 [文件] (http://www.gluster.org/community/documentation/index.php/Features/Server-quorum)
提供此功能的详细的说明。
服务器仲裁卷选项有︰

> Option:cluster.server-仲裁-比率
值描述︰ 0 到 100
 
> Option:cluster.server-仲裁-类型
值描述︰ 无 |服务器                                                 

如果设置为服务器，此选项允许指定的卷参与服务器端的法定人数。
如果设置为无，那孤独的容量不考虑体积检查。

Cluster.server 与仲裁的比例是百分比数字和是群集范围内 — — 即
你不能在同一受信任池中有不同配比的不同卷。

对于两个节点可信的存储池很重要的是要将此值设置
大于 50%，不能两者都相信彼此分开的两个节点
他们同时已足够法定人数。2 节点平原副本卷，这将

意思是两个节点都需要启动并运行。所以是没有 HA/故障切换的概念。


有用户从 2 节点与同行探讨创建副本 2 卷
'假' 节点没有砖和启用服务器仲裁比率为 51%。
这并不会阻止文件进入裂。例如，如果 B1

B2 是副本的工作做一个砖节点和 B3 是虚拟节点，我们可以
最终仍然会裂像这样︰

1.B1 下山，B2 和 B3 都起来。仲裁服务器仍然是。修改文件

由客户端。
2.B2 下山，B1 回来。遇到了服务器仲裁。相同的文件被修改

由客户端。
3.我们现在有不同内容的文件在 B1 和 B2 ==> 裂。

在作者看来，服务器仲裁是有用的如果你想要避免分裂大脑
对卷配置跨节点而不是 I/O 路径。
不同于在客户端-仲裁在哪里该卷将变为只读时仲裁丢失损失
中的特定节点的服务器仲裁使 glusterd 杀死在那砖过程
节点 （参加卷） 甚至使读取是不可能。

客户端仲裁
==============
客户端法定人数是实现在空燃比，防止分裂 — — 大脑处于 I/O 功能
对于分布式复制/复制卷的路径。默认情况下，如果客户端仲裁

不满足的特定副本成因类型属次，它将变为只读。其他 subvols

（dist rep 卷） 将仍然在 R/W 访问。

以下的卷设置的选项用于配置它︰
> 选项︰ cluster.quorum 型
默认值︰ 无
值描述︰ 无 | 汽车 | 固定
如果设置为"固定"，此选项允许写入文件只有当数目
积极砖的那副本集 （文件所属的） 是更大
或等于计数 ' 仲裁计数选项中指定。
如果此选项设置为"自动"，允许对文件的写操作只当数目
上升的砖 > = 细胞 （的砖构成该副本/2 总数）。
如果复制副本的数量是偶数，则进一步检查︰
如果砖的数目是正好等于 n/2，然后第一砖必须
是到了砖之一。如果它是超过 n/2 然后它不是

必要的第一块砖是向上砖之一。
    
> 选项︰ cluster.quorum 计数
> 值描述︰
必须积极地复制副本设置为允许的砖数目写道。
结合 cluster.quorum 型使用此选项 * = 固定 * 选项
若要指定多少砖要积极参加仲裁。
如果仲裁类型是自动此选项已没有意义。
                                                       
> 选项︰ cluster.quorum-读取
默认值︰ 无
值描述︰ 是 | 无
如果仲裁读取设置为 'yes' (或 '真实' 或 '的) 然后将允许甚至读取
只有满足了仲裁，而读 （写） 将返回 ENOTCONN。
如果设置为 no (或 false 或关闭)，然后读取将送达仲裁时甚至
不满足，但是写操作将失败，并。


副本 2 和副本 3 卷
===============================
从上面的描述，很明显该客户端仲裁真的不能应用
到副本 2 卷:(without costing HA)。
如果仲裁类型设置为自动，然后由说明
鉴于早些时候，最早的砖必须永远都可走，不论的地位
第二个砖。换句话说，如果只有第二个砖，成因类型属次变得下，即没有医管局。

如果仲裁类型设置为固定，仲裁计数 * 有 * 有两个
为了防止分裂大脑。（否则写可以成功的 brick1，另一个在 brick2 => 裂）。

所以对于所有实际的目的，如果你想在副本 2 卷，卷中的高可用性
这被建议不启用客户端法定人数。

在副本 3 卷，客户端仲裁是默认启用，并设置为自动。
这意味着 2 砖需要进行写入操作成功。这里是如何这

配置可以防止文件结束了在裂︰

说的 B1、 B2、 B3 是砖︰
1.B3 是下来，仲裁满足，写发生在 B1 和 B2 上的文件
2.B3 上来，B2 是下来，再次达到法定人数，写发生在 B1 和 B3。
3.B2、 B1 下山，符合法定人数。现在当发出写操作时，看到了空燃比

那 B2 和 B3 的挂起 xattrs 相互指责对方，因此写不
允许和与 EIO 失败。

还有一例角落甚至与副本 3 卷文件在哪里
最终在裂。空燃比通常需要范围锁为 {偏移量、 长度}

写。如果 3 写发生在同一个文件在非重叠 {抵消，长度}

和每个写入操作失败 （只有） 一个不同的砖，然后我们有的空燃比 xattrs
互相指责对方的文件。

仲裁配置
======================
仲裁配置又称仲裁卷是完美的甜蜜点
副本 2 路和 3 路副本以避免文件之间进入裂，
没有 x 存储空间 * * * 正如前面提到的 3。
用于创建卷的语法是︰
> #gluster 卷创建 <VOLNAME>副本 3 仲裁 1 host1:brick1 host2:brick2 host3:brick3

例如︰
> #gluster 卷创建 testvol 副本 3 仲裁 1 127.0.0.2:/bricks/brick{1..6} 力
卷创建︰ testvol︰ 成功︰ 请开始访问数据卷

> #gluster 卷信息
卷名︰ testvol
类型︰ 分布式复制
卷 ID: ae6c4162-38c2-4368-ae5d-6bad141a4119
状态︰ 创建
砖的数目: (2 + 1) x 2 = 6
传输类型︰ tcp
砖头︰
Brick1: 127.0.0.2:/bricks/brick1
Brick2: 127.0.0.2:/bricks/brick2
Brick3: 127.0.0.2:/bricks/brick3 （仲裁）
Brick4: 127.0.0.2:/bricks/brick4
Brick5: 127.0.0.2:/bricks/brick5
Brick6: 127.0.0.2:/bricks/brick6 （仲裁）
重新配置的选项︰
transport.address 家族︰ inet
performance.readdir 前方︰ 对 '

请注意每个副本成因类型属次三砖被指定为仲裁砖。
此砖将存储只有文件/目录的名称 （即树状结构）
和扩展属性 （元数据），但没有任何数据。即，文件大小

(as shown by `ls -l`) will be zero bytes. It will also store other gluster 
像.glusterfs 文件夹及其内容的元数据。自仲裁卷

也是一种类型的副本 3 卷，卷客户端 quourm 默认启用的和
设置为自动。此设置是 * * 不 * * 将被改变。


#Arbiter brick(s) 大小︰
由于仲裁砖不存储文件数据，其磁盘使用量将大大
较少比其他砖块的副本。砖的大小将取决于

多少个文件你计划要存储在该卷中。一个好的估计会

4 kb 次副本中的文件数。

它是如何工作的︰
-------------
有两个组件到仲裁卷。一个是仲裁 xlator

加载过程中，砖每隔两个 （即仲裁者） 砖。另一种是

仲裁逻辑本身是存在于空燃比 (复制 xlator) 加载
对客户端。

作为一种 '过滤器' 翻译为主席之友 — — 即它允许先前的行为
输入操作打 posix，阻止像某些 inode 操作
读 （展开的呼叫 ENOTCONN） 和展开其他 inode 操作
喜欢写，没有缠绕到 posix 成功截形等。

答案是后者。即目前在空燃比的仲裁逻辑执行以下任务︰


-充分文件锁时写入一个文件而不是范围锁定
正常的复制卷。这可以防止角-描述的情况裂

较早前的 3 种方式的副本。

仲裁中的卷允许未行为写主席之友在一起
与客户端法定人数可归纳为以下步骤︰

-如果所有 3 砖 （快乐案件），然后是一个没有问题和主席之友被允许。

-如果 2 砖了，如果其中之一是仲裁者 （即第三砖） * 和 *
它指责其他砖头为一个给定的文件，然后所有的写，主席之友将会失败
与 ENOTCONN。这是因为在这种情况下，只有真实的副本上

已关闭的砖。因此我们不能允许写入，直到那块砖头也是了。

如果仲裁者不会责怪其他砖，主席之友将允许继续。
'怪' 这里是 w.r.t 值的空燃比的更新日志扩展属性。

-如果 2 砖是一种向上和仲裁服务器已关闭，则将允许主席之友。
当仲裁时，条目/元数据发生治愈它。当然数据

治愈，则不需要。

-如果只有一个砖了，然后客户端仲裁不满足和容量变得。

-在所有情况下，如果只有一个源之前发起 FOP
（即使所有砖块都了） 如果 FOP，则该源，会失败
应用程序将接收 ENOTCONN。例如，假定写入失败对 B2

和 B3，即 B1 是唯一的来源。现在如果出于某种原因，第二个写

失败对 B1 （之前没有机会为夏枯草完成尽管所有砖
正在了），应用程序将接收失败 (ENOTCONN) 为这写。
 

砖头是向上或向下上述不一定意味着砖
进程处于脱机状态。它也可以指装载到砖连接丢失

由于网络断开连接等。
