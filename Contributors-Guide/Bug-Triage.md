Bug 会审准则
=====================

-会审的 bug 是一项重要的任务;当正确地完成，它可以

减少报告 bug 和可用性之间的时间
极大地修复。

-Triager 应该专注于新的 bug，并尝试定义问题
很容易理解和尽可能准确。目标

triagers 是减少开发人员需要解决 bug 的时间
报告。

-Triager 就像一个助手，帮助信息
收集和可能调试一个新的 bug 报告。因为

triager 帮助编写 bug 之前开发商介入，它
新的社区成员，都可以很好的作用
在软件技术方面感兴趣。

-Triagers 将偶然发现许多种不同的等一系列的问题
从有关拼写错误或不清楚日志消息报道
内存泄漏导致崩溃或性能问题，在环境中
与存储几百台服务器。

没有人指望 triagers 可以编写所有的 bug 报告。因此大多数

开发人员将能够协助 triagers、 回答问题和
建议到调试和数据收集的方法。随着时间推移，triagers 得到

更多的经历，将依靠较少的开发人员。

* * Bug 会审可概括如下要点: * *

-是否有足够的信息，在 bug 说明？
-是一个重复的 bug？
-它分配正确的 GlusterFS 组件是吗？
-是的 Bugzilla 字段正确吗？
— — 意思是程序错误摘要正确吗？
-分配 bug 或添加人到"抄送"列表
-修复严重程度和优先级。
-Todo，如果在多个 GlusterFS 版本的 bug。
-添加适当的关键词为 bug。

以上各点详细的讨论如下。

关于 Bug 会审的每周会议
---------------------------------

我们尝试在对 Freenode \#gluster-meeting 每周见面。会议

日期和时间为下一次会议通常在中更新
[议程](https://public.pad.fsfe.org/p/gluster-bug-triage)。


入门教程︰ 找到会审的报告
---------------------------------------

有许多不同的技术和方法找到提交的报告
会审。一个简单的方法是使用这些预定义的 Bugzilla 报告 (

报告完全结构性的 URL，并且可以手动
修改）︰

-新 * * 错误 * *，而没有会审关键字 [Bugzilla
链接] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&f1=keywords&keywords=Triaged%2CFutureFeature&keywords_type=nowords&list_id=3014117&o1=nowords&product=GlusterFS&query_format=advanced&v1=Triaged)
-新 * * 功能 * *，而没有会审关键字 （标识
由 FutureFeature 关键字，只有项目感兴趣的可能
领导人） [Bugzilla
链接] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&f1=keywords&f2=keywords&list_id=3014699&o1=nowords&o2=allwords&product=GlusterFS&query_format=advanced&v1=Triaged&v2=FutureFeature)
-新的 glusterd bug: [Bugzilla
链接] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&product=GlusterFS&f1=keywords&o1=nowords&v1=Triaged&component=glusterd)
-新的 Replication(afr) bug: [Bugzilla
链接] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&component=replicate&f1=keywords&list_id=2816133&o1=nowords&product=GlusterFS&query_format=advanced&v1=Triaged)
-新的 distribute(DHT) bug: [Bugzilla
链接] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&component=distribute&f1=keywords&list_id=2816148&o1=nowords&product=GlusterFS&query_format=advanced&v1=Triaged)

-针对版本 3.6 新的 bug:
[< https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&product=GlusterFS&f1=keywords&f2=version&o1=nowords&o2=regexp&v1=Triaged&v2 > = \^3.6
Bugzilla 链接]
-针对版本 3.5 新的 bug:
[< https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&product=GlusterFS&f1=keywords&f2=version&o1=nowords&o2=regexp&v1=Triaged&v2 > = \^3.5
Bugzilla 链接]
-针对版本 3.4 新的 bug:
[< https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&product=GlusterFS&f1=keywords&f2=version&o1=nowords&o2=regexp&v1=Triaged&v2 > = \^3.4
Bugzilla 链接]

-[< https://bugzilla.redhat.com/page.cgi?id=browse.html&product=GlusterFS&product_version > = & bug\_status = 所有 & 选项卡 = 历史
bugzilla 跟踪器] （可以包括已会审 bug）

-[会审 NetBSD
bug] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&keywords=Triaged&keywords_type=nowords&op_sys=NetBSD&product=GlusterFS)
-[会审 FreeBSD
bug] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&keywords=Triaged&keywords_type=nowords&op_sys=FreeBSD&product=GlusterFS)
-[会审 Mac OS
bug] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&keywords=Triaged&keywords_type=nowords&op_sys=Mac%20OS&product=GlusterFS)

除了手动检查 Bugzilla bug 来会审，它也是
接收电子邮件时，新的可能
bug 归档或更新现有的 bug。

如果在任何点你感觉你不知道该怎么办某一特定
报告，请先问 [irc 或邮件列表
列出了] (http://www.gluster.org/community/index.html) 在更改之前
东西。

有足够的信息吗？
----------------------------

若要使报表有用，适用同样的规则作为为
[bug 报告指南](./Bug-Reporting-Guidelines.md)。


它很难概括是什么让一份好的报告。"平均"

记者是肯定很有帮助有好的步骤来重现，
GlusterFS 软件版本和测试/生产有关的信息
环境，GNU/Linux 分布。

如果记者是开发人员，重现步骤有时可以
省略了因为上下文是明显。* 然而，这可以创建问题

参与者需要找到他们的方式，因此强烈建议
若要列出的步骤来重现 issue.*

其他提示︰

-应该有每份报告的只有一件事。尽量不要混相关或

类似的寻找 bug，每份报告。

-它应该是可能调用了所描述的问题，在一些固定
点。"改进文档"或者"它运行速度慢"永远不可能

被称为固定，而"文件应涵盖的主题嵌入"
或者"页在 < http://en.wikipedia.org/wiki/Example > 应该加载
在五秒之内"会有一个准则。好的摘要

该 bug 也会帮助别人找到现有的 bug 和防止
归档的重复项。

-如果 bug 是一个图形化的问题，你可能想问的
要将附加到 bug 报告的屏幕截图。一定要问问，

截图不应包含任何机密信息。

它是一个重复吗？
------------------

一些报道 Bugzilla 已报道之前所以你可以
[搜索已经存在
报告] (https://bugzilla.redhat.com/query.cgi?format=advanced)。我们做

建议您不要花太多时间在上面;如果 bug 提起两次，

后来别人会将其标记为重复。如果 bug 是

复制，将其标记为重复下面的分辨率框中
通过设置注释字段 * * 关闭重复 * * 状态，不久
解释你行动在注释中的记者。标记为 bug 时

重复，它是需要引用原始的 bug。

如果你认为你发现重复，但你不是全部
当然，只添加了评论喜欢"这个 bug 看起来相关 bug XXXXX"（和
XXXXX 取代的 bug 数量） 所以别人可以看看，
帮助判断。

你也可以看看
https://bugzilla.redhat.com/page.cgi?id=browse.html&product=GlusterFS&product_version > = & bug\_status = 所有 & 选项卡 = 重复的
现有的重复项的列表

它被分配给正确的组件的 GlusterFS 吗？
-------------------------------------------------

请确保该 bug 分配正确的组件上。下面列出了

在 bugzilla GlusterFs 组件。

-访问控制-访问控制转换器
-边防局-伯克利 DB 后端存储
助推器-LD\_PRELOAD' 能够访问客户端
-建立-编译器、 包管理和平台特定警告
和错误
-cli-gluster 命令行
-核心-核心功能的文件系统
-分配-分发翻译 (以前 DHT)
-errorgen-错误创翻译
-保险丝-装载/保险丝译者和修补保险丝图书馆
-georeplication-Gluster 土力工程处复制
-glusterd-管理守护进程
-HDFS-GlusterFS 的 Hadoop 应用程序支持
ib--无限带宽动词运输
io 缓存-IO 缓冲区缓存翻译
io 线程-IO 线程性能翻译
-访问 glusterfs 卷 libglusterfsclient API 接口
以编程的方式
-锁定-POSIX 和内部锁
-日志-集中式日志记录，日志消息、 登录旋转等
-在 GlusterFS 的 nfs NFS 组件
-nufa-非-统一文件系统调度程序翻译
-对象-存储-对象存储
-移植-移植 GlusterFS 到不同的操作系统和
平台
-posix-POSIX (API) 根据后端存储
-协议-客户端和服务器协议转换器
-快速阅读-快速阅读翻译
配额-卷 & 目录配额翻译
-rdma RDMA 运输
预读-读前面的 （文件） 性能的转换器
-复制-复制翻译 （以前空燃比）
-rpc-RPC 层
-脚本-生成脚本、 装载脚本等。
stat-预取-Stat 回迁翻译
-条纹-条带化 (RAID 0) 群集翻译
-跟踪跟踪翻译
-运输-套接字 （IPv4，IPv6，unix，ib sdp） 和通用运输
代码
-未分类-未分类-要被重新归类为其他组件
-统一 — — 统一翻译和调度程序
-写-隐藏-写在性能转换器后面
-libgfapi-GlusterFS 的 Api
-测试-GlusterFS 测试框架
GlusterFS-gluster-hadoop-Hadoop 支持
gluster-hadoop-安装-自动化 Gluster 卷配置
Hadoop 环境
gluster-smb-gluster smb
-傀儡-gluster-GlusterFS 傀儡模块

搜索提示︰

-因为它往往是难为记者找到正确的地方 （产品
和组件） 哪里提交一份报告，还搜索重复项
你是外相同的产品和组件的 bug 报告
会审。
-使用常用词和与不同的组合，多试几次
因为可能有几种方法来描述同样的问题。如果你

选择适当的和共同的词，和你用多试几次
不同组合的那些，你确保具有匹配的
结果。
-下降的动词结尾 （例如搜索"删除"让你
报告为"删除"和"删除"），并尝试也类似
词 （例如搜索"删除目录浏览等等"和"消除"）。
-搜索使用的日期范围分隔符︰ bug 报表中的大多数
最近，所以你可以尝试提高搜索速度使用日期
转至"搜索的更改历史记录"[搜索的分隔符
页面] (https://bugzilla.redhat.com/query.cgi?format=advanced)。
示例︰ 搜索从"2011年-01-01"或"-730 d"（用于支付最后两个
年） 到"现在"。

字段是正确的？
-----------------------

# # # 摘要

有时摘要不好概述错误本身。你可能

想要更新 bug 汇总，使报告有别。A

好的标题可能包含︰

-（如果它被发现） 的根本原因简要说明
-一些症状的人正在经历

# # # 将人添加到"抄送"或者"分配对象"字段更改

通常情况下，开发商和潜在的受让人的地区，已
默认情况下，但有时报告抄送描述一般问题或
提起共同 bugzilla 产品。只有当你知道开发商谁

在 bug 报告，所涉领域的工作，如果你知道这些
开发人员接受得到过的重巡或分配给某些报告，你可以
将该人添加到抄送字段或甚至将分配到的 bug 报告
她/他。

要知道工作在哪个区域，检查了解组件所有者
您可以检查 glusterfs 代码目录的根目录中的"维护者"文件
或查询 [Gerrit] (http://review.gluster.org) （见的变化
[简化的开发工作流]()) /Developer-guide/Simplified-Development-Workflow.md


# # # 严重程度和优先级

请参阅下面有关可用的值的信息和他们
含义。

# # # 严重性

此字段是 bug 报告的外部权重拉下来
重要性和可以具有下列值︰

严重性 |定义

-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------
紧急 | 严重影响组织的任务关键型操作的灾难性问题。这可能意味着业务服务器、 开发系统或客户应用程序是下来或无法正常工作和程序不变通。

高 | 高影响问题，破坏了客户的运作，但还有一些产能
介质 | 部分非关键功能损失或损害某些操作，但允许客户执行关键任务的问题。这可能是小问题，有限的损失或没有损失的功能和对客户的功能影响有限

低 | 一般用法问题，有关产品增强或开发工作的建议
未指定 | 未指定的重要性

# # # 优先

此字段是 bug 报告的内部权重拉下来
重要性和可以具有下列值︰

优先 |定义

-------------|------------------------
紧急 | 极其重要
高 | 非常重要
介质 | 平均重要性
低 | 不是很重要
未指定 | 未指定的重要性


# # # 多个版本中存在的 bug

在会审过程中你可能会碰到某一特定的错误，这是目前
跨多个版本的 GlusterFS。这里是一个操作的过程︰


-我们应该有单独的 bug （我们应该每个版本
如果需要，克隆的 bug）
-在发布版本中的 bug 应该依靠 bug 的主线
（主分支） 如果 bug 是适用于主干线。
-这将确保此修复程序将会合而为一，硕士
第一分支，然后可以获得此修复程序移植到其他稳定
释放。

* 注意︰ 当 bug 取决于其他的 bug 时，这意味着该 bug 不能
固定除非其他的错误固定的 （取决于），或此 bug 停止其他
bug 被固定 （块） *

这里有一些例子︰

-对于 GlusterFS 3.5 引发 bug 和同一问题目前在
主线 （主分支） 和 GlusterFS 3.6
-克隆原始 bug 的主线。
-克隆另为 3.6。
-有 GlusterFS 3.6 bug 和 GlusterFS 3.5 bug '靠'
主线的 bug

-一个 bug 已存在的主线，并在出现相同问题
GlusterFS 3.5。
-为 GlusterFS 3.5 克隆原始的 bug。
-和有克隆的 bug （为 3.5) '取决于' '主流'
bug。

# # # 关键字

Bugzilla 许多预定义的搜索包含关键字。一个例子是

搜索的会审。如果 bug 是 '新' 和 '会审' 是没有

集，您 （作为 triager) 可以捡，使用此页可以会审它。当

bug 是 '新' 和 '会审' 是在关键字列表中，该 bug 是
准备由开发人员捡。

* * 会审 * *
︰ 一旦你做分流添加 * * 会审 * * 到关键字
bug，这样别人会知道会审的 bug 的状态。的

预定义的搜索，在页面顶部的这不然后会列出
会审的 bug 了。相反，该 bug 应该转移到了 [这

列表] (https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&keywords=Triaged&product=GlusterFS)。

* * EasyFix * *
︰ 加入 * * EasyFix * * 关键字，该 bug 获取添加到 [列表
应该是简单的对 fix](/Developer-guide/Easy-Fix-Bugs.md) 的 bug。
添加此关键字被鼓励为简单和定义良好的 bug
或增强功能。

* * 修补程序 * *
︰ 当问题的补丁程序已附加或包含的内联
添加 * * 补丁 * * 关键字，它是明确指出一些准备
为发展进行了已经。如果课程，它将有

一直好如果修补程序被送往 Gerrit 审查，但不是
每个人都愿意通过 Gerrit 跨栏时他们报告 bug。

您还可以添加 * * 补丁 * * 当 bug 是否已修复中的关键字
主线和修补已确定。添加一个链接

Gerrit 更改传播以便作出反向到一个稳定的版本
更简单。

* * 文件 * *
︰ 添加 * * 文件 * * 关键字时的 bug 已经被举报
文档。这有助于编辑和作家中的发现

他们可以解决的 bug。

* * 跟踪 * *
︰ 此关键字用于用于跟踪其他 bug 的 bug
特定的版本。例如 [3.6 跟踪器

bug] (https://bugzilla.redhat.com/showdependencytree.cgi?maxdepth=2&hide_resolved=1&id=glusterfs-3.6.0)

* * FutureFeature * *
︰ 此关键字用于 bug 使用要求
功能增强 (RFE-要求特征增强)
将来的版本的 GlusterFS。如果你通过请求打开的 bug

你想要在 GlusterFS 的下一个版本中看到的功能
请报告与此关键字。

将自己添加到抄送列表
---------------------------

通过将自己添加到抄送列表的 bug 报告，你改变，你
将会收到所有的批注和更改任何人对随访电子邮件
那个人的报告。这有助于学习有什么进一步的调查

别人做。您可以更改在 Bugzilla 上哪些操作的设置

您想要接收的邮件。

组会审的的 bug
---------------------

如果你碰到一个 bug 的 bug 或如果你认为任何 bug 应该去
彻底的 bug 会审小组，请设置 NEEDINFO 为 bugs@gluster.org
对该 bug。

解决 bug 报告
---------------------

请参阅 [Bug 报告生活 cycle](./Bug-report-Life-Cycle.md) 为
bug 状态和各项决议的含义。

会审的 Bug 的例子
-----------------------

这个 Bugzilla
[过滤]() https://bugzilla.redhat.com/buglist.cgi?bug_status=NEW&keywords=Triaged&keywords_type=anywords&list_id=2739593&product=GlusterFS&query_format=advanced

将列出新，会审的 Bug
