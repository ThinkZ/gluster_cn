Bug 经常得到固定在主前释放分支。Bug 是什么时候

固定在主分支，它可能是可取或必要的
稳定的分支。要修复在稳定的分支，我们需要通过

修复到稳定的分支。

在社会中的任何人都可以建议通过。如果您对感兴趣

建议通过，请检查 [通过
Wishlist](./Backport-Wishlist.md)。

本页面描述了通过简单的改变所需的步骤。更改

这并不适用于清洁会需要一些手动修改和使用
`git cherry-pick` may not always be the easiest solution.

# # 通过 Git 命令行
1.Git 克隆 GlusterFS 代码

git 克隆 ssh://username@review.gluster.org/glusterfs

2.创建和签出一个新的分支，为你的工作，基于分支
通过版本

git 结帐-t-b bug-123456/3.5 版起源/释放-3.5

3.樱桃挑选变化从主服务器。

$ git 挑选 x a0b1c2d3e4f5
-验证，已在主分支合并更改。

4.纠正更新/提交消息。

$ git 提交-s — — 修正 — — date="$(date)"
[这是一个示例](https://github.com/gluster/glusterfs/commit/40407afb529f6e5fa2f79e9778c2f527122d75eb) 的已经很好地描述为通过提交消息。注意到修补程序元数据像虫子，更改 ID 和回顾上标记的缩进。也是是从主分支摘樱桃的原始的提交 id。

-请确保报价审查标签
-更新 BUG 引用，指向用于此目的的 BUG
特定的发布分支
-添加签名关闭的标签

5.  Run `./rfc.sh` to post the backport for review.

。 /rfc.sh

# # 通过 Gerrit web 界面
1.从浏览器情况下导航 Gerrit 的必要更改。

2.点击 '樱桃挑选' 按钮。你现在将看到对话框中包含两个可编辑字段。第一次

一是指定到这个特定的变更需要的分支
樱桃采摘，二是为修改已经存在的提交消息。

3.开始在收视字段中输入分支名称，您可以选择
从列表中所需分支可用分支。

4.确保您只能编辑提交消息从以下︰

* BUG︰ 替换为正确的 bug id 举报的变化要 backported 的分支。
* 前缀所有其他行除了樱桃提交消息，签名-关闭-由，挑选和更改 Id 线更有效地比符号和空格 ' > '，并重新安排整体上具有以下格式︰

< 提交消息 >
. . .
</commit message>

> 审查上-: http://review.gluster.org/ <change-number>
> 烟︰ Gluster 构建系统 < jenkins@build.gluster.com >
> CentOS 回归︰ Gluster 构建系统 < jenkins@build.gluster.com >
> NetBSD 回归︰ NetBSD 构建系统 < jenkins@build.gluster.org >
> 审阅者-: username1 < user1@example.com >
> 审阅者-: username2 < user2@domain.com >
> 审阅者-: username3 < user3@abcd.com >

（樱桃采摘从提交 <hash-key>）

更改 Id: <change-id>
BUG: <bug-id>
签名-关闭-者︰ 用户名 < user@example.com >

5.点击 '樱桃挑选变化'。你现在会重定向到新 url 更改。


6.单击命名为主题字段旁的编辑按钮。

7.添加 '主题' 如下︰

bug <bug-id>
注:-<bug-id> 替换该分支所需的错误 id。

8.点击 '更新' 或 '提交'。

提交后片，请确保移动到 bug * 职位 *
状态。
