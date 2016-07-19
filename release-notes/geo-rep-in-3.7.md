# # # 改进节点故障转移问题处理通过使用 Gluster 元卷

复制副本成对一个 Geo rep 工作者应积极与所有
其他副本工人应该是被动的。当活跃的工人去

下来，被动的工人将会变得活跃。在以前的版本，这种逻辑

基于节点-uuid，而现在它基于锁文件中元
卷。现在它有可能更准确地决定主动/被动

和最小化的多种活动工作方案。

土力工程处代表作品没有元卷也，此功能是落后
compatible. By default config option `use_meta_volume` is False. This
feature can be turned on with geo-rep config `use_meta_volume`
true。如果没有此功能土力工程处代表工作因为它工作以前

释放。

如果关闭了 meta_volume 的问题︰

1.多个工人变得活跃和参与
同步。重复的努力和与并发相关的所有问题

执行存在。

2.故障转移只能在节点级别，如果砖过程中去了
节点是活着然后失败后不会发生和延迟同步。

3.有关安置砖的案例很难记录的步骤
副本 3。例如，在每个副本中的第一块砖不应

放置在同一个节点。等。


4.消费更新历史从以前失败节点的时候
回来了，这可能会导致问题，如延迟同步和数据
如果重命名的不一致性。

* * 修复 * *: [1196632] (https://bugzilla.redhat.com/show_bug.cgi?id=1196632)
[] 1217939() https://bugzilla.redhat.com/show_bug.cgi?id=1217939



# # # 改进历史更新历史消费

对消费历史更新历史，介绍了在以前的支持
释放，此版本中，这是更加稳定和改进。使用

文件系统爬网是减到最小，仅在初始同步期间有限。在

早期版本中，节点重新启动或砖过程会被当作
更新日志破损和土力工程处 rep 是回退到 XSync 的
持续时间。此版本中，将考虑更改日志会话

打破只当更改日志处于关闭状态。所有其他的场景

认为是安全。

此功能也被必需的 glusterfind。

* * 修复 * *: [1217944] (https://bugzilla.redhat.com/show_bug.cgi?id=1217944)


# # # 提高地位和检查点

地位得到了很多改进，显示准确的会议详细信息
信息、 用户信息、 奴隶节点连接到主节点，最后
已同步的时间等。初始化时间减少了，状态更改将

发生尽快土力工程处代表工人准备好了。（在以前的版本

初始化时间为 60 秒）

* * 修复 * *: [1212410] (https://bugzilla.redhat.com/show_bug.cgi?id=1212410)

# # # 工人重新启动改进

去和回来的工人是很普通的土力工程处代表，
原因喜欢网络故障，奴隶节点下去等。时候

最多，它有再重新更新历史，因为工人死亡之前
更新上次同步时间。现在优化批处理大小这样

重新处理量减到最小。

* * 修复 * *: [1210965] (https://bugzilla.redhat.com/show_bug.cgi?id=1210965)


# # # 改进重命名处理

当重命名的文件名哈希落到其他的砖，各自砖
更新日志记录重命名，但其余的像创建，数据是主席之友
记录中的第一块砖。每个地理代表工人每砖同步数据到

独立奴隶卷，这些东西坏了，大师
和奴隶卷变得不一致。与双氢睾酮团队的帮助下

重命名记录入账创建数据。

* * 修复 * *: [1141379] (https://bugzilla.redhat.com/show_bug.cgi?id=1141379)


# # # 同步 xattrs 和 acl

同步现在支持 xattrs 和 acl 到奴隶群集。这些

可以禁用的设置配置选项同步 xattrs 或到同步 acl
假。

* * 修复 * *: [1187021] (https://bugzilla.redhat.com/show_bug.cgi?id=1187021)
[] 1196690() https://bugzilla.redhat.com/show_bug.cgi?id=1196690



# # # 确定输入失败

日志记录，找出确切原因输入失败，GFID 的改进
冲突、 I/O 错误等。 不会安装日志中记录安全错误

在奴隶，安全错误是邮政处理，只有真正的错误
在大师中记录日志。

* * 修复 * *: [1207115] (https://bugzilla.redhat.com/show_bug.cgi?id=1207115)
[] 1210562() https://bugzilla.redhat.com/show_bug.cgi?id=1210562



# # # 改进 rm-rf 问题处理

逐次删除并创建了问题，处理这些问题
最小化。（固定不完全因为它取决于打开的问题

DHT)

* * 修复 * *: [1211037] (https://bugzilla.redhat.com/show_bug.cgi?id=1211037)


# # # 非根土力工程处复制简化

手动编辑 Glusterd vol 文件是通过引入简化
`gluster system:: mountbroker` command

* * 修复 * *: [1136312] (https://bugzilla.redhat.com/show_bug.cgi?id=1136312)

# # # 日志 Rsync 性能要求的基础上

Rsync 性能可以通过启用配置选项。后

这个土力工程处代表启动 rsync 性能记录在日志文件中可
邮政处理以得到有意义的指标。

* * 修复 * *: [764827] (https://bugzilla.redhat.com/show_bug.cgi?id=764827)

# # # 初始同步问题，因为在文件系统爬网期间上限比较

Bug 修复，固定逻辑错误在 Xsync 变化检测。上限是

在 xsync 爬网期间审议。土力工程处 rep XSync 缺掉的多个文件

考虑更新日志会照顾。但更新不会有

在启用地理复制之前创建的文件的完整细节。

Rsync/tarssh 失败时，土力工程处 rep 是现在能识别安全
错误并继续同步通过忽略这些问题。例如，

rsync 未能同步在硕士期间删除的文件
同步。这可以被忽略，因为该文件的链接，没有必要

请尝试同步。

* * 修复 * *: [1200733] (https://bugzilla.redhat.com/show_bug.cgi?id=1200733)


# # # 更新日志故障和砖故障处理

当砖过程落下，或任何的更新日志异常地质 rep
工人没有回 XSync 爬网。这是自 Xsync 坏

未能识别删除和重命名。现在这被防止，工人

去故障和等待复出那砖过程。


* * 修复 * *: [1202649] (https://bugzilla.redhat.com/show_bug.cgi?id=1202649)


# # # 加工后的工作目录中的存档更新历史

加工后的存档更新历史不生成空更新历史时
没有数据是可用的。这是在减少的大进步

在砖的 inode 消费。

* * 修复 * *: [1169331] (https://bugzilla.redhat.com/show_bug.cgi?id=1169331)


# # # 虚拟 xattr 触发同步

由于土力工程处代表工人重新启动时，我们使用历史更新历史。只有

`SETATTR` will be recorded when we touch a file. In previous versions,
重新触发文件同步是停止 geo rep、 触摸文件和开始
geo-replication. Now touch will not help since it records only `SETATTR`.
介绍了虚拟 Xattr retrigger 同步。没有土力工程处代表重新启动

必填。

* * 修复 * *: [1176934] (https://bugzilla.redhat.com/show_bug.cgi?id=1176934)


# # # SSH 密钥覆盖问题期间土力工程处代表创建

并行创建或多个地理 rep 会话创建被覆盖
质子交换膜键写的第一次。这会导致连接问题

当启动时土力工程处代表。

* * 修复 * *: [1183229] (https://bugzilla.redhat.com/show_bug.cgi?id=1183229)


# # # 所有权同步改进

土力工程处代表未能同步所有权信息从主机群集
到奴隶群集。

* * 修复 * *: [1104954] (https://bugzilla.redhat.com/show_bug.cgi?id=1104954)


# # # 奴隶节点故障切换处理的改进

当奴隶节点下山，师父工人相连，
砖将转到错误。现在它尝试连接到另一个奴隶节点

而不是等待奴隶节点能回来。

* * 修复 * *: [1151412] (https://bugzilla.redhat.com/show_bug.cgi?id=1151412)


# # # 支持的 ssh 密钥自定义位置

If ssh `authorized_keys` are configured in non standard location instead
of default `$HOME/.ssh/authorized_keys`. Geo-rep create was failing, now
这被支持。

* * 修复 * *: [1181117] (https://bugzilla.redhat.com/show_bug.cgi?id=1181117)
