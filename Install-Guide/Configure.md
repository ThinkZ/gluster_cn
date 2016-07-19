# # # 配置防火墙

为 Gluster 内部群集通讯或者防火墙
要关闭或启用每个服务器的通信。

		iptables -I INPUT -p all -s `<ip-address>` -j ACCEPT

# # # 配置受信任的池

请记住，信任的池是术语，用来定义的群集
在 Gluster 的节点。选择一台服务器将您的"主"服务器。这是

只是为了简单起见，将通常要运行所有命令
从这个教程。请记住，运行许多 Gluster 特定命令

(like \`gluster volume create\`) on one server in the cluster will
在所有其他服务器上执行相同的命令。

gluster 同行探针 （群集或 IP 地址，如果你没有 DNS 或 /etc/hosts 条目中的其他服务器的主机名）

Notice that running \`gluster peer status\` from the second node shows
第一个节点已添加。

# # # 分区，格式和装载砖

假设你有一块砖头在 /dev/康体发展局︰

fdisk/dev/康体发展局和创建一个分区

# # # 格式化分区

mkfs.xfs-i 大小 = 512/dev/sdb1

# # # 挂载分区作为 Gluster"砖"

mkdir-p/出口/sdb1 & & 山 /dev/sdb1/出口/sdb1 & & mkdir-p /export/sdb1/brick

# # # 添加一个条目到 /

回声"/ dev/sdb1/出口/sdb1 xfs 默认值 0 0"> > fstab/等 /

# # # Gluster 音量设置

最基本的 Gluster 卷类型 （也是一个"只分发"卷
简称"纯 DHT"卷，如果你想要给人留下深刻印象
水冷却器）。这种类型的体积只是分发数据

均匀地分布到可用砖的体积。所以，如果我写 100

文件，平均来看，五十岁终将一台服务器上和五十岁会结束
在另一个上。这是比"复制"的卷，但不是作为

受欢迎的因为它不会给你两个最追捧的特点
Gluster 的 — — 如果多个复制的数据和自动故障转移
发生错误。要设置复制的卷︰


gluster 卷创建 gv0 副本 2 node01.mydomain.net:/export/sdb1/brick node02.mydomain.net:/export/sdb1/brick

把这些分解成碎片，第一部分说打造 gluster
卷名为 gv0 (名称是任意的 gv0 选择是只是因为
它是比 gluster\_volume\_0 更少的输入）。接下来，我们告诉它要

卷复制副本卷，并保持数据的副本上至少 2
在任何给定时间的砖头。因为我们只有两个砖总，这

意味着每个服务器将房子数据的副本。最后，我们指定

哪些节点使用和在这些节点上的砖。这里的顺序是

（如最可能是重要的当你有更多砖......
编写这篇文章，Gluster 3.3 的当前版本） 来指定砖
在这样一种方式，你能让这两个副本的数据驻留在
单个节点。这会令人尴尬的解释你

老板当你防弹，完全是多余的总是对超级
单点故障发生时，群集来到嘎然而止。

现在，我们可以检查以确保按预期运行的东西︰

gluster 卷信息

您应该看到类似于以下内容的结果︰

卷名︰ gv0
类型︰ 复制
卷 ID: 8bc3e96b-a1b6-457d-8f7a-a91d1d4dc019
状态︰ 创建
砖的数目︰ 1 x 2 = 2
传输类型︰ tcp
砖头︰
Brick1: node01.yourdomain.net:/export/sdb1/brick
Brick2: node02.yourdomain.net:/export/sdb1/brick

这告诉我们，基本上是我们在该卷的过程中指定什么
创建。其中提及的"地位"。状态为"已创建"

该卷已创建，但还没有开始，就意味着
这将导致任何尝试装入卷失败。

现在，我们应该启动卷。

gluster 卷开始 gv0

找到所有文档 [这里] (.../ index.md)
