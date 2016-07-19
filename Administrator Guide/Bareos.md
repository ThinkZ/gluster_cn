# 配置 Bareos 在 Gluster 上存储备份

这说明假定您已经准备好了一个 Gluster 环境和
configured. The examples use `storage.example.org` as a Round Robin DNS name
这可以用于接触任何可用的 GlusterD 进程。的

Gluster Volume that is used, is called `backups`. Client systems would be able
要访问该卷的像这样带保险丝安装︰

# 山-t glusterfs storage.example.org:/backups 命令

[] Bareos(http://bareos.org) 包含一个插件使用的存储守护进程

`libgfapi`. This makes it possible for Bareos to access the Gluster Volumes
而不需要有一个保险丝安装可用。

在这里我们将使用一个服务器，专门做备份。该系统是

called `backup.example.org`. The Bareos Director is running on this host,
与 Bareos 存储守护进程。在示例中，还有一个文件守护进程

在同一台服务器上运行。这使得备份 Bareos

主任，是有用的 Bareos 数据库和配置的备份
保持这种方式。


# Bareos 安装

绝对最小的 Bareos 安装需要 Bareos 主任和存储
守护进程。若要备份一个文件系统，文件守护进程需要可用

太。对于本文档中的描述，使用了 CentOS 7，与

下列软件包和版本︰

-[glusterfs-3.7.4] (http://download.gluster.org)
-[bareos-14.2] (http://download.bareos.org) bareos-存储-glusterfs

Gluster 存储服务器不需要有任何安装的 Bareos 软件包。
它往往是更好地保持应用程序 (Bareos) 和存储服务器上
不同的系统。所以，当 Bareos 存储库已配置，安装

the packages on the `backup.example.org` server:

# yum 安装 bareos 主任 bareos-数据库-sqlite3 \
bareos-存储-glusterfs bareos-filedaemon。
bareos-bconsole

为了保持事情尽可能简单，SQlite 它用。为生产

建议 MySQL 或 PostgrSQL 的部署。它需要创建

初始的数据库︰

# sqlite3 /var/lib/bareos/bareos.db < /usr/lib/bareos/scripts/ddl/creates/sqlite3.sql
# chown bareos:bareos /var/lib/bareos/bareos.db
# chmod 0660 /var/lib/bareos/bareos.db

The `bareos-bconsole` package is optional. `bconsole` is a terminal application
可以用于启动备份，检查不同 Bareos 的状态
components and the like. Testing the configuration with `bconsole` is
相对简单。

一旦安装了软件包，您将需要启动和启用守护程序︰

# 立即开始 bareossd
# 立即开始 bareosfd
# 立即开始 bareosdir
# 立即启用 bareossd
# 立即启用 bareosfd
# 立即启用 bareosdir


# Gluster 容量的制备

有允许 Bareos 访问 Gluster 卷所需的几个步骤。通过

默认 Gluster 不允许客户端从一个非特权端口连接。
因为 Bareos 存储守护程序不运行，以根用户权限连接
需要打开了。

有两个过程涉及客户端访问 Gluster 卷时。为

初始阶段，GlusterD 联系，当客户端收到的布局
体积，客户端将连接到砖直接。对更改

允许非特权的进程连接，因此有两个方面︰

1. In `/etc/glusterfs/glusterd.vol` the option `rpc-auth-allow-insecure on`
需要添加所有存储服务器上。修改后的

配置文件 GlusterD 过程需要使用重新启动
   `systemctl restart glusterd`.
1 卷.砖过程是通过卷选项来配置。
   By executing `gluster volume set backups server.allow-insecure on` the
获取设置所需的选项。一些版本的 Gluster 需要卷停止/启动

该选项考虑之前，这些版本您将需要向
   execute `gluster volume stop backups` and `gluster volume start backups`.

除了网络权限，Bareos 存储守护进程需要
允许写入文件系统提供的 Gluster 卷。这是

通过设置正常 UNIX 权限/所有权等权
用户/组可以写入卷︰

# 山-t glusterfs storage.example.org:/backups 命令
# mkdir/产妇和新生儿破伤风/bareos
# chown bareos:bareos/产妇和新生儿破伤风/bareos
# chmod ug = 可/产妇和新生儿破伤风/bareos
# umount 命令

Depending on how users/groups are maintained in the environment, the `bareos`
用户和组不可能在存储服务器上可用。如果这就是

case, the `chown` command above can be adapted to use the `uid` and `gid` of
the `bareos` user and group from `backup.example.org`. On the Bareos server,
输出将类似于︰

# id bareos
uid=998(bareos) gid=997(bareos) groups=997(bareos),6(disk),30(tape)

And that makes the `chown` command look like this:

# chown 998:997/产妇和新生儿破伤风/bareos


# Bareos 配置

When `bareos-storage-glusterfs` got installed, an example configuration file
has been added too. The `/etc/bareos/bareos-sd.d/device-gluster.conf` contains
the `Archive Device` directive, which is a URL for the Gluster Volume and path
备份应得到存储位置。在我们的示例中，该条目应得到设置

自：

设备 {
名称 = GlusterStorage
存档设备 = gluster://storage.example.org/backups/bareos
设备类型 = gfapi
媒体类型 = GlusterFile
...
}

默认配置的 Bareos 提供的工作就是写到备份
`/var/lib/bareos/storage`. In order to write all the backups to the Gluster
卷相反，配置为 Bareos 主任需要修改。
In the `/etc/bareos/bareos-dir.conf` configuration, the defaults for all jobs
can be changed to use the `GlusterFile` storage:

JobDefs {
名称 ="DefaultJob"
...
# 存储 = 文件
存储 = GlusterFile
...
}

更改后的配置文件，Bareos 守护进程需要将它们应用。
最简单地通知更改的配置文件的过程是由
instructing them to `reload` their configuration:

# bconsole
连接到主任备份︰ 9101
1000 OK︰ 备份 dir 版本︰ 14.2.2 （2014 年 12 月 12 日）
输入期来取消命令。
* 重新加载

With `bconsole` it is also possible to check if the configuration has been
applied. The `status` command can be used to show the URL of the storage that
配置。当所有的设置是正确时，结果看起来像这样︰


* 状态存储 = GlusterFile
连接到存储在备份︰ 9103 GlusterFile 守护
...
"GlusterStorage"(gluster://storage.example.org/backups/bareos) 的设备未打开。
...


# 创建第一个备份

有几个默认工作配置在 Bareos 主任。其中之一

is the `DefaultJob` which was modified in an earlier step. This job uses the
`SelfTest` FileSet, which backups `/usr/sbin`. Running this job will verify if
配置工作正常。额外的工作，其他文件集和

以后可以添加更多文件守护进程 （得到备份的客户端）。

* 运行
必须指定作业名。
定义的工作资源是︰
1: BackupClient1
2: BackupCatalog
3: RestoreFiles
选择作业资源 (1-3): 1
运行备份作业
JobName: BackupClient1
水平︰ 增量
客户端︰ 备份-fd
...
好办吗？（是/国防部/否）︰ 是的

工作队列中。JobId = 1


The job will need a few seconds to complete, the `status` command can be used
to show the progress. Once done, the `messages` command will display the
结果︰

* 消息
...
JobId: 1
工作︰ BackupClient1.2015-09-30_21.17.56_12
...
终止︰ 备份好

归档文件包含该备份将位于 Gluster Volume 上。自

检查该文件是否可用，存储服务器上装入该卷︰

# 山-t glusterfs storage.example.org:/backups 命令
# ls/产妇和新生儿破伤风/bareos


# 进一步阅读

本文档打算提供一个快速启动的配置 Bareos 使用
Gluster 作为存储后端。可以配置 Bareos 创建的备份

不同的客户端 （符合运行文件守护进程），在计划的时间运行作业和
时间间隔以及更多。优秀的 [Bareos

可以咨询文件] (http://doc.bareos.org)，以找出如何
更多有用的方式比可以得到表示在此页上创建备份。
