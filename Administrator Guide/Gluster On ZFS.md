在 ZFS 上 Gluster
--------------

这是一整套分步说明安装 Gluster 在 ZFS 作为后备文件存储。有一些命令特有的我的安装，具体而言，ZFS 调优一节。* Moniti estis.*


制备
-----------

-安装 CentOS 6.3
-   Assumption is that your hostname is `gfs01`
-作为 root 用户运行的所有命令
-百胜更新
-禁用 IP 表

    `chkconfig iptables off`
    `service iptables stop`

-禁用 SELinux
-编辑 /etc/selinux/config
-设置 SELINUX = 禁用
-重新启动

在 Linux 上安装 ZFS
--------------------

-为 RHEL6 或 7 和衍生工具，你可以安装 ZFSoL 回购 （和座谈） 和使用，要安装 ZFS

    ```sh
RHEL 6:
    $ sudo yum localinstall --nogpgcheck `[`https://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`](https://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm)
    $ sudo yum localinstall --nogpgcheck `[`http://archive.zfsonlinux.org/epel/zfs-release.el6.noarch.rpm`](http://archive.zfsonlinux.org/epel/zfs-release.el6.noarch.rpm)
$ sudo 百胜安装内核发展 zfs
    
RHEL 7:
    $ sudo yum localinstall --nogpgcheck `[`https://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm`](https://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm)
    $ sudo yum localinstall --nogpgcheck `[`http://archive.zfsonlinux.org/epel/zfs-release.el7.noarch.rpm`](http://archive.zfsonlinux.org/epel/zfs-release.el7.noarch.rpm)
$ sudo 百胜安装内核发展 zfs
    ```

并跳到下面的完成 ZFS 配置。

或者你可以滚你自己如果你想要特定的修补程序︰

    `yum groupinstall "Development Tools"`

-下载 & 解压最新的 SPL 和 ZFS tarball 从 [zfsonlinux.org] (http://www.zfsonlinux.org)

# # # 安装 DKMS

我们希望当我们升级内核，所以你一定要与在 Linux 上的 ZFS DKMS 自动重建的内核模块。

-下载最新的 RPM，从 < http://linux.dell.com/dkms >
-安装 DKMS
rpm-Uvh dkms*.rpm

# # # 建造与安装 SPL

-输入 SPL 源目录
-以下命令创建两个源 & 三个二进制 Rpm。 删除静态模块 RPM （我们使用 DKMS），并安装其余︰

    ```sh
./ 配置
让 rpm
rm spl-模块-0.6.0*.x86_64.rpm
rpm-Uvh spl*.x86_64.rpm spl*.noarch.rpm
    ```

# # # 建造与安装 ZFS

-* 如果您计划使用 ' xattr = sa' 文件系统选项，请确保您有修为 < https://github.com/zfsonlinux/zfs/issues/1648 > ZFS * * 所以你符号链接不会 corrupted.* * * (适用于 ZFSoL 之前 0.6.3，xattr = sa 是安全使用在 0.6.3 及更高版本)
-输入 ZFS 源目录
-以下命令创建两个源 & 五个二进制 Rpm。 删除静态模块 RPM 和安装剩余部分。请注意我们有几个初步的包，来之前我们可以编译安装。


    ```sh
百胜餐饮集团安装 zlib 发展 libuuid 发展 libblkid 发展 libselinux 发展分手 lsscsi
./ 配置
让 rpm
rm zfs-模块-0.6.0*.x86_64.rpm
rpm-Uvh zfs*.x86_64.rpm zfs*.noarch.rpm
    ```

# # # 完成 ZFS 配置

-重新启动以使所有更改生效，如果需要
-   Create ZFS storage pool. This is a simple example of 4 HDDs in RAID10. NOTE: Check the latest [ZFS on Linux FAQ](http://zfsonlinux.org/faq.html) about configuring the /etc/zfs/zdev.conf file. You want to create mirrored devices across controllers to maximize performance. Make sure to run `udevadm trigger` after creating zdev.conf.

    ```sh
存储池创建-f sp1 A0 B0 镜子 A1 B1
zpool 状态 sp1
df-h
    ```

-您应该会看到 /sp1 装入点
-启用 ZFS 压缩以节省磁盘空间︰

    `zfs set compression=on sp1`

-可以 lz4 压缩使用 ZFS 的后期版本上，因为它可以更快，特别是为不可压缩的工作负载。它是安全地改变这一飞，ZFS 将压缩新数据与当前设置︰


    `zfs set compression=lz4 sp1`

-设置 ZFS 的可调参数。这是特定于我的环境。

-将设置为 33%和 75%的已安装的 RAM 的最大的电弧缓存分钟。因为这是一个专用的存储节点，走不开了。在我的例子我的服务器有 24 G 的 RAM。更多的 RAM 是更好地与 ZFS。

-我们使用 SATA 驱动器不接受命令标签队列，因此设置为 1 的 min 和 max 挂起的请求
-禁用读预取，因为它是几乎完全无用的没有任何在我们的环境，但不必要工作的驱动器。我看到 \<10%预取缓存命中，所以它真的不是必需，实际上伤害了性能。

-将事务组超时设置为 5 秒钟，以防止出现由于写入大批量冻结该卷。5 秒是默认的但安全迫使这。

-忽略客户端刷新/同步命令;让 ZFS 这处理的事务组超时时间冲洗。注意︰ 需要 UPS 备份解决方案，除非你不介意失去这 5 秒价值的数据。


    ```sh
回声"选项 zfs zfs_arc_min = 8 G zfs_arc_max = 18 G zfs_vdev_min_pending = 1 zfs_vdev_max_pending = 1 zfs_prefetch_disable = 1 zfs_txg_timeout = 5"> /etc/modprobe.d/zfs.conf
重新启动
    ```

-将 acltype 属性设置为 posixacl 指示应使用 Posix Acl。

    `zfs set acltype=posixacl sp1`

安装 GlusterFS
-----------------

    ```sh
    wget -P /etc/yum.repos.d `[`http://download.gluster.org/pub/gluster/glusterfs/LATEST/EPEL.repo/glusterfs-epel.repo`](http://download.gluster.org/pub/gluster/glusterfs/LATEST/EPEL.repo/glusterfs-epel.repo)
百胜餐饮集团安装 glusterfs {-保险丝-服务器}
服务 glusterd 开始
服务 glusterd 状态
chkconfig glusterd 上
    ```

-继续你政府飞行服务队同行探头、 创建卷，& c。
-向山 GFS 卷自动重新启动后，将这些行添加到 /etc/rc.local (假设您 gluster 卷被称为"出口"，您所需的装入点是/导出︰

    ```sh
# 山 GFS 卷
装载-t glusterfs gfs01: / 出口/导出
    ```

宋人笔记 & TODO
--------------------------

# # # 日常电子邮件状态报告

Python 脚本源;把你需要的电子邮件地址放在 'toAddr' 变量。添加一个 crontab 条目以运行此日报。


	```sh
# ！ / usr/bin/python
导入日期时间、 套接字、 smtplib，子进程
def doShellCmd(cmd):
subproc = 子进程。Popen (cmd，壳 = True，stdout = 子进程。管）

cmdOutput=subproc.communicate() [0]
返回 cmdOutput
hostname=socket.gethostname()
statusLine ="现状"+ 主机名 +"at"+ str(datetime.datetime.now())
zpoolList = doShellCmd （zpool 列表）
zpoolStatus = doShellCmd （zpool 状态）
zfsList = doShellCmd ('zfs list')
报告 = (statusLine +"\n"+
"-----------------------------------------------------------\n" +
zfsList +
"-----------------------------------------------------------\n" +
zpoolList +
"-----------------------------------------------------------\n" +
) zpoolStatus
fromAddr ="从︰ 根 @"主机名 +"\r\n"
toAddr ="到︰ user@your.com \r\n"
主题 ="主题︰ ZFS 状态从"主机名 +"\r\n"
msg = （主语 + 报告）
服务器 = smtplib。SMTP('localhost')

server.set_debuglevel(1)
server.sendmail （fromAddr、 toAddr、 味精）
server.quit()
    ```

# # # 从 ZFS 快照还原文件

显示哪个节点文件是 （对于从 ZFS 快照还原文件）︰

     `getfattr -n trusted.glusterfs.pathinfo <file>`

# # # 定期 ZFS 快照

因为社区网站不会让我实际上与 Akismet 垃圾阻塞后由于一些随机的错误脚本，我就会相反帖子链接。

-[定期 ZFS 快照] (http://community.spiceworks.com/how_to/show/15303-recurring-zfs-snapshots)
-或使用 < https://github.com/zfsonlinux/zfs-auto-snapshot >
