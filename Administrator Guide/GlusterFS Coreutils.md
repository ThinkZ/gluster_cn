Coreutils GlusterFS 卷
===============================
GlusterFS Coreutils 是一套实用程序，其目的是要模仿标准的 Linux coreutils，它利用 gluster C API 才能做工作的例外。它提供了一个接口类似于 ftp 程序。

操作包括从服务器获取文件到本地计算机，将文件从本地计算机上传到服务器，从服务器检索目录信息，等等之类。

# # 安装
# # # 安装 GlusterFS
Prerequsities、 指令和 GlusterFS 配置的信息，请参阅安装指南从 < http://gluster.readthedocs.org/en/latest/ >。

# # # 安装 glusterfs coreutils
现在 glusterfs coreutils 将打包只为 rpm。将很快支持其他包格式。


# # # 为 fedora
地下城与勇士/百胜用于安装 glusterfs coreutils:

地下城与勇士安装 glusterfs coreutils
或

百胜餐饮集团安装 glusterfs coreutils

# # 使用
glusterfs coreutils 提供一组基本实用程序如猫、 cp、 羊群、 ls、 mkdir、 rm、 统计和尾巴，专门为 libgfapi 使用俗称 GlusterFS API 实现的。这些实用程序可以使用 gluster 远程内

壳或作为独立的命令和 '女朋友' 前置到各自的基名称。例如，glusterfs 猫实用被命名为 gfcat，等等与羊群核心提供的实用程序的独立 gfflock 命令是不这样 （请参见备注节为什么群设计以这种方式） 的一个例外。


# # # 使用 coreutils 远程 gluster 壳内
# # # 调用一个新的 shell
为了进入一个 gluster 客户端外壳，键入 * gfcli * 并按回车键。你现在将可获赠一个类似的提示，如下所示︰


gfcli >

请参见手册页 * gfcli * 查看更多选项。
# # # 连接到一个 gluster 卷
现在我们需要为一些 glusterfs 卷，已经开始向客户端连接。使用连接命令来做到这一点，如下所示︰


gfcli > 连接 glfs: / / < 服务器 IP 或主机名 > / < VOLNAME >

例如，如果您有卷命名卷的服务器的主机名 localhost 上面的命令将采取以下形式︰

gfcli > 连接 glfs://localhost/vol

请确保你成功通过验证新的提示，这应该看起来像连接到一个远程 gluster 卷︰

gfcli (< 服务器 IP 或主机名/<VOLNAME>)
# # # 试试你最喜欢的实用程序
请去通过不同的工具的手册页和每个命令的可用选项。例如，* 人 gfcp * 将显示详细信息的外部或内部 gluster 壳的 cp 命令的用法。运行不同的命令，如下所示︰


gfcli （localhost/音量） ls。
gfcli （localhost/音量） stat.trashcan
# # # 终止客户端连接从卷
使用断开连接命令以关闭连接︰

gfcli （localhost/音量） 断开连接
gfcli >
# # # 从外壳程序退出
从外壳程序运行退出︰

gfcli > 退出

# # # 使用独立 glusterfs coreutil 命令
如上文所述 glusterfs coreutils 还提供独立的命令来执行基本的 GNU coreutil 功能。所有这些命令的前面的 '女朋友'。而不是调用 gluster 客户端壳您可以直接使用这些来建立和执行的操作在一杆。例如见 gfstat 命令的以下示例用法︰


gfstat glfs://localhost/vol/foo

还有关于群 coreutility 这是作为一个独立命令不可 '注意到' 一节所述的理由豁免。

在每个命令和相应的选项的详细信息请参阅相关的手册页。

# # 笔记
* 在 gluster 客户端壳一个特定会话，命令的历史记录将被保留，即，您可以使用上下箭头键搜索通过以前执行的命令或使用 Ctrl + R 的扭转历史搜索技术。
* 群作为独立的 'gfflock' 不可。因为锁总是与文件描述符相关联。与所有其他命令不同群不能马上清理文件描述符后获取锁。对于群我们需要保持活动连接作为 glusterfs 客户端。

