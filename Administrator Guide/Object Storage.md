# SwiftOnFile

SwiftOnFile 项目使 GlusterFS 卷 openstack 用作后端
斯威夫特-分布式的对象存储。这允许对象放在斯威夫特的 RESTful

要根据文件在文件系统接口和副反之亦然，即文件访问 API
创建超过文件系统可以作为对象访问接口 （限本机 NFS/保险丝）
在斯威夫特的基于 Rest 的 API。

SwiftOnFile project was formerly known as `gluster-swift` and also as `UFO
（统一文件和对象） 在那之前。有关 SwiftOnFile 的详细信息可以

点击此处 [] (https://github.com/swiftonfile/swiftonfile/blob/master/doc/markdown/quick_start_guide.md)。
在 gluster 斯威夫特 （现在已经过时） 和 swiftonfile 的工作有差异
项目。可以发现旧 gluster swift 代码和相关文档

在 [冰窖处] (https://github.com/swiftonfile/swiftonfile/tree/icehouse)
swiftonfile 回购。

# # SwiftOnFile vs gluster 斯威夫特

|                                                                                     Gluster-Swift                                                                                      |                                                                                              SwiftOnFile                                                                                               |

|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| One GlusterFS volume maps to and stores only one Swift account. Mountpoint Hierarchy: `container/object`                                                                                | One GlusterFS volume or XFS partition can have multiple accounts. Mountpoint Hierarchy: `acc/container/object`                                                                                          |
|越权帐户服务器、 容器服务器和目标服务器。我们需要保持与上游 Swift 同步和经常可能需要代码更改或解决方法以支持新的快捷功能 |实现只对象服务器。很少需要追赶 swift 作为代理、 集装箱和帐户水平很可能会与 SwiftOnFile 兼容，因为它是只存储策略的新增功能。|

|不使用星展银行帐户和容器。容器上市涉及文件系统爬网。帐户/容器的头给不准确或过时的结果没有 FS 爬网。            |斯威夫特的 DBs 用于存储帐户和容器的信息。帐户或容器的上市并不涉及 FS 爬网。头上到帐户/容器 — — 能够支持帐户配额的准确信息。|

|获取容器和帐户的清单上文件系统中的实际文件。                                                                                                                       |相处的容器和帐户只列出对象放在斯威夫特。通过文件系统接口创建的文件不显示在容器和对象的列表。                                              |

|独立部署所需，并不与现有迅速群集将集成。                                                                                                     |与任何现有的迅速部署作为存储策略集成。                                                                                                                                     |


