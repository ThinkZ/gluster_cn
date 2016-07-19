# # # 安装 Gluster

基于 RPM 的发行版，如果你将使用无限带宽，添加
glusterfs RDMA 包到装置。对于 RPM 基于系统，百胜餐饮集团

作为安装方法用于满足外部 depencies
如兼容 readline5

# # # 为 Debian

下载的软件包

wget-钕-数控-r-A.deb http://download.gluster.org/pub/gluster/glusterfs/LATEST/Debian/wheezy/

(从读者的注意︰ 以上不工作。检查

< http://download.gluster.org/pub/gluster/glusterfs/LATEST/Debian/ > 为
3.5 版本或使用 http://packages.debian.org/wheezy/glusterfs-server

安装 Gluster 软件包 （做这两台服务器）

dpkg-i glusterfs_3.5.2 4_amd64.deb

# # # 为 Ubuntu

Ubuntu 10 和 12︰ 安装 python 软件属性︰

sudo apt-get 来安装 python-软件-属性
		
Ubuntu 14︰ 安装软件-属性-常见︰

sudo apt-get 来安装软件-属性-常见

然后添加 GlusterFS PPA 社区︰

sudo 添加 apt 资料库 ppa:gluster / glusterfs-3.5
sudo apt-get 来更新

最后，安装的软件包︰

sudo apt-get 来安装 glusterfs-服务器

* 注意︰ 软件包存在为 Ubuntu 12.04 LTS，12.10、 13.10 和 14.04
LTS *

# # # 为红色帽子/CentOS

下载的软件包 (修改 URL 为您具体的释放和
体系结构）。

wget-P /etc/yum.repos.d http://download.gluster.org/pub/gluster/glusterfs/LATEST/RHEL/glusterfs-epel.repo

安装 Gluster 软件包 （做这两台服务器）

百胜餐饮集团安装 glusterfs 服务器

# # # 为 Fedora

安装 Gluster 软件包 （做这两台服务器）

百胜餐饮集团安装 glusterfs 服务器

一旦你完成安装，你可以继续前进到 [configuration](./Configure.md) 节。
