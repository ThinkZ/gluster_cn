如何编译 GlusterFS Rpm 从 git 源 RHEL/CentOS，和软呢帽
--------------------------------------------------------------------------

从 git 创建 rpm 的 GlusterFS 源是相当容易的一旦你知道的步骤。

RPM 可以至少在编译以下操作系统︰

红色帽子企业 Linux 5，6 （& 7 时可用）
-CentOS 5、 6 （& 7 时可用）
-Fedora 16-20

编制具体说明如下。如果您使用的︰


-Fedora 16-20-Fedora 的步骤，然后做所有的通用步骤。
-CentOS 5.x-步骤 CentOS 5.x 的同时，然后做的所有常见步骤
-CentOS 6.x-步骤 CentOS 6.x 的同时，然后做所有的通用步骤。
-RHEL 6.x-步骤 RHEL 6.x 的同时，然后做所有的通用步骤。

请注意--这些指令被显式的考所有 CentOS 5.10，RHEL 6.4，CentOS 6.4 + Fedora 16-20。RHEL/CentOS 和 Fedora 的其他版本可能工作太，但还没有经过测试。请如果您这样做，相应地更新此页。:)


# # # 制备步骤为 Fedora 16-20 （仅限） 的

1.安装 gcc，python 开发头文件和 python setuptools:

sudo 百胜 y $ 安装 python 开发 python setuptools 海湾合作委员会

2.如果你录制 GlusterFS 3.4 版本，然后安装 python swiftclient。其他 GlusterFS 版本不需要它︰


$ sudo 这么 simplejson python swiftclient

现在贯彻 * * 常见步骤 * * 下面的部分。

# # # 制备步骤为 CentOS 5.x （仅限）

你需要首先安装的座谈和一些 CentOS 特定软件包。下面的命令会为你做的事。在那之后，按照"共同步骤"一节。


1.首先安装战术︰

		$ curl -OL `[`http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm`](http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm)
sudo 百胜 y $ 安装座谈-释放-5-4.noarch.rpm-nogpgcheck

2.安装仅在 CentOS 上所需的程序包 5.x:

sudo 百胜 y $ 安装 buildsys 宏 gcc ncurses 发展 python ctypes python sphinx10 \
redhat-rpm-配置

现在贯彻 * * 常见步骤 * * 下面的部分。

# # # 制备步骤为 CentOS 6.x （仅限）

你需要首先安装的座谈和一些 CentOS 特定软件包。下面的命令会为你做的事。在那之后，按照"共同步骤"一节。


1.首先安装战术︰

		$ sudo yum -y install `[`http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`](http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm)

2.安装仅在 CentOS 上所需的程序包︰

sudo 百胜 y $ 安装 python webob1.0 python-粘贴-deploy1.5 python sphinx10 redhat-rpm-配置

现在贯彻 * * 常见步骤 * * 下面的部分。

# # # 制备步骤为 RHEL 6.x （仅限）

你需要首先安装的座谈和一些 RHEL 特定软件包。下面的 2 命令会为你做的事。在那之后，按照"共同步骤"一节。


1.首先安装战术︰

		$ sudo yum -y install `[`http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`](http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm)

2.安装仅在 RHEL 上所需的程序包︰

$ sudo yum-y — — enablerepo =-rhel-6-服务器-可选 rpm 安装 python webob1.0 \
python-粘贴-deploy1.5 python sphinx10 redhat-rpm-配置

现在贯彻 * * 常见步骤 * * 下面的部分。

# # # 常见步骤

这些步骤是 Fedora 和 RHEL/CentOS。在结束了你得为你的平台，准备好安装 GlusterFS Rpm 的完整集合。


* * 下面的步骤 1 的注意事项: * *

-如果您在 RHEL/CentOS 5.x 和获取有关 lvm2 开发不是可用的消息，它是好的。你可以忽略它。:)

-如果您在 RHEL/CentOS 6.x 和获取关于 python eventlet、 python netifaces、 python 狮身人面像和 pyxattr 不是可用的任何消息，没关系。您可以忽略它们。:)


1.安装所需的软件包

$ sudo yum-y — — disablerepo = rhs * — — enablerepo = * 可选 rpm 安装 git autoconf \
automake 野牛 dos2unix flex glib2 发展 libaio 发展融合发展。
libattr 发展 libibverbs 发展 librdmacm 发展 libtool libxml2 发展 lvm2 开发使 \
openssl 发展 pkgconfig pyliblzma python 开发 python eventlet python netifaces \
python 粘贴部署 python simplejson python 狮身人面像 python webob pyxattr readline 发展。
rpm-生成 systemtap 特殊和差别待遇发展焦油 libcmocka-发展

2.克隆 GlusterFS git 存储库

    	$ git clone `[`git://git.gluster.org/glusterfs`](git://git.gluster.org/glusterfs)
$ cd glusterfs

3.选择要编译哪个分支

如果你想要编译的最新的开发代码，你可以跳过此步骤并前进到下一个。:)


如果相反，你想要编译的代码的特定版本的 GlusterFS （如 v3.4)，得到在这里释放名称的列表︰

$ git 分支-|grep 释放

remotes/origin/release-2.0
remotes/origin/release-3.0
remotes/origin/release-3.1
remotes/origin/release-3.2
remotes/origin/release-3.3
remotes/origin/release-3.4
remotes/origin/release-3.5

然后切换到使用 git"签出"命令，以及后释放的名称正确释放"遥控器/起源 /"咬从上面的列表︰

$ git 签出版本-3.4

* * 请注意--* * 的 CentOS 5.x 说明只测试过在 GlusterFS git 中主分支。它是未知 （但） 如果他们为分支机构工作老然后 3.5 版。


4.配置和编译 GlusterFS

现在，你已准备好去编译 Gluster 的功能︰

$./autogen.sh
$./ 配置 — — 启用 fusermount
$ 使 dist

5.创建 GlusterFS Rpm

$ cd 临时演员/LinuxRPM
$ 使 glusterrpms

应该完成，没有错误，您留下包含 Rpm 的目录。

$ ls-l * rpm
-rw-rw-r-1 jc jc 3966111 Mar 2 12:15 glusterfs-3git-1.el5.centos.src.rpm
-rw-rw-r-1 jc jc 1548890 Mar 2 12:17 glusterfs-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 66680 Mar 2 12:17 glusterfs-api-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 20399 Mar 2 12:17 glusterfs-api-发展-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 123806 Mar 2 12:17 glusterfs-cli-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 7850357 Mar 2 12:17 glusterfs-debuginfo-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 112677 Mar 2 12:17 glusterfs-发展-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 100410 Mar 2 12:17 glusterfs-保险丝-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 187221 Mar 2 12:17 glusterfs-地理-复制-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 299171 Mar 2 12:17 glusterfs-libs-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 44943 Mar 2 12:17 glusterfs-rdma-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 123065 Mar 2 12:17 glusterfs-回归-测试-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 16224 Mar 2 12:17 glusterfs-资源-代理-3git-1.el5.centos.x86_64.rpm
-rw-rw-r-1 jc jc 654043 Mar 2 12:17 glusterfs-服务器-3git-1.el5.centos.x86_64.rpm
