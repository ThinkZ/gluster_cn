# 处理的用户属于多个组

用户可以属于许多不同 (UNIX) 组。这些群体通常是

用于允许或拒绝执行命令的权限或访问文件和
目录。

一个用户可以属于的组的数量取决于操作系统，但
也有支持较少的组的组件。在 Gluster，有

在堆栈中的不同层面的不同限制。在解释

这份文件应该澄清存在哪些限制，并且这些怎么能
处理。


# # tl; 博士

-如果用户属于多个 90 组，砖过程需要解决
  the secondary/auxilary groups with the `server.manage-gids` volume option
- the linux kernels `/proc` filesystem provides up to 32 groups of a running
  process, if this is not sufficient the mount option `resolve-gids` can be
使用
- Gluster/NFS needs `nfs.server-aux-gids` when users accessing a Gluster volume
通过 NFS 属于超过 16 组

为所有上述选项计数，做组解决系统
must be configured (`nsswitch`, `sssd`, ..) to be able to get all groups when
众所周知，只有一个 UID。


# # GlusterFS 协议限制

当 Gluster 客户端做一些行动 Gluster 的卷上时，该操作是
在 RPC 数据包发送。此 RPC 数据包中包含页眉的凭据

用户。服务器端接收 RPC 数据包，并使用凭据

从 RPC 头执行所有权操作和允许拒绝检查。

GlusterFS 协议所使用的 RPC 标头可以包含最多 ~ 93 组。
为了通过这限制，接收 RPC 服务器进程 （砖）
程序可以做解决本地组和忽略 （太少）
RPC 页眉的组。

这就要求服务过程可以解决所有的用户组
客户端进程的 UID。大多数的环境中，他们已经有很多团体，

使用配置用户和组设有中环
位置。它对于企业共同来管理用户和他们组中

LDAP，Active Directory，NIS 或类似。

要在服务器端 （砖），该卷上解析用户的组
option `server.manage-gids` needs to be set. Once this option is enabled, the
砖的过程不会使用 Gluster 客户端发送，但会的组
use the POSIX `getgrouplist()` function to fetch them.

因为这是一个协议限制，所有的客户端，包括保险丝坐骑
Gluster/NFS 服务器和 libgfapi 应用程序受到了这。


# # 组限制带保险丝

保险丝客户端获取的过程，并通过读取 I/O 组
information from `/proc/$pid/status`. This file only contains up to 32 groups.
如果客户端 xlators 依靠所有群体的过程/用户 （如 posix-acl)，
这些 32 组可能会限制功能。

For that reason a mount option has been added. With the `resolve-gids` mount
option, the FUSE client calls the POSIX `getgrouplist()` function instead of
reading `/proc/$pid/status`.


# # 组限制为 NFS 的

NFS 协议 （实际上 AUTH_SYS/AUTH_UNIX RPC 报头） 允许达 16
群体。这些都是 NFS 服务器接收到从 NFS 客户端的组。

类似的方式砖过程可以解决群体上
服务器端，NFS 服务器可以通过 NFS 客户端和使用 UID
这一点解决群体。为此卷选项是

`nfs.server-aux-gids`.

其他 NFS 服务器也提供类似的选项。Linux 内核 nfsd 服务器

uses `rpc.mountd --manage-gids`. NFS-Ganesha has the configuration option
`Manage_Gids`.


# # 这些解决方案的影响

所有提到的选项默认被禁用的。原因之一是

解决组是一个昂贵的操作。在许多情况下，就没有必要

支持很多组织和性能的影响。

当群体都得到解决时，被缓存列表。缓存的有效期

可配置。Gluster 进程并非只是那些缓存这些

groups. `nscd` or `sssd` also keep a cache when they handle the
`getgroupslist()` requests. When there are many requests, and querying the
群体从集中的管理系统花费的时间长、 缓存可能受益
从较长的有效性。

其他，不太明显的差异，也就可能发现了。是的很多过程

编写安全意识减少过程可以组
effectively use. This is normally done with the `setegids()` function. When
存储过程不履行是有效的较少的组，
进程使用 UID 来解决所有群体的过程中，有的组
dropped with `setegids()` are added back again. this could lead to permissions
这一进程不应该有。
