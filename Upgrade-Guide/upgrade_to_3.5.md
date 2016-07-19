# # # Glusterfs 从 3.4.x 升级到 3.5

现在，GlusterFS 3.5.0 出来了，这里是一些机制，以升级
从早些时候安装的 GlusterFS 版本。

* * 从 GlusterFS 3.4.x:** 的升级

GlusterFS 3.5.0 是与 3.4.x 兼容 （是的你看它正确 ！）。你

可以通过以下两个过程之一升级您的部署
下面。

* *) 安排停机 （推荐） * *

这种方法，安排停机时间和防止从所有客户端
访问的服务器。

如果您有配置的配额，您需要执行步骤 1 和 6，
否则你可以跳过它。

如果您有运行的土力工程处复制会话，停止会话使用
土力工程处代表停止命令 （请参阅的土力工程处 rep 升级步骤步骤 1
下面提供）

1.执行"前-升级-脚本-为-quota.sh""升级为配额的步骤"一节所述。
2.停止您服务器上的所有的 glusterd、 glusterfsd 和 glusterfs 进程。
3.安装 GlusterFS 3.5.0
4.启动 glusterd。
5.确保所有开始的卷有过程在线在"gluster 卷状态"。
6.执行"升级为配额的步骤"一节所述的"升级后脚本"。

您将需要重复这些步骤在所有服务器上该窗体你
受信任的存储池。

若要升级土力工程处复制会话，请参照地理 rep 升级
步骤如下 （从第 2 步）

升级后的服务器，它被建议升级所有客户端
安装到 3.5.0。

* * b） 滚动升级与没有停机时间 * *

如果你有复制或分发复制卷用砖
放置在冗余的正确时尚，有没有数据
自我疗愈和冒险的感觉，你可以执行滚动升级
通过下面的过程︰

注︰ 滚动升级的土力工程处复制会话从 glusterfs 版本 \ < 3.5 到 3.5.x 不受支持。

如果您有配置的配额，您需要执行步骤 1 和 7，
否则你可以跳过它。

1.执行"前-升级-脚本-为-quota.sh""升级为配额的步骤"一节所述。
2.停止您服务器上的 glusterd、 glusterfs 和 glusterfsd 的所有进程。
3.安装 GlusterFS 3.5.0。
4.启动 glusterd。
5.  Run “gluster volume heal `<volname>` info” on all volumes and ensure that there is nothing left to be self-healed on every volume. If you have pending data for self-heal, run “gluster volume heal `<volname>`” and wait for self-heal to complete.
6.确保所有开始的卷有过程在线在"gluster 卷状态"。
7.执行"升级为配额的步骤"一节所述的"升级后脚本"。

重复上面的步骤是您值得信赖的一部分的所有服务器上
存储池。

再一次升级后的服务器，它被建议升级所有
客户端安装到 3.5.0。

做报告您的发现 3.5.0 在 gluster 用户，对 Freenode \#gluster
和 bugzilla。

请注意这可能不适用于所有的安装与升级。如果

你注意到有什么不对劲，想看看它这里介绍的内容，请
点相同。

# # # * * 为配额 * * 的升级步骤

配额的升级过程涉及执行两个升级脚本︰

1.前-升级-脚本-为-quota.sh，和之间
2.后-升级-脚本-为-quota.sh

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

* 对卷"vol1"，启用配额调用脚本方式如下: *

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

这里是您现有的土力工程处复制安装升级到新的步骤
分布式的地理-glusterfs-3.5 复制。新的版本 leverges

在你的主音量的所有节点，并提供更好的性能。

注意︰

因为新版本的土力工程处代表非常不同，年长的那个，
这必须脱机完成。

新版本支持仅通过两个 gluster 卷之间同步
ssh + gluster。

这个文档处理升级土力工程处代表。所以升级卷不是

在这里详细讨论。

* * 下面是升级的步骤: * *

1.停止在旧版本中土力工程处复制会话 (\ < 3.5) 使用
以下命令

        #gluster volume geo-replication `<master_vol>` `<slave_host>`::`<slave_vol>` stop

2.现在由于新的土力工程处复制需要 gfids 的主人和奴隶
卷一样，生成一个包含所有文件的 gfids 文件
硕士

cd/usr/共享/glusterfs/脚本 /;
        bash generate-gfid-file.sh localhost:`<master_vol>` $PWD/get-gfid.sh    /tmp/master_gfid_file.txt ;
        scp /tmp/master_gfid_file.txt root@`<slave_host>`:/tmp

3.现在去奴隶主机和应用到奴隶卷 gfid。

cd /usr/share/glusterfs/scripts /
        bash slave-upgrade.sh localhost:`<slave_vol>` /tmp/master_gfid_file.txt    $PWD/gsync-sync-gfid

这会要求你输入密码的奴隶群集中的所有节点。请

如果要求为他们提供。

4.还注意到，这将重新启动您的奴隶 gluster 卷 (停止和
开始）

5.现在创建和启动奴隶主与奴隶之间的土力工程处代表会议。
有关创建新土力工程处 rep seesion 指令，请参阅
分布式地理代表管理员指南。

        gluster volume geo-replication `<master_volume>` `<slave_host>`::`<slave_volume>` create push-pem force
        gluster volume geo-replication `<master_volume>` `<slave_host>`::`<slave_volume>` start

6.现在您的会话将升级为使用分布式地理 rep
