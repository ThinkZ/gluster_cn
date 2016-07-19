本指南已经过测试的 Ubuntu 13.10 在清洁、 与时俱进
环境。年长的 Ubuntu 发行版所需一些黑客如果我还记得

正确。其他 debian 的基于发行版应该能够遵循这

调整为依赖项。请更新此如果你得到它的工作

另一个发行版。

# # # 令人满足的依赖关系

使变 qemu 依赖项的第一个步

apt-get 来生成 dep qemu

这下一个命令抓住在 debian 中指定的所有依赖项
控制文件，要求从上游 debian sid，你可以看看
选项指定和调整口味。

# 得到的几乎所有的休息和工具上班了 debian 神奇的魔力
apt-get 来安装 devscripts 被子 libiscsi-dev libusbredirparser-dev libssh2-1-dev libvdeplug-dev libjpeg-dev glusterfs *

我们需要 libseccomp 的较新版本的 Ubuntu 13.10

mkdir libseccomp
cd libseccomp
# 抓住它从上游的 sid
wget http://ftp.de.debian.org/debian/pool/main/libs/libseccomp/libseccomp_2.1.0+dfsg.orig.tar.gz
wget http://ftp.de.debian.org/debian/pool/main/libs/libseccomp/libseccomp_2.1.0+dfsg-1.debian.tar.gz
# 把它准备好
焦油 xf libseccomp_2.1.0 + dfsg.orig.tar.gz
cd libseccomp-2.1.0+dfsg/
# 安装 debian 的魔法
焦油 xf。/libseccomp_2.1.0+dfsg-1.debian.tar.gz

# 如果任何适用系列文件
虽然被子推;被子刷新;完成

# 生成 debs，他们就会出现一个目录了
debuild-i-我们-uc b
裁谈会。
# 安装它
dpkg-i *.deb

# # # 建筑 QEMU

这下一部分是简单明了的如果您的依赖性得到满足。为

一旦它被提取前，先进的读者看看周围 debian/控制
如要更改哪些选项 QEMU 建立与安装
和什么目标的要求。

裁谈会。
mkdir qemu
cd qemu
# 下载我们的消息来源。你会想要检查经常对这些变化

wget http://ftp.de.debian.org/debian/pool/main/q/qemu/qemu_1.7.0+dfsg.orig.tar.xz
wget http://ftp.de.debian.org/debian/pool/main/q/qemu/qemu_1.7.0+dfsg-2.debian.tar.gz
wget http://download.gluster.org/pub/gluster/glusterfs/3.4/LATEST/glusterfs-3.4.2.tar.gz
焦油 xf glusterfs-3.4.2.tar.gz
焦油 xf qemu_1.7.0 + dfsg.orig.tar.xz
cd qemu-1.7.0+dfsg/
# 解压的 debian 的魔力
焦油 xf。/qemu_1.7.0+dfsg-2.debian.tar.gz

# glusterfs 给以生成
cp-r。/glusterfs-3.4.2 glusterfs

# glusterfs 签入配置怪异张望。我从未问过为什么但是移动了一个 src 东西工程和测试很好

cd 的 glusterfs api /
mv src / *。
裁谈会。/..

#you 需要编辑 debian/控件以允许 glusterfs 替换

-# # — — 启用 glusterfs todo
+ #-启用 glusterfs
+ glusterfs 常见 (> = 3.4.0)，

#And 最后建立。它会采取年龄。 http://xkcd.com/303/

# 如果任何适用系列
虽然被子推;被子刷新;完成

# 生成包
debuild-i-我们-uc b
裁谈会。

您现在可用于安装的带劲。它是取决于读者来确定

什么目标他们想要安装。
