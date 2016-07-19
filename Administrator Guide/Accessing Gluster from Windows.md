# 访问 Gluster 卷通过 SMB 协议

层状的产品 Samba 用于导出 Gluster 卷和 ctdb 提供的高可用性桑巴舞。
这里是配置高可用 Samba 群集出口 Gluster 卷的步骤。

_Note︰ 这些配置步骤是适用于 Samba 版本 = 4.1.* 和 Gluster 版本 > = 3.7。 * ctdb > = 2.5_* *

# # #Step 1︰ 选择将出口 Gluster 卷的服务器。
服务器可能/不可能信任的存储池的一部分。最好的服务器数量是 < = 4。在这些服务器上安装 Samba 和 ctdb 的软件包。


# # #Step 2︰ 启用/禁用 Gluster 卷通过 SMB 的汽车出口
```# gluster volume set VOLNAME user.smb disable/enable```

# # #Step 3︰ 设置 CTDB 群集︰
1.创建 ctdb 元卷，副本 N，N 被用作 Samba 服务器的服务器数目。
此卷将主机只有一个零字节锁文件，因此选择最小尺寸的砖。
用来创建 n 副本卷运行以下命令︰
   ```# gluster volume create <volname> replica n <ipaddr/host name>:/<brick_patch>.... N times```

2."在以下文件中，全部替换"在语句中"元 = 所有"为新创建的卷的名称。
    ```
/var/lib/glusterd/hooks/1/start/post/S29CTDBsetup.sh
/var/lib/glusterd/hooks/1/stop/pre/S29CTDB-teardown.sh
    ```

3.启动 ctdb 卷
   ```# gluster vol start <volname>```

4.确认满足下列条件︰
* 如果在 smb.conf 文件中运行 samba/ctdb 的所有节点都需要添加以下行︰
    ```
聚类 = 是
idmap 后端 = tdb2
    ```

* 如果在运行 ctdb/samba 的所有节点上/gluster/锁在装入 ctdb 卷

* 如果在 fstab/等/添加 ctdb 卷装载条目

* 如果文件 /etc/sysconfig/ctdb 存在的所有节点上运行 ctdb/桑巴

5.创建运行 ctdb/桑巴舞的所有节点上的 /etc/ctdb/nodes 文件并在文件中添加所有这些节点的 ip 地址。
例句︰
   ```
#cat /etc/ctdb/nodes
10.16.157.0
10.16.157.3
10.16.157.6
10.16.157.8
   ```
此处列出的 Ip 都是 Samba/ctdb，应该是一个私人的非路由子网和服务器只用于内部群集通信的私人 IPs。更多详细信息，请参阅 ctdb 手册页。


6.创建运行 ctdb/桑巴舞的所有节点上的 /etc/ctdb/public_addresses 文件并添加虚拟 IPs 采用以下格式︰
   ```<virtual IP><routing prefix> <node interface>```  
   ```
例句︰
#cat /etc/ctdb/public_addresses
192.168.1.20/24 eth0
192.168.1.21/24 eth0
   ```

# # #Step 4︰ 建议在导出卷之前的设置
1.允许不安全端口，客户端到砖和 glusterd 连接的客户端
   ```# gluster volume set VOLNAME server.allow-insecure```

此外编辑受信任的存储池中的所有节点上的 /etc/glusterfs/glusterd.vol 并添加以下设置︰
     ```option rpc-auth-allow-insecure on```

重新启动 glusterd 服务在所有节点上︰
     ```systemctl restart glusterd```

2.禁用在 gluster 客户端缓存的元数据︰
   ```gluster volume set VOLNAME stat-prefetch off```

3.为确保锁和 IO 一致性︰
   ```#gluster volume set VOLNAME storage.batch-fsync-delay-usec 0```

4.如果使用 Samba 4.X 版本添加以下行或全球部分中所有 gluster 卷 smb.conf
   ```
内核共享模式 = 否
内核机会锁定 = 否
映射档案 = 否
隐藏的地图 = 否
读取的地图只有 = 否
映射系统 = 否
存储 dos 属性 = 是
   ```

注意︰
设置存储 dos 属性 = no' 建议如果存档，隐藏，读取唯一的 dos 属性则不使用。
这可以给更好的性能。

5.如果 selinux 启用和强制执行措施，请尝试以下命令如果 ctdb 失败。
   ```
# setsebool-P use_fusefs_home_dirs 1
# setsebool-P samba_load_libgfapi 1
   ```

# # #Step 5︰ 装入卷使用 SMB
1.如果没有活动目录安装所有 samba 服务器上添加用户并设置密码
   ```
# adduser 用户名
# smbpasswd-用户名
   ```

2.启动 ctdb，smb 和其他相关的服务︰
   ```
# 立即重新开始 ctdb
# ctdb 状态
# ctdb ip
# ctdb ping-n 所有
   ```

3.为了验证是否通过 samba 出口量可以由用户访问︰
   ```# smbclient //<hostname>/gluster-<volname> -U <username>%<password>```

4.对 linux 系统上安装︰
   ```# mount -t cifs -o user=<username>,pass=<password> //<Virtual IP>/gluster-<volname> /<mountpoint>```

在 Windows 系统上装载︰
   ```>net use <device:> \\<Virtual IP>\gluster-<volname>```  
或
   ```\\<Virtual IP>\gluster-<volname>``` from windows explorer.
