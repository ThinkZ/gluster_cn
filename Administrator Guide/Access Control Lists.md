#POSIX 访问控制列表

POSIX 访问控制列表 (Acl) 允许您将分配不同
为不同的用户或组即使他们不这样做的权限
对应于原始所有者或所属的组。

例如︰ 用户约翰会创建文件，但不是想让任何人
要用此文件中，除非另一个用户，安东尼 （即使做任何事
有其他用户属于组约翰）。

这意味着，除了文件所有者、 文件组和其他人，
其他用户和组可以被授予或拒绝访问通过使用
POSIX Acl。

##Activating POSIX Acl 支持

使用 POSIX Acl 的文件或目录，文件的分区或
目录必须安装与 POSIX Acl 支持。

# # #Activating POSIX Acl 支持服务器

若要装载 POSIX Acl 支持后端出口目录，请使用
下面的命令︰

`# mount -o acl `

例如︰

`# mount -o acl /dev/sda1 /export1 `

或者，如果在 /etc/fstab 文件中列出了该分区，则添加
要包括 POSIX Acl 选项的分区以下条目︰

`LABEL=/work /export1 ext3 rw, acl 14 `

# # #Activating POSIX Acl 支持在客户端上

要装入 POSIX Acl 支持 glusterfs 卷，请使用以下
命令︰

`# mount –t glusterfs -o acl `

例如︰

`# mount -t glusterfs -o acl 198.192.198.234:glustervolume /mnt/gluster`

##Setting POSIX Acl

你可以设置两种类型的 POSIX Acl，那就是，访问 Acl 和默认
Acl。您可以使用 Acl 访问授予对特定文件的权限或

目录。您可以使用默认 Acl 只上目录，但如果文件

内部目录没有 Acl，它将继承的权限
目录的默认 Acl。

您可以为用户不在用户中每个用户，每个组，设置 Acl
组的文件，并通过有效正确的面具。

##Setting 访问 Acl

您可以应用访问权限 Acl 来为这两个文件授予权限和
目录。

* * 到设置或修改访问权限 Acl * *

您可以设置或修改访问权限 Acl 使用下面的命令︰

`# setfacl –m  file `

ACL 条目类型具有所有者的 POSIX Acl 表示、 群体、
和其他。

Permissions must be a combination of the characters `r` (read), `w`
(write), and `x` (execute). You must specify the ACL entry in the
以下格式，并且可以指定多个条目类型隔开
逗号。

ACL 条目 |描述

--- |---

u:uid: \ <permission\> |为用户设置的访问权限 Acl。您可以指定用户名或 UID

g:gid: \ <permission\> |设置组的 Acl 访问。您可以指定组的名称或 GID。

m:\ <permission\> |设置有效权限掩码。面具是所属组的所有访问权限和所有的用户和组项的组合。

o:\ <permission\> |对于不使用的文件组中的用户设置的访问权限 Acl。


如果一个文件或目录已 POSIX Acl 和 setfacl
使用命令，额外的权限添加到现有的
修改 POSIX Acl 或现有规则。

例如，要给读取和写入权限用户安东尼︰

`# setfacl -m u:antony:rw /mnt/gluster/data/testfile `

##Setting 默认 Acl

你可以默认 Acl 只适用于目录。他们确定

文件系统对象，从其父网站继承权限
当它创建的目录。

要设置默认 Acl

您可以设置默认 Acl 的文件和目录使用下面的代码
命令︰

`# setfacl –m –-set `

必须将权限组合字符 r （阅读）、 w （写入） 和 x （执行）。指定 ACL entry_type，如下所述，用逗号分隔多个条目类型。


u: * user_name:permissons*
为用户设置的访问权限 Acl。指定的用户名或 UID。


g: * group_name:permissions*
设置组的 Acl 访问。指定的组的名称或 GID。


m: * 权限 *
设置有效权限掩码。面具是所属的组，和所有的用户和组条目的所有访问权限的组合。


o: * 权限 *
对于不使用的文件组中的用户设置的访问权限 Acl。

例如，若要设置/数据目录的默认 Acl，以便阅读
不在用户组中的用户︰

`# setfacl –m --set o::r /mnt/gluster/data `

> * * 注意 * *
>
> 访问 Acl 设置为单个文件可以覆盖默认值
> Acl 权限。

* * 的默认 Acl * * 的影响

以下为在其中的方法目录的权限
默认 Acl 传递到的文件和子目录中︰

-一个子目录继承父目录的默认 Acl
无论是作为其默认 Acl 和 Acl 访问。
-文件作为其访问 Acl 继承的默认 Acl。

##Retrieving POSIX Acl

您可以查看现有的 POSIX Acl 的文件或目录。

* * 到查看现有 POSIX Acl * *

-查看现有访问 Acl 的文件，使用下面的命令︰

    `# getfacl `

例如，若要查看现有的 POSIX Acl 为 sample.jpg

# getfacl /mnt/gluster/data/test/sample.jpg
# 所有者︰ 安东尼
# 组︰ 安东尼
user::rw-
group::rw-
other::r — —

-查看默认 Acl 的目录，使用以下命令︰

    `# getfacl `

例如，若要查看现有 Acl/数据/doc

# getfacl /mnt/gluster/data/doc
# 所有者︰ 安东尼
# 组︰ 安东尼
user::rw-
用户︰ 约翰︰ r-
group::r — —
mask::r — —
other::r — —
默认︰ 用户︰ 可
默认︰ 用户︰ 安东尼︰ 可
默认︰ 组:: r-x
默认︰ 面膜︰ 可
默认︰ 其他:: r-x

##Removing POSIX Acl

要删除所有用户、 组或其他人的权限，请使用
下面的命令︰

`# setfacl -x `

# # #setfaclentry_type 选项

ACL entry_type 转换的 POSIX ACL 的所有者、 组和其他表示形式。

必须将权限组合字符 r （阅读）、 w （写入） 和 x （执行）。指定 ACL entry_type，如下所述，用逗号分隔多个条目类型。


u: * 用户名 *
为用户设置的访问权限 Acl。指定的用户名或 UID。


g: * 组名 *
设置组的 Acl 访问。指定的组的名称或 GID。


m: * 权限 *
设置有效权限掩码。面具是所属的组，和所有的用户和组条目的所有访问权限的组合。


o: * 权限 *
对于不使用的文件组中的用户设置的访问权限 Acl。

例如，若要删除用户安东尼的所有权限︰

`# setfacl -x u:antony /mnt/gluster/data/test-file`

##Samba 和 Acl

如果你正在使用 Samba 访问 GlusterFS 熔断器的装载，然后 POSIX Acl
默认情况下启用。与已编译 samba

`--with-acl-support` option, so no special flags are required when
访问或安装一个桑巴舞共享。

##NFS 和 Acl

目前 GlusterFS 支持 POSIX ACL 配置通过 NFS 挂载
即 setfacl 和 getfacl 命令工作通过 NFS 挂载。
