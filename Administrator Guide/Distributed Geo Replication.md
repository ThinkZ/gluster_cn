# 分布式地理复制在 glusterfs-3.5

这是被释放的 glusterfs-3.5 的新 dustributed-地理-复制的管理操作指南

# # # 注意︰
这篇文章被针对用户/管理员想试试新的土力工程处复制，不用多深入到塔内件和使用的技术。

# # # 如何是从早期的土力工程处复制不同？

-到目前为止，在全球环境展望-复制中，唯一的主音量中的节点将参加土力工程处复制。这意味着，数据同步照顾只由一个节点群集中的其他节点将坐空闲 （不参与数据同步）。与分布式-地理-复制主音量的每个节点需要 repsonsibility 的同步在该节点中的数据。在复制配置，其中之一会 ' Active'ly 同步数据，而其他节点的副本对会 '被动'。在 '被动' 节点只有变得活跃的时候 '积极' 双下山。这种方式新土力工程处代表利用该卷中的所有节点和删除同步从一个单一节点的瓶颈。

-新变化检测机制是其他已有新的 geo rep 改善的东西。到目前为止全球环境展望-rep 用于抓取通过 glusterfs 文件系统，找出需要同步的文件。因为爬网的文件系统可以是一个昂贵的操作，这曾经是性能的主要瓶颈。与分布式地理-销售代表，需要同步的所有文件是通过更改日志 xlator 都确定的。更新日志 xlator 期刊 modifes 文件和这些期刊然后消费由土力工程处代表到有效标识的文件落物保护结构，需要同步。

-一种新的同步方法焦油 + ssh，介绍了改善性能的几个特定的数据集。您可以切换 rsync 和焦油 + ssh 通过 CLI 到套件的方法同步您的数据设置需求。这焦油 + ssh 更适合于有大量小文件的数据集。



# # # 使用分布式地理-复制︰

# # # 的先决条件︰
-之间应该有密码的 ssh 设置主音量到奴隶卷中的一个节点中的至少一个节点。土力工程处代表创建命令应该从这个节点具有无密码的 ssh 执行安装程序以奴隶。


-不同于以前的版本，奴隶 * * 必须 * * 是 gluster 卷。奴隶可以不是一个目录。同时应创建并开始创建土力工程处代表会议之前的主人和奴隶的卷。


# # # 创建秘密 pem 酒馆文件
-执行以下命令从哪里你设置密码的 ssh 节点以奴隶。这将创建秘密 pem 酒馆文件，会在主音量的 RSA 密钥的所有节点的信息。土力工程处代表创建时执行命令，glusterd 使用此文件来建立土力工程处代表特定的 ssh 连接

```sh
gluster 系统︰ 执行 gsec_create
```

# # # 创建地理复制会话。
创建一个 geo rep 会话之间主设备和从设备的容量，使用下面的命令。执行此命令 <slave_host> 命令中指定的节点应该少 ssh 设置它们之间的密码。推 pem 选项实际上使用先前创建的秘密 pem 酒馆文件，建立了土力工程处代表特定密码少 ssh 之间在主人对奴隶的每个节点中的每个节点。

```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制创建推 pem [力]
```

如果奴隶卷中总的可用大小小于大师的总大小，该命令将引发错误消息。在这种情况下可以使用 '强制' 选项。


在用例就是在主音量节点 rsa 密钥分配给奴隶节点通过外部代理和奴隶侧核查喜欢︰
-如果 ssh 22 端口是打开的奴隶
-具有适当无密码 ssh 登录设置
-奴隶卷创建并为空
-如果奴隶有足够的内存
是照顾由外部代理，下面的命令可以用于创建地理复制︰
```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制创建无验证 [力]
```
主节点 rsa 密钥分发给奴隶节点在这种情况下不会发生和上文提到的奴隶不进行验证，这两件事要照顾 externaly。

# # # 创建非根土力工程处复制会话

`mountbroker` is a new service of glusterd. This service allows an
非特权的进程拥有 GlusterFS 装载通过注册一个标签
（和 DSL （领域特定语言） 选项） 与通过 glusterd
glusterd volfile。使用 CLI，您可以发送到 glusterd 到装入请求

收到的已装入卷的别名 （符号链接）。

从代理的请求，对弱势群体的奴隶代理使用
mountbroker 服务的 glusterd 设置为辅助 gluster 山
在这个特殊的环境，以确保代理只是代理
允许访问提供行政的特殊参数以
对特定卷的级别访问。

* * 到安装辅助 gluster 山为代理 * *:

1.  In all Slave nodes, Create a new group. For example, `geogroup`

2.  In all Slave nodes, Create a unprivileged account. For example, ` geoaccount`. Make it a member of ` geogroup`

3.  In all Slave nodes, Create a new directory owned by root and with permissions *0711.* For example, create mountbroker-root directory `/var/mountbroker-root`

4.  In any one of Slave node, Run the following commands to add options to glusterd vol file(`/etc/glusterfs/glusterd.vol`
    in rpm installations and `/usr/local/etc/glusterfs/glusterd.vol` in Source installation.

    ```sh
gluster 系统︰ 执行 mountbroker 选择 mountbroker 根 /var/mountbroker-root
gluster 系统︰ 执行 mountbroker 选择土力工程处复制日志组 geogroup
gluster 系统︰ 执行 mountbroker 选择 rpc-身份验证-允许-不安全上
    ```

5.在任何一个奴隶节点，将 Mountbroker 用户添加到 glusterd vol 文件使用，

    ```sh
gluster 系统︰ 执行 mountbroker 用户 geoaccount slavevol
    ```

Where `slavevol` is Slave Volume name.

如果您承载多个奴隶卷上的奴隶，为每个并向 volfile 使用添加下列选项

    ```sh
gluster 系统︰ 执行 mountbroker 用户 geoaccount2 slavevol2
gluster 系统︰ 执行 mountbroker 用户 geoaccount3 slavevol3
    ```

若要添加多个卷，每个 mountbroker 用户，

    ```sh
gluster 系统︰ 执行 mountbroker 用户 geoaccount1 slavevol11、 slavevol12、 slavevol13
gluster 系统︰ 执行 mountbroker 用户 geoaccount2 slavevol21，slavevol22
gluster 系统︰ 执行 mountbroker 用户 geoaccount3 slavevol31
    ```

6. Restart `glusterd` service on all the Slave nodes

7.安装 passwdless 从主节点之一 SSH 用户对奴隶节点之一。例如，到 geoaccount。


8.土力工程处复制之间创建关系主设备和从设备向用户通过在主节点上运行以下命令︰
例如，

    ```sh
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制创建推 pem [力]
    ```

9. In the slavenode, which is used to create relationship, run `/usr/libexec/glusterfs/set_geo_rep_pem_keys.sh` as a root with user name, master volume name, and slave volume names as the arguments.

    ```sh
/usr/libexec/glusterfs/set_geo_rep_pem_keys.sh <mountbroker_user> <master_volume> <slave_volume>
    ```

注意︰

Mountbroker 土力工程处代表会议删除时，下面的命令可用于删除卷每个 mountbroker 用户。
如果要删除的卷是最后的一个 mountbroker 用户，还会删除用户。

若要删除 mountbroker 用户，每卷

    ```sh
gluster 系统︰ 执行 mountbroker volumedel geoaccount2 slavevol2
    ```
若要删除多个卷，每个 mountbroker 用户，

    ```sh
gluster 系统︰ 执行 mountbroker volumedel geoaccount2 slavevol1，slavevol2
    ```
# # # 配置元卷

注意︰ 如果共享元卷是已经创建和安装在 '/ var/运行/gluster/shared_storage' 作为 nfs 或快照的一部分，请跳到配置元卷与土力工程处复制一节。

3 方式复制常见 gluster 元卷应配置和共享的 nfs、 快照和土力工程处复制。元卷的名称应该是 'gluster_shared_storage' 和应安装在 ' / var/运行/gluster/shared_storage /'。


元卷需要与土力工程处复制更好地处理配置
重命名和土力工程处复制期间砖低头其他一致性问题
主音量配置与 EC(Erasure Code)/统率时的场景

* * 配置元卷的步骤如下: * *

3 方式复制的元卷群集中的创建主与所有三个砖块从不同的节点，如下所示。

gluster 卷创建 gluster_shared_storage 副本 3 <host1>:<brick_path> <host2>:<brick_path> <host3>:<brick_path>
    
开始元卷，如下所示。

gluster 卷开始 <meta_vol>

装入元卷，如下所示在主的所有节点。

装载-t glusterfs <master_host>: gluster_shared_storage /var/run/gluster/shared_storage

# # #Configure 元卷与土力工程处复制会话

<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置 use_meta_volume true

如果 Mountbroker 设置，

<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置 use_meta_volume true

# # # 开始土力工程处代表会议
还有该命令从以前的版本，这个版本没有变化。

    ```sh
启动的全球环境展望-复制卷 gluster <master_volume> <slave_host>::<slave_volume>
# 如果 Mountbroker 设置，
启动的全球环境展望-复制卷 gluster <master_volume> <mountbroker_user>@<slave_host>::<slave_volume>
    ```

此命令实际上启动会话。意思 gsyncd 监视器进程将会启动，反过来产生 gsync 工人处理任何需要的时候。这同时也启用了更改日志 xlator （如果不在上的状态已经），其中开始录制每个 glusterfs 砖上的所有更改。而如果在土力工程处代表启动过程中，大师是空的变化检测机制将更新日志。否则它会的 xsync （所做的更改由爬行通过文件系统标识）。后来当初始数据是 syned 到奴隶，变化检测机制将被设置为更新日志


# # # 的土力工程处复制状态

gluster 现在已经变形的状态命令。

    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制状态
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制状态
    ```

这将显示从每个砖的主人到奴隶节点每个砖的会话的状态。

如果你想更详细的状态，然后运行状态详细信息'

    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制状态详细信息
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制状态详细信息
    ```

此命令显示额外信息，比如，总的文件同步，需要同步，删除挂起等的文件。

# # # 停止土力工程处复制会话

此命令停止所有土力工程处代表涉及处理即 gsyncd 监视器和工作流程。请注意，更改日志将会 * * 不 * * 关闭与此命令。


    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制停止 [力]
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制停止 [力]
    ```

Force 选项是使用，当一个节点 （或在节点之一 glusterd） 已关闭。一旦停止，会话可以重新启动任何时间。请注意，在重新启动的会话，时变化检测机制将回退到 xsync 模式。发生这种情况，即使你有更新日志生成期刊，而在土力工程处代表停止会话。


# # # 删除土力工程处复制会话

现在您可以删除 glusterfs 土力工程处代表会议。这将删除与土力工程处 rep 会话相关联的所有配置数据。


    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制删除
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制删除
    ```

这将删除在每个节点的所有 gsync conf 文件。这将返回失败，如果任何节点已关闭。还有与土力工程处代表停止，不同 '强制' 与此选项。


# # # 更改的配置值

有一些可以使用 CLI 更改的配置值。你可以看到所有的当前配置值与以下命令。


    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置
    ```

但你可以检查只有其中之一，像 log_file 或改变探测器

    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置日志文件
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置日志文件

<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置变化探测器
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置变化探测器

<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置工作-迪尔
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置工作-迪尔
    ```

若要设置一个新值，对此，只是提供一个新的值。请注意，并非所有配置值都允许更改。一些不能修改。


    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置变化探测器 xsync
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置变化探测器 xsync
    ```

请确保您提供正确的值的配置值。如果你有大量的小文件的数据集，那么您可以使用 tar + ssh 作为同步方法。请注意，是否 geo rep 会话正在运行，这将重新启动 gsyncd。


    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置使用 tarssh true
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置使用 tarssh true
    ```

这些值重置为默认也是简单的。

    ```sh
<master_volume> <slave_host>::<slave_volume> gluster 卷土力工程处复制配置 \!use-tarssh
# 如果 Mountbroker 设置，
<master_volume> <mountbroker_user>@<slave_host>::<slave_volume> gluster 卷土力工程处复制配置 \!use-tarssh
    ```

这使得配置密钥 (焦油-ssh 在这种情况下) 回退到它的默认值。
