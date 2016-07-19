Bug 经常得到固定在主前释放分支。

当在主分支修复一个 bug 可能是可取或
要通过稳定分支的修复程序。

此页用于帮助组织支持 （和优先顺序）
反向 bug 修复的对社会的重要性。

# # # GlusterFs 3.6

Backports 要求 3.6.0
-----------------------------

3.6.0 跟踪 bug:
< > https://bugzilla.redhat.com/show_bug.cgi?id=glusterfs-3.6.0

请添加 ' glusterfs-3.6.0' '块' 字段中的 bug 建议
在 GlusterFS 3.6.0 列入。

# # # GlusterFs 3.5

3.5.3 要求 Backports
-----------------------------

当前 [计划的 bug 列表
列入] (https://bugzilla.redhat.com/showdependencytree.cgi?hide_resolved=0&id=glusterfs-3.5.3)。

-文件反向 3.5.3 一个补丁的新 bug:
[< https://bugzilla.redhat.com/enter_bug.cgi?product=GlusterFS&blocked=glusterfs-3.5.3&version=3.5.2&short_desc=backport%20request%20for%20 >......
新 glusterfs 3.5.3 通过请求]

# # # GlusterFs 3.4

3.4.6 要求 Backports
-----------------------------

3.4.6 跟踪 bug:
< > https://bugzilla.redhat.com/show_bug.cgi?id=glusterfs-3.4.6

请添加 ' glusterfs-3.4.6' '块' 字段中的 bug 建议
在 GlusterFS 3.4.6 列入。

< > https://bugzilla.redhat.com:443/show_bug.cgi?id=1116150
< > https://bugzilla.redhat.com:443/show_bug.cgi?id=1117851

3.4.4 要求 Backports
-----------------------------

< https://bugzilla.redhat.com/show_bug.cgi?id=859581 >-"自我修复
过程有时可以创建目录，而不是符号链接的
.glusterfs 中的根 gfid 文件"

< https://bugzilla.redhat.com/show_bug.cgi?id=1041109 >-"结构需要
清洗"消息出现在访问文件时。

< https://bugzilla.redhat.com/show_bug.cgi?id=1073023 >-glusterfs 装载
坠机后删除砖，分离同行和终止

3.4.3 要求 Backports
-----------------------------

< https://bugzilla.redhat.com/show_bug.cgi?id=859581 >-"自我修复
过程有时可以创建目录，而不是符号链接的
.glusterfs 中的根 gfid 文件"

< https://bugzilla.redhat.com/show_bug.cgi?id=1041109 >-"结构需要
清洗"消息出现在访问文件时。

< https://bugzilla.redhat.com/show_bug.cgi?id=977492 >-大 NFS 写道
对 Gluster 慢下来，然后停下来

< https://bugzilla.redhat.com/show_bug.cgi?id=1073023 >-glusterfs 装载
坠机后删除砖，分离同行和终止

3.3.3 要求 Backports
-----------------------------

[默认情况下启用 fusermount，使夜间 autobuilding
工作] (https://bugzilla.redhat.com/1058666)

3.4.2 要求 Backports
-----------------------------

请输入 bugzilla ID 或这里修补 URL:

1） 直到 RDMA 处理改进，我们应输出一个警告时
使用 RDMA 卷-
< > https://bugzilla.redhat.com/show_bug.cgi?id=1017176

2) 无法收缩没有数据损失-卷
< > https://bugzilla.redhat.com/show_bug.cgi?id=1024369

3） 群集双氢睾酮︰ 允许非本地客户端功能与 nufa 卷。
-< http://review.gluster.org/5414 >

3.4.1 要求 Backports
-----------------------------

请输入 bugzilla ID 或修补 URL 在这里。

< https://bugzilla.redhat.com/show_bug.cgi?id=812230 >-"配额上下文
未设置在 inode"

< https://bugzilla.redhat.com/show_bug.cgi?id=893778 >-"NFS 崩溃的错误"

谁评论此列表说明︰ 这些都是问题的修复程序
在我们的生产造成了实际的服务中断
安装，因而对我们来说绝对必需 (— — 斯洛伐克
) Rintel:

< https://bugzilla.redhat.com/show_bug.cgi?id=994392 >-"设置 ACL
glusterfs 3.4.0 条目失败"

< https://bugzilla.redhat.com/show_bug.cgi?id=991622 >-"fd 泄漏
在"打开隐藏"卷选项设置为运行 dbench 时观察到
"on"复制卷上"

这些都是我们在 git 日志审查期间遇到了问题和
那看起来足够可怕我们挑选他们为了避免风险，
尽管没有实际上被击中。希望，可以帮助决定是否有

樱桃采摘他们值得 (— — 斯洛伐克 Rintel):

< https://bugzilla.redhat.com/show_bug.cgi?id=961691 >"CLI 崩溃
执行"gluster 对等地位"命令"

< https://bugzilla.redhat.com/show_bug.cgi?id=965995 >"快速阅读和
打开隐藏 xlator︰ 使选项 (volume\_options) 结构为 NULL
终止。

< https://bugzilla.redhat.com/show_bug.cgi?id=958691 >"nfs 根南瓜︰
重命名上创建文件内设置粘滞位驻留文件
目录"

< https://bugzilla.redhat.com/show_bug.cgi?id=982919 >"双氢睾酮︰ 文件
存储在目录没有散列范围 （哈希布局）"

< https://bugzilla.redhat.com/show_bug.cgi?id=976189 >"statedump 崩溃
在 ioc\_inode\_dump"

< https://bugzilla.redhat.com/show_bug.cgi?id=982174 >"cli 崩溃时
设置 diagnostics.client 日志级别设置为跟踪"

< https://bugzilla.redhat.com/show_bug.cgi?id=989579 >"glusterfsd 崩溃
smallfile 基准"

< http://review.gluster.org/5821 >，"测试︰ 在年底叫 '清理'
每个测试"，< https://bugzilla.redhat.com/show_bug.cgi?id=1004756 >，
983975 backport

< http://review.gluster.org/5822 >，"glusterfs api.pc.in 包含
rpath"，< https://bugzilla.redhat.com/show_bug.cgi?id=1004751 >，通过
1002220

< http://review.gluster.org/5824 >"glusterd.service （舰碰撞），确保
glusterd 开始之前任何本地 gluster 挂载"，
< https://bugzilla.redhat.com/show_bug.cgi?id=1004796 >，通过的
1004795

< https://bugzilla.redhat.com/show_bug.cgi?id=819130 > 元，检查
glusterfs.spec.in 具有所有相关更新

< https://bugzilla.redhat.com/show_bug.cgi?id=1012400 >-Glusterd 会
不存储的所有卷时领先同行设置全局选项
拒绝

请求的 Backports
-------------------

-请通过 [gfapi︰ 关闭日志文件 fd 和初始化为 NULL
在 glfs\_fini] (http://review.gluster.org/#/c/6552) 到 3.5 版
-完成
-请通过 [群集/dht︰ 确保 loc 具有
gfid] (http://review.gluster.org/5178) 到释放-3.4
-请通过 [Bug 887098] (http://goo.gl/QjeMP) 成释放 3.3
(FyreFoX)-完成
-请通过 [Bug 856341] (http://goo.gl/9cGAC) 成释放-3.2
和释放 3.3 (-我 o/电动 Debian) 半熟的释放-3.3
-请通过 [Bug 895656] (http://goo.gl/ZNs3J) 成释放-3.2
释放-3.3 （x4rlos，意指）-做为释放 3.3
-请通过 [Bug 918437] (http://goo.gl/1QRyw) 成释放 3.3
(tjstansell)-完成
-请通过成 [Bug
884597] (https://bugzilla.redhat.com/show_bug.cgi?id=884597)
释放-3.3 (nocko)-完成

解决的 bug
----------------

-[Bug 838784] (https://bugzilla.redhat.com/show_bug.cgi?id=838784)
-[Bug 893778] (https://bugzilla.redhat.com/show_bug.cgi?id=893778)
-[Bug 913699] (https://bugzilla.redhat.com/show_bug.cgi?id=913699);
可能与有关 [Bug
884597] (https://bugzilla.redhat.com/show_bug.cgi?id=884597)
