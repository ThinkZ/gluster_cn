现在，GlusterFS 3.7.0 出来了，这里是从升级的过程
较早前已安装的 GlusterFS 版本。请阅读整个 howto

之前升级您的部署

# # # 升级前

默认情况下，GlusterFS 包含从 3.6.0 afrv2 执行。如果你是

使用 GlusterFS 复制 (\ < 3.6) 在您的设置，请注意
新的 afrv2 执行只是兼容 3.6 或更高版本
GlusterFS 客户端。如果你不更新您的客户端 GlusterFS

3.6 版本与您的服务器需要禁用客户端
自我疗愈过程在升级之前。您可以执行这由下面

步骤。

[root@~]# gluster v 设置 testvol cluster.entry 自我治愈了
卷组︰ 成功
[root@~]#
[root@~]# gluster v 设置 testvol cluster.data 自我治愈了
卷组︰ 成功
[root@~]# gluster v 设置 testvol cluster.metadata 自我治愈了
卷组︰ 成功
[root@~]#

# # # GlusterFS 升级到 3.7.x

* *) 调度停机时间 * *

这种方法，安排停机时间和防止从所有客户端
访问 (umount 您的卷，停止 gluster 卷......等） 的服务器。


1.停止您服务器上的所有的 glusterd、 glusterfsd 和 glusterfs 进程。
2.安装 GlusterFS 3.7.0
3.启动 glusterd。
4.确保所有开始的卷有过程在线在"gluster 卷状态"。

您将需要重复这些步骤在所有服务器上该窗体你
受信任的存储池。

升级后的服务器，它被建议升级所有客户端
安装到 3.7.0

* * b） 滚动升级 [待定] * *

# # # 从 3.4.x 升级到 3.7.X 特别注意事项

如果你有配额或土力工程处复制在 3.4.x 中配置，请阅读
下面。否则，你可以跳过本节。


在介绍了配额及土力工程处复制体系结构的更改
Gluster 3.5.0。因此安排停机时间升级建议

从 3.4.x 到 3.7.x 如果您有启用这些功能。

# # # * * 为配额 * * 的升级步骤

配额的升级程序包括以下内容︰

1.运行前-升级-脚本-为-quota.sh
2.升级到 3.7.0
2.运行后-升级-脚本-为-quota.sh

有关脚本的详细信息都作为下。

* 升级前的脚本: *

它是什么做的︰

预升级脚本 (前-升级-脚本-为-quota.sh) 遍历
有配额的卷列表启用和配置捕获
根据文件中的每个此类卷的配额限制
通过执行 /var/tmp/glusterfs/quota-config-backup/vol\_\ <VOLNAME\>
对其中每个配额列表命令。

运行升级前脚本的先决条件︰

1.使确定 glusterd 和进程正在运行的所有节点的砖
在群集中。
2.预升级脚本必须在升级之前运行。
3.预升级脚本必须在唯一的中的节点上运行
群集。

位置︰

必须从源代码树检索前-升级-脚本-为-quota.sh
下的额外的目录。

调用︰

Invoke the script by executing \`./pre-upgrade-script-for-quota.sh\`
从任一群集中的节点上壳。

-示例︰

[root@server1 extras]#./pre-upgrade-script-for-quota.sh

* 升级后脚本: *

它是什么做的︰

升级后的脚本 (后-升级-脚本-为-quota.sh) 镐
已启用配额的卷。

因为群集必须运行在 op-版本 3 的配额工作，
'默认-软-限制' 这些卷的每个设置为 80%（其中
is its default value) via \`volume set\` operation as an explicit
要撞 op 版本的群集，也触发了触发器
重写的 volfiles 就会敲击配额关闭客户端的卷文件。

一旦做到这一点，这些卷被开始有力地使用 \'volume
启动 force\' 启动配额守护进程的所有节点上。

此后，为每个这些卷、 路径和限制
其上配置从备份文件进行检索
/var/tmp/glusterfs/quota-config-backup/vol\_\ <VOLNAME\> 及限制
set on them via the \`quota limit-usage\` interface.

注意︰

In the new version of quota, the command \`quota limit-usage\` will fail
如果上哪个配额限制的目录是为一个给定的卷设置
不存在。因此，建议您创建这些

目录前运行后-升级-脚本-为-quota.sh 如果你
想要在这些目录上设置的限制。

运行升级后脚本的先决条件︰

1.升级后的脚本必须执行后中的所有节点
群集已经升级。
2.此外，访问给定的卷的所有客户端也必须
升级之前运行该脚本。
3.使确定 glusterd 和进程正在运行的所有节点的砖
在升级后的群集中。
4.该脚本必须从同一节点运行在升级前
在运行脚本。

位置︰

有额外下可以找到后-升级-脚本-为-quota.sh
glusterfs 的源树的目录。

调用︰

后-升级-脚本-为-quota.sh 采用一个命令行参数。这

参数可以是以下之一: ' 的卷的名称，
已启用配额;或 '' '所有' '


在第一种情况下，调用后-升级-脚本-为-quota.sh 从
每个卷配额启用，用的卷的的名称与的壳
作为命令行参数传递︰

-示例︰

对于卷"vol1"在其启用配额，以下列方式调用该脚本︰
      
[root@server1 extras]#./post-upgrade-script-for-quota.sh vol1

在第二种情况下，升级后的脚本拿自己，
卷上的配额启用，并执行升级后
在他们每个人的过程。在这种情况下，调用

后-升级-脚本-为-quota.sh 从壳作为 '都' 传递
在命令行参数︰

-示例︰

[root@server1 extras]#./post-upgrade-script-for-quota.sh 所有

注意︰

在第二种情况下后,-升级-脚本-为-quota.sh 过早退出
对更换故障时任何给定的卷。在这种情况下，您可以运行

后-升级-脚本-为-quota.sh 单独 （使用卷名
命令行参数） 在此卷上，也出现的所有卷上
after this volume in the output of \`gluster volume list\`, that have
启用配额。

在 /var/tmp/glusterfs/quota-config-backup 下备份的文件是
在升级后的程序供参考后予以保留。

# # # * * 升级为土力工程处复制步骤: * *

新版本支持仅通过两个 gluster 卷之间同步
ssh + gluster。

' 下面是升级的步骤。''


1.停止在旧版本中土力工程处复制会话 (\ < 3.5) 使用
以下命令

<master_vol> <slave_host>::<slave_vol> #gluster 卷土力工程处复制停止

2.现在由于新的土力工程处复制需要 gfids 的主人和奴隶
卷一样，生成一个包含所有文件的 gfids 文件
硕士

cd/usr/共享/glusterfs/脚本 /;
bash 生成 gfid file.sh localhost: <master_vol> $PWD/得到-gfid.sh /tmp/master_gfid_file.txt;
scp /tmp/master_gfid_file.txt 根 @<slave_host>: / tmp

3.奴隶群集安装升级至 3.7.0

4.现在转到奴隶主机并将 gfid 应用于奴隶卷。

cd /usr/share/glusterfs/scripts /
bash 奴隶 upgrade.sh localhost: <slave_vol> /tmp/master_gfid_file.txt $PWD/gsync-同步-gfid

这会要求你输入密码的奴隶群集中的所有节点。请

如果要求为他们提供。此外注意到，这将重新启动你的奴隶

gluster 卷 （停止和启动）

5.将主群集升级到 3.7.0

6.现在创建和启动奴隶主与奴隶之间的土力工程处代表会议。
有关创建新土力工程处代表会议的说明，请参阅
管理员指南 》 中的分布式地理代表章。

<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制创建推送 pem 力
启动的全球环境展望-复制卷 gluster <master_volume> <slave_host>::<slave_volume>

在这一点上，应该配置您的分布式的地理复制
适当地。
