静态代码分析工具
--------------------------

所报告的问题的 bug 修复 * 静态代码分析工具 * 应
请按照 [发展工作 Flow](./Development-Workflow.md)

# # # Coverity

GlusterFS 是 [Coverity 的] (https://scan.coverity.com/) 扫描的一部分
程序。

-为看到 Coverity 问题你要的 GlusterFS 成员
在 Coverity 扫描网站项目。
-这里是链接到 [Coverity 扫描
网站] (https://scan.coverity.com/projects/987)
-转到链接上方，GlusterFS 项目 （如订阅
贡献者）。包括你在它会向管理员发送请求

该项目。
-一旦 GlusterFS Coverity 扫描管理员批准您的请求，
你将能够看到由 Coverity 引发的缺陷。
-[BZ 789278] (https://bugzilla.redhat.com/show_bug.cgi?id=789278)
应该用于 Coverity 问题在主伞 bug
除非你试图在 Bugzilla 修复一个 bug 时特定分支。
-同时发送固定 Coverity 问题请使用修补程序
相同的 bug 数量。
-为 3.6 分公司 Coverity 跟踪 bug 是
[] 1122834() https://bugzilla.redhat.com/show_bug.cgi?id=1122834

-当你决定在一些问题上工作，请将它分配给你的名字
在相同的 Coverity 网站。所以，我们不要踩着每个别人

工作。
-当标记一个 bug 故意在 Coverity 扫描网站，请把
解释为相同。所以，它将帮助他人到

了解背后的理据。

* 如果您有更多的疑问，请发送到
[gluster-发展](http://www.gluster.org/interact/mailinglists) 邮寄

列表中 *

# # # CPP 检查

Cppcheck 是可用在 Fedora 和 EL 的座谈回购

-安装 Cppcheck

百胜餐饮集团安装 cppcheck

-克隆 GlusterFS 代码

git 克隆 https://github.com/gluster/glusterfs) glusterfs

-运行 Cpp 检查

cppcheck glusterfs / 2>cppcheck.log

-[BZ 1091677] (https://bugzilla.redhat.com/show_bug.cgi?id=1091677)
应该用于为 Cppcheck 提交补丁到主分支
报告的问题。

# # # 每日运行

我们现在对日常运行的各种静态源代码分析工具
glusterfs 来源。有的大师，每日分析

释放-3.6 和 3.5 版的分支机构。

在公布成绩
< > http://download.gluster.org/pub/gluster/glusterfs/static-analysis/
