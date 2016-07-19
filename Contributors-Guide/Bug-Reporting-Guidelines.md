前提交 bug
-------------------

如果你发现任何问题，这些作为有用的初步检查︰

-   Is SELinux enabled? (you can use `getenforce` to check)
-   Are iptables rules blocking any data traffic? (`iptables -L` can
帮助检查）
-是的所有节点可以到达从彼此吗？[网络问题]

-请请搜索 Bugzilla 看到如果 bug 已经报道
-选择 GlusterFS 作为"产品"，然后键入一些东西
有关在"词"框。如果您看到崩溃或中止，

寻找中止消息的一部分可能是有效的。如果

你感觉冒险你可以选择"高级的搜索"
选项卡;这使更多的控制，但不是更好的

发现现有的 bug。
-如果 bug 已已经申请特定版本和你
在另一个版本中，发现 bug
-请克隆对现有的 bug 进行释放，你发现
问题。
-如果现有 bug 是反对主线和你发现
为释放，然后克隆的 bug 问题 * 取决于 * 应
将设置为 BZ 主线 bug。

任何人都可以在 Bugzilla 中搜索，您不需要的帐户。搜索

需要一些努力，但有助于避免重复，和你可能会发现，
你的问题已经被解决了。

报告了一个错误
---------------

-你应该有一个 Bugzilla 帐户
-这里是链接到文件的 bug:
[] Bugzilla() https://bugzilla.redhat.com/enter_bug.cgi?product=GlusterFS

-模板提交 bug 可以发现 [
*here*](./Bug-Reporting-Guidelines.md)

* 注︰ 请去通过所有以下部分，以了解什么
我们需要放在一个错误的信息。因此，它将帮助开发人员

根原因并修复它 *

# # # 所需的信息

你应该收集以下信息之前创建的 bug 报告。

# # # 包信息

-从中使用的包的位置
-包信息-glusterfs 软件包安装的版本

# # # 群集信息

-在群集中的节点数
-主机名和 ip 地址 gluster 节点 [如果不是安全的
问题]
-主机名 / IP 将帮助开发人员在理解 &
相关日志
-   Output of `gluster peer status`
-节点 IP，从中做"x"操作
-"x"在这里意味着任何操作导致的问题

# # # 卷信息

-卷的数量
-卷名
卷的特定问题看到 [如适用]
-类型的卷
如果可用容量选项
-   Output of `gluster volume info`
-   Output of `gluster volume status`
-得到带有问题的卷的 statedump

`   $ gluster volume statedump `<vol-name>

This dumps statedump per brick process in `/var/run/gluster`

* 注︰ 收集从一个 gluster 节点中 directory.* statedumps

在所有节点包含卷的砖中重复。所有所以

收集的目录存档、 压缩和附加到 bug

# # # 砖信息

-xfs 时做砖分区选项
-这可使用此命令︰

`   $ xfs_info /dev/mapper/vg1-brick`

的砖上扩展属性
-这可使用此命令︰

`   $ getfattr -d -m. -ehex /rhs/brick1/b1`

# # # 客户端信息

-OS 类型 （Windows、 RHEL）
-OS 版本︰ 在 Linux 发行版得到如下︰

`   $ uname -r`\
`   $ cat /etc/issue`

-保险丝或装载命令在客户端与输出 NFS 挂载点
-   Output of `df -Th` command

# # # 工具信息

-如果任何工具用于测试，提供关于它工作做一个信息版本
-如果任何 IO 模拟使用脚本，提供脚本

# # # 日志信息

-您可以检查日志中的问题，警告，错误检查。
-自我修复日志
-平衡日志
-Glusterd 日志
-砖日志
-NFS 日志 （如果适用）
-Samba 登录 （如果适用）
客户端装载日志
-添加整个日志作为附件，如果它很大，无法粘贴作为
评论

# # # CentOS/Fedora 的 SOS 报告

-获得的 sosreport 从涉及 gluster 节点和客户端 [在
案中的 CentOS /Fedora]
-一个有意义的名称 ip 地址为 sosreport，通过重命名/添加添加
主机名/ip 为 sosreport 的名称
