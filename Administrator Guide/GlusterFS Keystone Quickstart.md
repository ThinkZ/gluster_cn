这是一个文档，并可能包含一些错误或缺少的信息。

我目前正在建设 AWS 图像与此安装，但是如果你不能等待，并且想要安装此脚本，这里有两篇文章，以适合亚马逊 CentOS/RHEL 6 AMI，如 ami a6e15bcf 的默认设置中的命令

本文档假定您已经有 GlusterFS 与不明飞行物安装，3.3.1-11 或更高版本，和都在这里使用的说明︰

[`http://www.gluster.org/2012/09/howto-using-ufo-swift-a-quick-and-dirty-setup-guide/`](http://www.gluster.org/2012/09/howto-using-ufo-swift-a-quick-and-dirty-setup-guide/)

从很大程度上推导出这些文档︰

[`http://fedoraproject.org/wiki/Getting_started_with_OpenStack_on_Fedora_17#Initial_Keystone_setup`](http://fedoraproject.org/wiki/Getting_started_with_OpenStack_on_Fedora_17#Initial_Keystone_setup)
[`http://blog.jebpages.com/archives/fedora-17-openstack-and-gluster-3-3/`](http://blog.jebpages.com/archives/fedora-17-openstack-and-gluster-3-3/)

添加 RDO Openstack 灰熊和座谈的回购协议︰

		$ sudo yum install -y `[`http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm`](http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm)
		$ sudo yum install -y `[`http://rdo.fedorapeople.org/openstack/openstack-grizzly/rdo-release-grizzly-1.noarch.rpm`](http://rdo.fedorapeople.org/openstack/openstack-grizzly/rdo-release-grizzly-1.noarch.rpm)

安装 Openstack 基石

$ sudo 百胜安装 openstack 基石 openstack utils python keystoneclient

配置重点

$ 猫 > keystonerc < < _EOF
出口 ADMIN_TOKEN = $(openssl 兰德-十六进制 10)
出口 OS_USERNAME = 管理员
出口 OS_PASSWORD = $(openssl 兰德-十六进制 10)
出口 OS_TENANT_NAME = 管理员
		export OS_AUTH_URL=`[`https://127.0.0.1:5000/v2.0/`](https://127.0.0.1:5000/v2.0/)
		export SERVICE_ENDPOINT=`[`https://127.0.0.1:35357/v2.0/`](https://127.0.0.1:35357/v2.0/)
出口 SERVICE_TOKEN = \ $ADMIN_TOKEN
_EOF
$ ../ keystonerc

sudo openstack db $ — — 服务基石 — — 初始化

将重点配置追加到 /etc/swift/proxy-server.conf

$ sudo-i '
# 猫 > > /etc/swift/proxy-server.conf < < _EOM'
[筛选器︰ 重点]'
使用 = 蛋︰ 斯威夫特 #keystoneauth'
operator_roles = 管理员，swiftoperator'
		
[筛选器︰ authtoken]
paste.filter_factory = keystoneclient.middleware.auth_token:filter_factory
auth_port = 35357
auth_host = 127.0.0.1
auth_protocol = https
_EOM
退出

完成配置斯威夫特和使用命令行工具的重点︰

sudo openstack config $--设置的 /etc/swift/proxy-server.conf 筛选器︰ authtoken admin_token $ADMIN_TOKEN
sudo openstack config $--设置的 /etc/swift/proxy-server.conf 筛选器︰ authtoken auth_token $ADMIN_TOKEN
sudo openstack config $--设置的 /etc/swift/proxy-server.conf 默认 log_name proxy_server
sudo openstack config $--设置的 /etc/swift/proxy-server.conf 筛选器︰ authtoken signing_dir/等/斯威夫特
sudo openstack config $--设置的 /etc/swift/proxy-server.conf 管道︰ 主管道"运行状况检查缓存 authtoken 重点代理-服务器"

sudo openstack config $--设置的 /etc/keystone/keystone.conf 默认 admin_token $ADMIN_TOKEN
sudo openstack config $--设置的 /etc/keystone/keystone.conf ssl 启用真实
sudo openstack config $--设置的 /etc/keystone/keystone.conf ssl 密钥文件 /etc/swift/cert.key
sudo openstack config $--设置的 /etc/keystone/keystone.conf ssl certfile /etc/swift/cert.crt
sudo openstack config $--签署 token_format UUID 的集 /etc/keystone/keystone.conf
sudo openstack config $--设置的 /etc/keystone/keystone.conf sql 连接 mysql://keystone:keystone@127.0.0.1/keystone

配置重点在引导时启动，并启动它。

$ sudo chkconfig openstack-基本原理
如果您编写脚本这 $ sudo 服务 openstack 重点启动 #，要等待几秒钟，开始使用它

我们使用的不受信任的证书，所以告诉重点不抱怨。如果你替换为受信任的证书，或不使用 SSL，则将此设置为""。


$ 没安全感 ="— — 没有安全感"

在重点中创建的重点及迅速的服务︰

$ KS_SERVICEID = $(重点 $INSECURE 服务创建 — — 名称 = 基石 — — 类型 = 身份 — — 描述 ="重点标识服务"| grep"id"|切-d"|"-f 3)

$ SW_SERVICEID = $(重点 $INSECURE 服务创建 — — 名称 = swift — — 类型 = 对象存储 — — 描述 ="快速服务"| grep"id"|切-d"|"-f 3)

		$ endpoint="`[`https://127.0.0.1:443`](https://127.0.0.1:443)`"
$ 基石 $INSECURE 端点创建 — — service_id $KS_SERVICEID \
		  --publicurl $endpoint'/v2.0' --adminurl `[`https://127.0.0.1:35357/v2.0`](https://127.0.0.1:35357/v2.0)` \
		  --internalurl `[`https://127.0.0.1:5000/v2.0`](https://127.0.0.1:5000/v2.0)
$ 基石 $INSECURE 端点创建 — — service_id $SW_SERVICEID \
— — publicurl $endpoint '/ v1/AUTH_ (tenant_id) $s \
— — adminurl $endpoint '/ v1/AUTH_ (tenant_id) $s \
— — internalurl $endpoint'/ v1/AUTH_ (tenant_id) $ s'

创建管理员租户︰

$ admin_id = $(重点 $INSECURE 租客-创建 — — 名称管理 — — 描述"内部管理租户"| grep id | awk ' {打印 $4}')

创建管理员角色︰

$ admin_role = $(重点 $INSECURE 角色创建 — — 名称管理 | grep id | awk ' {打印 $4}')
$ ksadmin_role = $(重点 $INSECURE 角色创建 — — 名称 KeystoneServiceAdmin | grep id | awk ' {打印 $4}')
$ kadmin_role = $(重点 $INSECURE 角色创建 — — 名称 KeystoneAdmin | grep id | awk ' {打印 $4}')
$ member_role = $(重点 $INSECURE 角色创建 — — 名称成员 | grep id | awk ' {打印 $4}')

创建管理员用户︰

$ user_id = $(重点 $INSECURE 用户创建 — — 名称管理 — — 租户 id $admin_id — — 通 $OS_PASSWORD | grep id | awk ' {打印 $4}')
$ 基石 $INSECURE 用户-角色添加 — — $user_id — — 租户 id $admin_id 的用户 id。
— — 角色 id $admin_role
$ 基石 $INSECURE 用户-角色添加 — — $user_id — — 租户 id $admin_id 的用户 id。
— — 角色 id $kadmin_role
$ 基石 $INSECURE 用户-角色添加 — — $user_id — — 租户 id $admin_id 的用户 id。
— — 角色 id $ksadmin_role

如果您不具有多卷支持 (破碎在 3.3.1-11)，然后卷的名称不会关联到住户，和所有租户将映射到同一个卷，那么只需使用一个正常的名字。（这将固定在 3.4 中，并应固定在 3.4 beta 版。为此 bug 报告是在这里: < https://bugzilla.redhat.com/show_bug.cgi?id=924792 >)


$ volname ="admin"
# 或如果您有多卷修补程序
$ volname = $admin_id

创建和启动管理卷︰

$ sudo gluster 卷创建 $volname $myhostname: $pathtobrick
$ sudo gluster 卷启动 $volname
$ sudo 服务 openstack 重点启动

为管理员租客创建圆环。如果你有工作多卷支持，然后你可以指定多个卷的名称调用中︰


$ cd/等/斯威夫特
$ sudo $volname /usr/bin/gluster-swift-gen-builders
$ sudo swift init 主要重启

创建与管理房客与密码 testadmin 和管理员角色相关联的 testadmin 用户︰

$ user_id = $(重点 $INSECURE 用户创建 — — 名称 testadmin — — 租户 id $admin_id — — 通 testadmin | grep id | awk ' {打印 $4}')
$ 基石 $INSECURE 用户-角色添加 — — $user_id — — 租户 id $admin_id 的用户 id。
— — 角色 id $admin_role

测试用户︰

		$ curl $INSECURE -d '{"auth":{"tenantName": "admin", "passwordCredentials":{"username": "testadmin", "password": "testadmin"}}}' -H "Content-type: application/json" `[`https://127.0.0.1:5000/v2.0/tokens`](https://127.0.0.1:5000/v2.0/tokens)

有关更多示例，请参见在这里︰

< > http://docs.openstack.org/developer/keystone/api_curl_examples.html
