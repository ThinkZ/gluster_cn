# # 出口和 nfs Netgroup 身份验证

此功能将 Linux 风格出口 & netgroup 身份验证添加到 Gluster 的 NFS 服务器。更具体地说，此功能允许用户限制访问特定 IPs （出口认证） 或网络组 （netgroup 身份验证） 或两者兼而有之 Gluster 卷和 Gluster 卷子目录。Netgroup 在 Unix 环境中用于控制访问 NFS 出口、 远程登录和远程炮弹。每个网络组具有唯一的名称和定义一组主机、 用户、 组和其他 netgroup。此信息存储在文件和 gluster NFS 服务器管理客户端基于这些文件的权限


# # 含义和用法

目前，gluster 可以限制对卷通过简单的 IP 列表的访问。但这一特点使这种能力更具可扩展性允许的 ip 地址通过网络组管理大型列表。此外，它提供了更细粒度的权限处理卷像通配符支持，某些客户端等只读权限。


The file `/var/lib/glusterd/nfs/export` contains the details of machines which can be used as clients for that server.An typical export entry use the following format :

< 出口路径 > <host/netgroup> （选项）。

在这里出口名称可以是 gluster 内该卷的卷或子目录路径。它包含的主机/网络组，然后按照选项适用于该条目列表的下一步。开头的字符串 ' @' 被视为网络组和一个字符串开始没有 @ 是主机。选项包括装载相关参数，就是现在"sec"、"ro/rw"、"anonuid"一个有效的选项。如果 * 是提到作为主机/网络组的领域，然后再将任何客户端可以安装该导出路径。


The file `/var/lib/glusterd/nfs/netgroup` should mention the expansion of each netgroup which mentioned  in the export file. An typical netgroup entry will look like :

< 网络组名称 > ng1000\nng1000 ng999\nng999 ng1\nng1 ng2\nng2 (ip1，ip2，...)

在特定的时间间隔后，gluster NFS 服务器将检查这些文件的内容

# # 卷选项

1.启用出口/网络组功能

gluster 卷上设置启用身份验证-<volname>nfs.exports

2.更改 gluster NFS 服务器的刷新时间间隔

gluster 音量设置 <volname>nfs.auth-刷新-间隔-sec < 以秒为单位的时间 >

3.更改导出项的缓存时间间隔

gluster 音量设置 <volname>nfs.auth-缓存-ttl-sec < 以秒为单位的时间 >

# # 测试出口/网络组文件

用户应该有能力在应用配置之前检查文件的有效性。"Glusterfsd"命令现在具有下列附加参数，可以用来检查配置︰

--打印 netgroup︰ 验证 netgroup 文件并将其打印出来。例如，

    -    `glusterfsd --print-netgroups <name of the file>`

--打印出口︰ 验证导出文件并将其打印出来。例如，

    -    `glusterfsd --print-export <name of the file>`


# # 应要注意的点。

1.此功能当前不支持所有的选项在 man 页面的出口，但我们可以轻松地添加它们。

2. The files `/var/lib/glusterd/nfs/export` and `/var/lib/glusterd/nfs/netgroup` should be created before setting the `nfs.exports-auth-enable` option in every node in Trusted Storage Pool.

3.这些文件是由用户的处理的标识。因此，它们的内容可以跨信任存储池不同 gluster nfs 服务器之间。即有可能有不同的身份验证 gluster 同一群集中的 NFS 服务器的机制。


4. Do not mixup this feature and authentication using `nfs.rpc-auth-allow`, `nfs.export-dir` which may result in inconsistency.

# # 疑难解答

更改后的内容文件，如果它并没有反映正确的身份验证机制，只是重新启动服务器使用卷停止和启动，这样该 gluster NFS 服务器将有力地再次读取这些文件的内容。
