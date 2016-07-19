此页面介绍了如何构建和安装 GlusterFS。

生成要求
------------------

下列软件包都需要建立 GlusterFS，

-GNU Autotools
-Automake
-Autoconf
-Libtool
-法 (一般 flex)
-GNU 野牛
-OpenSSL
-libxml2
-Python 2.x
-libaio
-libibverbs
-librdmacm
-readline
-lvm2
-glib2
-liburcu
-cmocka
-libacl
-sqlite

# # # Fedora

以下的 yum 命令安装所有生成所需经费
软呢帽，

# yum 安装 automake autoconf libtool flex 野牛 openssl 发展 libxml2 发展 python 开发 libaio 发展 libibverbs 发展 librdmacm 发展 readline 发展 lvm2 开发 glib2 发展用户空间区重案组发展 libcmocka 发展 libacl 发展 sqlite-发展

# # # Ubuntu

以下的 apt get 命令将安装生成的所有要求
Ubuntu，

$ sudo apt-get 来安装使 flex 野牛 pkg 配置 libssl-dev libxml2-dev python-dev libaio-dev libibverbs-dev librdmacm-dev libreadline-dev liblvm2-dev libglib2.0-dev liburcu-dev libcmocka-dev libsqlite3-dev libacl1-dev automake autoconf libtool

从源代码
--------------------

本节描述如何生成 GlusterFS 来源。它被假设

你有 GlusterFS 源文件 （无论是从一个发布的压缩文件的副本
或一个 git 克隆）。下面的所有命令都都要运行与源

工作目录的目录。

# # # 为建筑的配置

运行下面的命令一次的配置和设置生成
过程。

运行 autogen 生成配置脚本。

$./autogen.sh

一旦 autogen 成功完成生成的配置脚本。运行

要生成生成文件的配置脚本。

$./ 配置

如果已经安装了上述的生成要求，运行
配置脚本应该给下面配置摘要，

GlusterFS 配置摘要
===========================
保险丝客户︰ 是的
无限带宽动词︰ 是的
epoll 方面 IO 复用︰ 是的
argp 独立︰ 没有
fusermount︰ 是的
readline︰ 是的
georeplication︰ 是的
Linux AIO︰ 是的
启用调试︰ 没有
systemtap︰ 没有
块设备 xlator︰ 是的
glupy︰ 是的
使用系统日志︰ 是
XML 输出︰ 是的
QEMU 块格式︰ 是的
加密 xlator︰ 是的

在开发过程中是启用调试版本。为此，请运行

配置与 ' — — 启用调试标志。

$./ 配置 — — 启用调试

可以通过运行配置与发现进一步配置标志
'— — 帮助' 标志，

$./ 配置 — — 帮助

# # # 楼

一旦配置好，可以用一个简单建 GlusterFS 使命令。

$ 使

加快构建过程在多核计算机上，添加 '-jN' 标志，
其中 N 是并行作业的数量。

# # # 安装

运行使安装以安装 GlusterFS。默认情况下，GlusterFS 将

安装到 'usr/本地' 前缀。若要更改安装前缀，请给

适当的选项来配置。如果安装到默认的

前缀，你可能需要使用 'sudo' 或 ' 苏-c 来安装。

$ sudo 让安装

# # # 运行 GlusterFS

GlusterFS 可以只作为根用户运行，所以需要将以下命令
以 root 身份运行。如果你已经安装到默认 ' usr/本地 '

前缀，添加 '/ usr/局部/sbin' 和 '/ usr，本地，bin' 到您之前的路径
运行下面的命令。

通常，源安装不会安装任何 init 脚本。所以你

将需要手动启动 glusterd。若要手动启动 glusterd 只是

运行，

# glusterd

这将启动 glusterd 和叉入作为守护程序背景
过程。你现在运行 'gluster' 命令，并利用 GlusterFS。


建设的包
-----------------

# # # 建筑 Rpm

构建 Rpm 是很简单的。RPM 基于系统，为 eg。软呢帽，

来源和做的配置步骤中所示建设
来自源一节。配置步骤之后，请运行以下命令

步骤，构建 Rpm，

$ cd 临时演员/LinuxRPM
$ 使 glusterrpms

这将从源代码中 '临时演员/LinuxRPM' 创建 rpm。*(Note: You

将需要安装的 rpmbuild 的要求，包括 rpmbuild 和
嘲笑） *

构建 Rpm 的更详细的说明可在
[] CompilingRPMS(./Compiling-RPMS.md)。

