设置为 GlusterFS 回归测试 Rackspace 詹金斯奴隶
=======================================================================

这是对于 RHEL/CentOS 6.x。下面的命令应该以 root 身份运行。


# # # 安装所需的额外软件包

百胜餐饮集团-y 安装 cmockery2 发展 dbench libacl 发展模拟 nfs utils yajl perl 测试盐小仆

# # # 启用百胜 cron 自动 rpm 更新

chkconfig 百胜 cron 上

# # # 添加模拟用户

useradd-g 模拟模拟

# # # 禁用 eth1

因为如果超过 1 GlusterFS 可以失败以太网接口

sed-i 的 / 启动 = yes/启动 = no /' /etc/sysconfig/network-scripts/ifcfg-eth1

# # # 禁用 IPv6

如每 < https://access.redhat.com/site/node/8709 >

sed-i 的 / IPV6INIT = yes/IPV6INIT = no /' /etc/sysconfig/network-scripts/ifcfg-eth0
回声 ' ipv6 选项禁用 = 1' > /etc/modprobe.d/ipv6.conf
chkconfig ip6tables 关闭
sed-i 的 / NETWORKING_IPV6 = yes/NETWORKING_IPV6 = no /' /etc/sysconfig/network
回声 '' > > /etc/sysctl.conf
回声 '# ipv6 支持在内核中，默认设置为 0' > > /etc/sysctl.conf
回声 ' net.ipv6.conf.all.disable_ipv6 = 1' > > /etc/sysctl.conf
回声 ' net.ipv6.conf.default.disable_ipv6 = 1' > > /etc/sysctl.conf
sed-i 的 / v inet6 /-inet6 /' / 等/网格及上网方式

# # # 更新主机名

六、 /etc/sysconfig/network
vi/等/主机

# # # 从 /etc/hosts 删除 IPv6 和 eth1 接口

sed-i 's/^10\./#10\./' / 等/主机
sed-i 的 / ^2001 / # 2001年 /' / 等/主机

# # # 安装 ntp

百胜餐饮集团-y 安装 ntp
chkconfig ntpdate 上
服务 ntpdate 开始

# # # 安装 OpenJDK，詹金斯奴隶所需

百胜餐饮集团-y 安装 java 1.7.0 以来 openjdk

# # # 创建詹金斯用户

useradd-G 轮詹金斯
chmod 755/首页/詹金斯

# # # 设置詹金斯密码

passwd 詹金斯

# # # 从 build.gluster.org 复制詹金斯 SSH 密钥

mkdir /home/jenkins/.ssh
chmod 700 /home/jenkins/.ssh
		cp `<somewhere>` /home/jenkins/.ssh/id_rsa
chown-R jenkins:jenkins /home/jenkins/.ssh
chmod 600 /home/jenkins/.ssh/id_rsa

# # # 生成詹金斯用户 SSH 已知的主机文件

苏-詹金斯
mkdir ~/foo
cd ~/foo
		git clone `[`ssh://build@review.gluster.org/glusterfs.git`](ssh://build@review.gluster.org/glusterfs.git)
（这会问是否应添加新的主机指纹。 选择是）

裁谈会。
rm-rf ~/foo
退出

# # # 从 RPMForge 安装 git

百胜餐饮集团-y 安装 http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
百胜餐饮集团-y — — enablerepo = rpmforge 额外更新 git

# # # 安装 GlusterFS 修补程序验收测试

git 克隆 git://forge.gluster.org/gluster-patch-acceptance-tests/gluster-patch-acceptance-tests.git /opt/qa

# # # 添加环回挂载点到 fstab/等 /

1 GB Rackspace vm 使用该操作︰

回声 '/ backingstore /d xfs 循环 0 2' > > fstab/等 /
装入 /d

2 GB 及以上 Rackspace VM 使用该操作︰

回声 '/ dev/xvde /d xfs 默认值 0 2' > > fstab/等 /
装入 /d

# # # 创建所需的回归测试的目录

JDIRS ="var/日志/glusterfs /var/lib/glusterd/var/运行/gluster /d/d/archived_builds/d/后端/d/生成/d/登录 /home/jenkins/root"
mkdir-p $JDIRS
chown $JDIRS jenkins:jenkins
chmod 755 $JDIRS
ln-s/d/建立名声

# # # 创建的目录在哪里存档回归日志

及 ="/ 档案/archived_builds /archives/logs"
mkdir-p $ADIRS
chown $ADIRS jenkins:jenkins
chmod 755 $ADIRS

# # # 安装 Nginx

为使日志可通过 http

百胜餐饮集团-y 安装 http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm
百胜餐饮集团-y 安装 nginx
lokkit-s http

# # # Nginx 配置文件要复制到的地方

cp-f /opt/qa/nginx/default.conf /etc/nginx/conf.d/default.conf

# # # Sudo 的轮组启用

sed-i 的 / # %wheel\tALL= （所有） \tNOPASSWD/%wheel\tALL= （所有） \tNOPASSWD /' sudoers/等 /

# # # 重启 （网络更改才会生效）

重新启动

# # # 添加正向和反向 DNS 条目为奴到 rackspace 公司 DNS

Rackspace 公司最近添加 [API 调用其云
DNS] (https://developer.rackspace.com/docs/cloud-dns/getting-started/?lang=python)
服务，所以我们应该能够完全自动化本部以及现在。
