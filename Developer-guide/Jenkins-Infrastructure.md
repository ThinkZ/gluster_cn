目前我们使用 Gerrit 和 [詹金斯] (http://jenkins-ci.org)。
我们 Gerrit 实例︰

http://review.gluster.org

它被位于在举办一些古代 VM （严重需要升级）
叫 iWeb 的地方。我们想将这迁移到 Rackspace VM 中

不久的将来。

我们主要的詹金斯实例︰

http://build.gluster.org

这也是詹金斯，相当过时版本上严重
过时的 VM。那个至少在 rackspace 公司的。我们打算在迁移到

新的 VM （和新詹金斯） 在不-太-遥远的未来。然而没有 ETA。;)


除了这些主要的两块，我们有大批的 VM 的在 rackspace 公司
与各种 OS 的身上︰

http://build.gluster.org/computer/

在我们的列表中︰

-bulk\*.cloud.gluster.org\

-临时 VM 的用于运行批量回归测试，为
我们虚假回归失败问题分析
-安装和维护的贾斯汀 · 克利夫特

-freebsd0.cloud.gluster.org\

-FreeBSD 10.0 VM 在 rackspace 公司。用于自动冒烟测试

关于 FreeBSD 的所有建议的修补程序 （使用 Gerrit 触发器）。

-g4s-rackspace 公司-\ * （除了政府飞行服务队-rackspace 公司-f20-1)，和
小小-rackspace 公司-f20-1

-各种 VM 在 Rackspace Fedora 与 el6 型，由安装程序
路易斯 · 帕邦。从詹金斯描述，它们的节点

"开架书库雨燕执行功能测试套件
Gluster-为-斯威夫特"。

-政府飞行服务队-rackspace 公司-f20-1

-在自动构建 Rpm rackspace 公司 VM。安装程序 +

由路易斯 · 帕邦。

-netbsd0.cloud.gluster.org\

-NetBSD 6.1.4 VM 在 rackspace 公司。用于自动冒烟测试

在 NetBSD 6.x 的所有建议的修补程序 （使用 Gerrit 触发器）。
-安装和维护的马努德雷福斯

-netbsd7.cloud.gluster.org\

-NetBSD 7 （测试版） 在 Rackspace VM。用于自动烟雾

测试的所有建议的修补程序 （使用 Gerrit NetBSD 7
触发器）。
-安装和维护的马努德雷福斯

-nbslave7\*.cloud.gluster.org\

-NetBSD 7 奴隶的虚拟机上运行我们的回归测试
-安装和维护的马努德雷福斯

-slave20.cloud.gluster.org-slave49.cloud.gluster.org\

-CentOS 6.5 VM 在 rackspace 公司。用于自动回归

所有建议的修补程序的测试 （使用 Gerrit 触发器）。
-安装和维护的迈克尔 · 谢瑞尔

工作是在 GlusterFS 回归测试，所以他们就会运转
FreeBSD 和 NetBSD （而不是只是 Linux)。当这是完整的

我们会自动运行完全回归测试 FreeBSD 和 NetBSD
为所有也提出修补程序。

非詹金斯 Vm
---------------

**backups.cloud.gluster.org**

担任我们每夜的备份的服务器。安装和维护的迈克尔 ·

谢瑞尔。

* * bareos-dev.cloud.gluster.org，bareos data.cloud.gluster.org**

共享虚拟机调试 Bareos 和 libgfapi 的集成。由维护

尼尔斯 · 德沃斯。

**bugs.cloud.gluster.org**

主办
[gluster-bug-webui]() https://github.com/gluster/gluster-bugs-webui

为 bug 会审/检查。由尼尔斯 de Vos 维护。


**docs.cloud.gluster.org**

运行 readTheDocs-由 Soumya Deb 的文件服务器。

**download.gluster.org**

我们主要的下载服务器-持有 Gluster 二进制文件我们
生成，人们可以下载。

* * gluster-声纳 * *

承载我们 Gluster
[] SonarQube（http://sonar.peircean.com/dashboard/index/com.peircean.glusterfs:glusterfs-java 文件系统）

实例。安装和维护由路易斯 · 祖克曼。


* * 盐 master.gluster.org**

我们配置管理大师的 VM。由迈克尔 · 谢瑞尔。


**munin.gluster.org**

Munin 大师。由迈克尔 · 谢瑞尔。


**webbuilder.gluster.org**

我们的网站生成器。由迈克尔 · 谢瑞尔。


* * www.gluster.org aka supercolony.gluster.org**

主要的网站服务器。由迈克尔 · 谢瑞尔，贾斯汀

克利夫特，别人 （添加您的姓名）
