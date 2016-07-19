GlusterFS 的简化的开发工作流
=============================================

本网页提供了简化开发工作流模型的使用
GlusterFS 项目。这将给获取修补程序所需的步骤

接受到的 GlusterFS 源代码。

访问 [发展工作 Flow](./Development-Workflow.md) 一更多
工作流的详细的描述。

最初的制备
-------------------

围绕着 GlusterFS 开发工作流
[] Git() http://git.gluster.org/?p=glusterfs.git;a=summary

[] Gerrit(http://review.gluster.org) 和

[詹金斯](http://build.gluster.org)。


使用这些工具，需要一些初步的准备。

# # # 开发系统设置

你应该安装并在您的开发系统上安装 Git。使用您

分配特定的包马槽安装 git。安装后

配置 git。至少，设置 git 用户的电子邮件。要设置电子邮件

这样做，

$ git config--全球 user.name"名称"
$ git config--全球 user.email < 电子邮件地址 >

您还应该生成 ssh 密钥对如果你还没有做它。
要生成一个密钥对吗，

$ ssh 凯基

并按照说明进行操作。

下一步，安装 GlusterFS 的建设要求。请参阅

[建设 GlusterFS-生成要求]（./Building-GlusterFS.md#Build 要求）

实际的要求。

# # # Gerrit 安装程序

为了促进 GlusterFS，你首先应该注册上
[] gerrit(http://review.gluster.org)。


注册后，您将需要选择一个用户名，设置首选
电子邮件和上传 ssh gerrit 中的公钥。你可以从

gerrit 设置页。请确保你设置首选的电子邮件

您为 git 配置的电子邮件。

# # # 获取源码

Git 克隆 GlusterFS 源使用

<ssh://> <username>@review.gluster.org/glusterfs.git

（gerrit 用户名替换为 <username>）。

$ git 克隆 (ssh: / /) <username>@review.gluster.org/glusterfs.git

这将克隆到一个子目录中名为 glusterfs GlusterFS 源
与主分支签出。

至关重要的是，你使用此链接克隆，否则你不会
能够提交供审查 gerrit 的修补程序。

实际的发展
------------------

本节中的命令将运行在 glusterfs 源
目录。

# # # 创建发展分支

这被建议使用单独的地方发展分支机构为每个
你想要捐献给 GlusterFS 的变化。若要创建发展

科，第一次签出要上班上的上游分支和
更新它。对 GlusterFS 的上游分支模型的更多详细信息

可以发现在

[开发工作流-Branching\_policy](./Development-Workflow.md#branching-policy)。

例如，如果你要在主分支上开发

$ git 签出大师
$ git 拉

现在，创建一个新的分支从大师和开关到新的分支。它是

建议要有描述性的分支名称。这样做，


$ git 分支 <descriptive-branch-name>
git 签出 $ <descriptive-branch-name>

或，

$ git 结帐-b <descriptive-branch-name>

做到这两点在一个命令中。

# # # 黑客

一旦你已经切换到发展的分支，您可以执行
实际代码更改。[生成](./ 建筑 GlusterFS) 和测试

看看是否您更改工作。

# # # 测试

除非您的更改是非常轻微的琐碎，你还应当添加
测试您的更改。测试用来确保你做的更改

无意中都不是坏了。在测试的更多细节，可以发现在


[开发工作流-测试用例]()./Development-Workflow.md#test-cases

和
[开发工作流-回归测试和测试用例](./ Development-Workflow.md#regression-tests-and-test-cases)


# # # 回归测试

一旦您的更改正在运行的回归测试套件，以确保
你没有破坏什么。回归测试套件需要

工作 GlusterFS 安装，需要以 root 身份运行。要运行

回归测试套件，做

# 使安装
#./run-tests.sh

# # # 提交更改

如果你没有破坏什么，你现在可以提交您的更改。第一次

标识的文件，你修改/添加/删除使用 git 状态和
现阶段这些文件。

$ git 状态
$ git 中添加 < 修改过的文件列表 >

现在，这些使用提交更改

$ git commit-s

提供有意义的提交消息。提交消息策略是

所述

[开发工作流-提交策略](./Development-Workflow.md#commit-policy)。


至关重要的是你犯下与 '-s' 选项，将
签收与您已配置的电子邮件，提交配置 gerrit
拒绝其中不是签署过的修补程序。

# # # 提交供审查

若要提交更改为审查，运行 rfc.sh 脚本，

$。 /rfc.sh

该脚本将要求您输入一个 bugzilla bug id。每一次改变

提交给 GlusterFS 需要一个 bugzilla 条目将被接受。如果你这样做

不是已经有一个 bug id、 文件在 [Red Hat 的一个新的 bug
Bugzilla] (https://bugzilla.redhat.com/enter_bug.cgi?product=GlusterFS)。
如果该修补程序提交审查，rfc.sh 脚本会返回
gerrit 审查请求的 url。

更多细节上的 rfc.sh 脚本发售
[开发工作流-rfc.sh](./Development-Workflow.md#rfc.sh)。


审查过程
--------------

您的更改现在将由 GlusterFS 维护人员审查和
组件上 [gerrit] (http://review.gluster.org) 的所有者。您可以按照

参加审查进程上的变化在审查和 url。的

审查过程涉及几个步骤。

要知道组件所有者，您可以检查根中的"维护者"文件
glusterfs 代码目录的

# # # 自动验证

每次更改提交给 gerrit 触发自动初始
对 [詹金斯] (http://build.gluster.org) 的核查。自动化

验证可以确保您的更改不会中断生成和有
关联的错误 id。

更多的细节可以发现在

[开发工作流-自动核查](./Development-Workflow.md#auto-verification)。


# # # 正式审查

自动验证成功后，部分业主将
执行一次正式审查。如果他们是好与您的更改，他们会

给积极的审查。如果不是他们会给负面的评论和添加

评论的原因。

是审查限定符和那些不合资格的更多资料
可用在

[开发工作流-提交预选赛]()./Development-Workflow.md#submission-qualifiers

和
[开发工作流-提交那些不合资格](./Development-Workflow.md#submission-disqualifiers)。


如果你改变获取负面的评论，你需要到地址
评论并重新提交您的更改。

# # # 重新提交

切换到您发展分支和新更改地址
评审意见。生成并测试，新的变化是否正常工作。


阶段所做的更改，并提交新更改使用，

$ git 提交--修订

'— — 修订' 确保您更新您的原始提交所需和
不创建一个新的提交。

现在，您可以重新提交审查使用 rfc.sh 的更新的提交
脚本。

正式审查过程可能需要很长时间。增加的机会

迅速的评论，您可以添加组件所有者作为审阅者
在 gerrit 浏览页面。这将确保他们注意到的变化。的

组件所有者列表可以发现在维护中的现有文件
GlusterFS 源

# # # 核查

维护组件所有者已经给出了积极的审查后，将
运行回归测试套件对您的更改，以确认您的更改
工作并没有破坏任何东西。此验证通过

詹金斯的帮助。

如果验证失败，您将需要进行必要的更改，
重新提交更新的提交供审查。

# # # 验收

成功验证后，维护人员将合并/樱桃挑选 （作为
有必要） 你到上游的 GlusterFS 源的变化。您的更改

现在将可用在上游 git 回购供大家使用。
