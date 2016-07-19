#Puppet Gluster
<!---
GlusterFS 模块由詹姆斯 ·
Copyright (C) 2010-2013年 + 詹姆斯舒
由詹姆斯 · 舒 < james@shubin.ca > 写

此程序是免费的软件︰ 你可以将它重新分发和/或修改
它根据 GNU Affero 通用公共许可证发布的条款
自由软件基金会，任一版本 3 的许可证，或
（在您的选择） 任何更新的版本。

此程序被分布在希望它会是有益的
但没有任何担保。没有甚至隐含的担保

适销性或适合特定目的。 请参见

GNU Affero 通用公共许可证有关更多详细信息。

您应该已经收到一份 GNU Affero 通用公共许可证
随着此程序。 如果不是，请参阅 < http://www.gnu.org/licenses/ >。

-->
# #A GlusterFS 傀儡模块由 [詹姆斯] (https://ttboj.wordpress.com/)
从 # # #Available:
# # # [https://github.com/purpleidea/puppet-gluster/] (https://github.com/purpleidea/puppet-gluster/)

# # #Also 索取︰
# # # [https://forge.gluster.org/puppet-gluster/] (https://forge.gluster.org/puppet-gluster/)

# # #Table 的内容

1.[Overview](#overview)
2.[模块描述-什么模块 does](#module-description)
3.[设置-入门 Puppet-Gluster](#setup)
* [傀儡 Gluster 是什么能]？(#what-能-木偶-gluster-管理)

* [简单 setup](#simple-setup)
* [弹性 setup](#elastic-setup)
* [先进的 setup](#advanced-setup)
4.[用法/常见问题-管理与常见问题的 questions](#usage-and-frequently-asked-questions) 札记
5.[参考-类和类型 reference](#reference)
* [gluster::simple](#glustersimple)
* [gluster::elastic](#glusterelastic)
* [gluster::server](#glusterserver)
* [gluster::host](#glusterhost)
* [gluster::brick](#glusterbrick)
* [gluster::volume](#glustervolume)
* [gluster::volume::property](#glustervolumeproperty)
6.[实例-示例 configurations](#examples)
7.[限制-傀儡版本、 操作系统的兼容性等。......]() #limitations

8.[发展-模块 development](#development) 的背景
9.[作者-作者和联系 information](#author)

# #Overview

傀儡 Gluster 模块安装、 配置和管理 GlusterFS 群集。

##Module 描述

这个傀儡 Gluster 模块处理安装、 配置和管理
GlusterFS 所有群集中的主机。

# #Setup

# # #What 可以傀儡 Gluster 管理？

傀儡 Gluster 被设计为能够管理尽可能多或尽可能少的你
GlusterFS 群集如你所愿。所有功能都是可选的。如果有一项功能

这似乎不是可选的和你认为它应该是，请让我
知道。话虽如此，它很好对我意义有傀儡 Gluster 管理

尽可能多的您作为它的 GlusterFS 基础结构可以。目前，它不能

机架式新服务器，但我接受资金，探索此功能;)在

它可以管理的时刻︰

* GlusterFS 软件包 (rpm)
* GlusterFS 配置文件 （/var/lib/glusterd/）
* GlusterFS 主机眯着眼睛看 （gluster 同行探头）
* GlusterFS 存储分区 (fdisk)
* GlusterFS 存储格式 (mkfs)
* GlusterFS 砖创作 (mkdir)
* GlusterFS 服务 (glusterd)
* GlusterFS 防火墙 （白名单）
* GlusterFS 创建卷 （gluster 卷创建）
* GlusterFS 卷状态 （开始/停止）
* GlusterFS 卷属性 （gluster 卷集）
* 和更多...

# # #Simple 安装程序

包括:: gluster::simple' 是足以让你启动并运行。当使用

gluster::simple 类，或使用任何其他傀儡 Gluster 配置，
必须在群集中的所有主机上使用相同的定义。最简单的

做到这一点的方式是使用像一个单一的共享的傀儡主机定义︰

```puppet
节点 / ^ annex\d + $/ {# 附件 {1，2，......N}

类 {':: gluster::simple':
}
}
```

如果你想要在不同的参数中传递，您可以指定它们在类中
以前你提供您的主机︰

```puppet
类 {':: gluster::simple':
=> 2，副本
=> ['volume1'、 'volume2'、 'volumeN']，卷
}
```

# # #Elastic 安装程序

Gluster::elastic 类尚不可用。敬请关注 ！


# # #Advanced 安装程序

有些系统管理员可能希望手动详细列举每个所需
傀儡 Gluster 部署的的组件。发生这种情况会自动与

更高层次的模块，但可能仍然会是一个可取的特点，特别是
非弹性储存池在哪里配置预计不会改变
很多 （如果有的话）。

要放在一起您的群集一块一块的你必须手动包括和
定义每个类，并键入您想要使用。如果有某些方面

你想自己管理，你可以忽略它们从您的配置。
请参见 [reference](#reference) 下面部分的具体细节。这里有一个

可能的例子︰

```puppet
类 {':: gluster::server':
shorewall => true，
}

gluster::host {'annex1.example.com':
# uuidgen 用于使这些
uuid => ' 1f660ca2-2 c 78-4aa0-8f4d-21608218c69c'，
}

# 注意这在你现有的文件系统上使用的文件夹...
# 这可用于原型 gluster 使用虚拟机
# 如果这不是一个单独的分区，记住你根 fs 将
# 运行空间不足，当你 gluster 卷 ！
gluster::brick {'annex1.example.com:/data/gluster-storage1':
areyousure => true，
}

gluster::host {'annex2.example.com':
# 注意︰ 指定主机 uuid 是现在可选的 ！
# 如果你没有作出选择，其中一个将被分配
=> ' 2fbe6e2f-f6bc-4c2d-a301-62fa90c459f8'，#uuid
}

gluster::brick {'annex2.example.com:/data/gluster-storage2':
areyousure => true，
}

$brick_list = [
' annex1.example.com:/data/gluster-storage1'，
' annex2.example.com:/data/gluster-storage2'，
]

gluster::volume {'examplevol':
=> 2，副本
=> $brick_list，砖
我开始写这 # undef => 开始自己
}

必须是 # namevar: <VOLNAME>#<KEY>
gluster::volume::property {'examplevol#auth.reject':
=> ['192.0.2.13'，'198.51.100.42'、 '203.0.113.69']、 价值
}
```

##Usage 和常见问题

所有的管理应通过操纵上相应的参数
傀儡 Gluster 类和类型。因为某些操作是不

然而，可能与傀儡-Gluster，或不支持的 GlusterFS，尝试
操纵傀儡配置不受支持的方式将导致
未定义的行为，并可能甚至会丢失数据，然而这是不太可能。

# # #How 更改复制副本计数？

你必须在创建卷之前进行设置。这是一个限制的 GlusterFS。

在某些情况下，在此对话框中可以通过添加更改复制副本计数
现有的砖多指望得到这个预期的效果。这种情况下

不是支持的傀儡 Gluster。如果您想要使用傀儡 Gluster

之前和/或这转型后，你可以这么做，但你必须做
手动进行更改。

# # #Do 我需要使用一个虚拟的 ip 地址吗？

作为分布式的锁管理器强烈建议使用虚拟 IP (VIP)
(DLM)，还为您的客户提供高可用性 (HA) IP 地址
将连接到。推理的更详细的解释，请参阅︰


[] https://ttboj.wordpress.com/2012/08/23/how-to-avoid-cluster-race-conditions-or-how-to-implement-a-distributed-lock-manager-in-puppet/() https://ttboj.wordpress.com/2012/08/23/how-to-avoid-cluster-race-conditions-or-how-to-implement-a-distributed-lock-manager-in-puppet/


请记住，即使您使用托管的解决方案 （如 AWS)，不
提供一个额外的 IP 地址，或者你想要避免使用额外的 IP，
和你没事不具有完整的医管局客户端安装，您可以使用未使用
作为 DLM VIP 专用 RFC1918 IP 地址。记住，第 3 层 IP 可以

在与第 3 层网络所使用的相同的层 2 网络上共存
您的群集。

# # #Is 可能让木偶 Gluster 完成在一个单一的运行？

号这是木偶，限制，有关如何 GlusterFS 运作。

例如，它不是可靠地可以预测哪些端口特定
GlusterFS 卷将运行直到后卷启动。其结果是，

本模块最初将白名单连接从 GlusterFS 主机的 ip 地址
地址，然后进一步限制这一次只允许单个端口
了解此信息。这是可能的在一起

[傀儡-shorewall](https://github.com/purpleidea/puppet-shorewall) 模块。

你应该注意到每次运行应完成没有错误。如果你看

错误，它的意思，要么是与您的系统错误和/或
配置，或因为在木偶 Gluster 的 bug。

# # #Can 这结合流浪吗？

不直到流浪汉正确支持 libvirt/KVM。无意使用

VirtualBox 的乐趣。

# # #Awesome 的工作，但它的功能和/或平台失踪的支持 ！

因为这是一个开源 / 自由软件项目，我也给走了
免费 （如啤酒，自由在免费、 免费象 libre），我不能提供
无限的支持。请考虑捐赠资金、 硬件、 虚拟机、

和其他资源。为特定的需求，你也许可以赞助功能 ！


# # #You 没有回答我的问题，有问题 ！

与我联系通过我 [技术博客] (https://ttboj.wordpress.com/contact/)
和我会尽我所能帮助。如果你有一个好的问题，请提醒我

添加到此文档我答案 ！

# #Reference
请注意，有大量的无证的选项。更多

有关这些选项的信息，请查看在源︰
[] https://github.com/purpleidea/puppet-gluster/(https://github.com/purpleidea/puppet-gluster/)。

如果你觉得我们很好的利用的选项需要在这里记录，请与我联系。

# # #Overview 的类和类型

* [gluster::simple](#glustersimple)︰ 简单的傀儡 Gluster 部署。
* [gluster::elastic](#glusterelastic)︰ 下施工。
* [gluster::server](#glusterserver)︰ 服务器主机的基类。
* [gluster::host](#glusterhost)︰ 每个参与的主机的主机类型。
* [gluster::brick](#glusterbrick)︰ 砖类型为每个定义的砖，每个主机。
* [gluster::volume](#glustervolume)︰ 为每个卷类型定义卷。
* [gluster::volume::property](#glustervolumeproperty)︰ 管理为每个卷的属性。

# gluster::simple
这是 gluster::simple。它可能应该照顾所有的用例的 80%。

它是用于部署快速测试集群尤其有用。它使用

有限状态机 (FSM) 来决定群集已结算时和卷
创建可以开始了。有限状态机在木偶 Gluster 的详细信息请参阅︰

[] https://ttboj.wordpress.com/2013/09/28/finite-state-machines-in-puppet/() https://ttboj.wordpress.com/2013/09/28/finite-state-machines-in-puppet/


####`replica`
复制副本计数。不能更改自动在初始部署之后。


####`volume`
卷名或要创建的卷名称的列表。

####`path`
每个主机的的有效砖路径。默认为本地文件系统。如果你需要

每个主机的不同路径，然后 Gluster::Simple 将不满足您的需求。

####`vip`
要用于群集的虚拟 IP 地址分布式锁管理器。

####`shorewall`
布尔值，指定是否应使用傀儡 shorewall 一体化，或不。

# gluster::elastic
根据施工。

# gluster::server
群集的主服务器类。当建筑 GlusterFS 必须包括

手动的群集。包装类如 [gluster::simple](#glustersimple)

这包括自动。

####`vip`
要用于群集的虚拟 IP 地址分布式锁管理器。

####`shorewall`
布尔值，指定是否应使用傀儡 shorewall 一体化，或不。

# gluster::host
群集的主要主机类型。每个参加 GlusterFS 的主机

群集必须定义此类型本身，和每个其他主机上。其结果是，

这不是一个单例像 [gluster::server](#glusterserver) 类。

####`ip`
指定使用该主机的 IP 地址。这将默认为

_ $:: ipaddress_ 变量。一定要手动设置这，如果你要声明这

自己不使用导出的资源每个主机上。如果每个主机认为

其他主机应该有相同的 IP 地址本身，然后傀儡 Gluster 和
GlusterFS 不会正常工作。

####`uuid`
通用唯一标识符 (UUID) 的主机。如果为空，傀儡 Gluster

将此自动生成的主机。您可以生成你自己

手动与 _uuidgen_，并设置他们自己。特别是发现了这个

有用的测试，因为我会挑容易识别 UUID 像︰
_aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa_，
_bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb_，等等。如果您手动设置的 UUID，

傀儡 Gluster 有机会运行，然后它会记住您的选择，和
存储本地如果你不再指定 UUID 再次使用它。这是

对于升级现有的非托管的 GlusterFS 安装特别有用
到木偶 Gluster 托管人，而无需更改任何 UUID。

# gluster::brick
群集的主要砖类型。每个砖是一个单独的存储部分，

在主机上使用。每台主机必须有至少一个砖，参加

群集，但通常主机将有多个砖。一块砖头可以很简单

作为一个文件系统文件夹，也可以是一个单独的文件系统。请阅读

正式的 GlusterFS 文件，如果你不完全满意
砖的概念。

对于大多数测试集群和实验，它是最容易使用
根文件系统上的目录。您甚至可以使用 _ / tmp_ 子文件夹，如果你

不在乎你的数据持久化。对于更严重的集群，你

可能需要创建单独的文件系统为您的数据。自托管的铁，

这并非罕见，创建多个 RAID 6 驱动器池，然后创建
每个虚拟驱动器的单独的文件系统。然后可以用作每个文件系统

单砖。

所以，GlusterFS 的每个卷有增长，无最大能力
在木偶 Gluster 砖是一种不必单独分区存储，
其实文件夹 （无论后备存储您祝福，愿您） 然后包含
子文件夹 — — 一个为每个卷。因此上的所有卷给定

GlusterFS 群集可以共享的总的可用存储空间。如果你希望

限制使用的每个卷的存储空间，您可以设置配额。或者，你

可以买到更多的硬件，和弹性增长您的 GlusterFS 卷，因为
每 GB 价格将明显小于任何专有存储系统。
这种砖共享的一个缺点是，如果你选择了砖
每主机计数，专为满足您的性能要求，和
同一群集中的每个 GlusterFS 卷已大大不同砖每
主机性能要求，那么这不会适合您的需要。我很怀疑

任何人其实有这样的要求，但如果你坚持要上需要这
条块分割，那么你大概可以使用傀儡 Gluster 分组
功能来完成这一目标。请让我知道你的使用情况，和

被警告的分组功能还没有被广泛的测试。

为了证明你在乎的是自动化，这种类型提供的能力，
自动分区和格式化您的文件系统。这意味着您可以插入

在新的铁，引导、 调配和自动配置整个系统。
遗憾的是，我没有很多测试硬件要经常使用此功能。
如果你想要捐一些，我会很乐意彻底测试这。有说

，我已经使用此功能，我认为它是非常安全的并具有
永远不会使我丢失的数据。如果你不确定，随时看看

代码，或避免完全使用此功能。如果你认为能使

然后，它甚至更安全，随时让我知道。

####`dev`
块设备，如 _ / dev/sdc_ 或 _ / dev/磁盘/由-id/scsi-0123456789abcdef_。通过

默认情况下，木偶 Gluster 将假定你正在使用一个文件夹来存储砖
数据，如果你不指定此参数。

####`fsuuid`
文件系统的 UUID。这将确保我们可以识别不同的文件系统。你

可以设置这用自动文件系统的创建，或者您可以指定
文件系统你想要使用的 UUID。

####`labeltype`
只有 _gpt_ 被支持。其他选项包括 _msdos_，但这根本不是

由于它的大小限制使用。

####`fstype`
这应该是 _xfs_ 或 _ext4_。建议使用 _xfs_，但 _ext4_ 也是

很常见。这只会影响获得由这一个文件系统

模块。如果你提供一个新的机器，_ext4_，根文件系统和

您创建的砖是根文件系统路径，那么此选项不执行任何操作。

####`xfs_inode64`
使用 _xfs_ fstype 时设置 _inode64_ 装载选项。选择等到设置。


####`xfs_nobarrier`
使用 _xfs_ fstype 时设置 _nobarrier_ 装载选项。选择等到设置。


####`ro`
是否应挂载文件系统只读。只用于紧急状况。


####`force`
如果等到，这将覆盖它看到任何 xfs 文件系统。这是有用的

反复重建 GlusterFS 和擦除数据。有其他安全

停止这的地方。一般情况下，您可能不想碰你的东西。


####`areyousure`
你想允许傀儡 Gluster 做危险的事情吗？您必须设置

这对等到允许傀儡 Gluster _fdisk_ 和 _mkfs_ 您的文件系统。

# gluster::volume
群集的的主要卷类型。这是大量的魔法发生的地方。

请记住，更改这些参数的一些卷后
创建不会工作，，你会体验未定义的行为。可能有

基于 FSM 的错误检查，以验证未发生任何更改，但它一直存在
出去，这个代码库最终可以支持这种变化，并且，以便
如果他们知道它是安全地这样做，用户可以手动更改参数。

####`bricks`
砖要用于此卷的列表。如果这离开时的默认值

等到，然后此列表是自动生成的。确定该算法

这项命令不支持所有可能的情况，以及最有可能不能
处理某些角落案件。它是可以检查 FSM 以查看

所选的砖订单在它之前有机会创建卷。该卷

创建脚本不会运行，直到有稳定砖列表一样被 FSM
在有 DLM 的主机上运行。如果您指定此列表的砖

手动，你必须选择的顺序，以匹配您所需的卷布局。如果你

不能确定如何订购砖，你应该审查 GlusterFS
文件第一次。

####`transport`
只有 _tcp_ 被支持。可能的值可以包含 _rdma_，但这不会改变

如果我不能无限带宽硬件访问任何测试。捐赠的欢迎。


####`replica`
复制副本计数。通常你会想要将此设置为 _2_。有些用户选择 _3_。

其他值都是少见的。_1_ 值可以用于简单测试

分布式安装程序，当你不关心你的数据或高可用性。A

值大于 _4_ 是可能浪费和不必要。甚至可能会

如果同步写正在等待缓慢的第四次，导致性能问题
服务器。

####`stripe`
条纹计数。彻底不支持和未经测试的选项。不推荐

使用由 GlusterFS。

####`ping`
我们想要包括 _fping_ 坪支票吗？

####`settle`
我们想运行解决检查吗？

####`start`
该卷的请求的状态。有效值包括︰ 等到 （开始），_false_

（停止） 或 _undef_ （未托管的启停状态）。

# gluster::volume::property
群集的主音量属性类型。这允许您管理 GlusterFS

卷的特定属性。有种类繁多的属性那卷

支持。对于属性的完整列表，您应该参考 GlusterFS

help_ 命令设置文档或运行 _gluster 卷。若要设置属性

您必须使用特殊名称模式的︰ _volume_ #_key_。价值的争论是

用于设置关联的值。它是足够聪明，接受中的值

该特定属性的最合乎逻辑格式。某些属性尚未

支持，所以请报告你使用这一功能有任何问题。
因为，因为此功能是令人敬畏到 _document 作为 code_ 卷
具体优化你曾经犯过，请确保您使用此功能即使
你不要使用所有其他人。

####`value`
要用于此卷的属性的值。

# #Examples
例如，配置，请参阅 [例子 /] 在 git 中的 (https://github.com/purpleidea/puppet-gluster/tree/master/examples) 目录
来源资料库。它是可用的︰


[] https://github.com/purpleidea/puppet-gluster/tree/master/examples() https://github.com/purpleidea/puppet-gluster/tree/master/examples


它也是可用的︰

[] https://forge.gluster.org/puppet-gluster/puppet-gluster/trees/master/examples() https://forge.gluster.org/puppet-gluster/puppet-gluster/trees/master/examples/


# #Limitations

本模块已经测试针对开放源傀儡 3.2.4 和更高。

模块上进行了测试︰

* 6.4 centOS

它可能会方面的工作没有发生任何事件或未经主要修改︰

* CentOS 5.x/6.x
* RHEL 5.x/6.x

它最有可能将工作与其他傀儡版本和在其他平台上，但
在其他条件下测试一直光由于缺乏资源。它将

最有可能不工作在 Debian/Ubuntu 系统而无需修改。我会

实在很想添加支持这些操作系统的系统，但没有任何
测试资源这样做。请赞助这如果你想要看到它发生。


# #Development

这是我个人的项目，我在我的空闲时间工作。
接受捐赠的资金、 硬件、 虚拟机和其他资源都是
表示赞赏。请与我联系，如果你想要赞助一项功能，请我去

教谈话/或咨询。

你可以遵循 [上我技术博客] (https://ttboj.wordpress.com/)。

# #Author

Copyright (C) 2010-2013年 + 詹姆斯舒

* [github] (https://github.com/purpleidea/)
* [@purpleidea] (https://twitter.com/# ！ / purpleidea)
* [https://ttboj.wordpress.com/] (https://ttboj.wordpress.com/)

