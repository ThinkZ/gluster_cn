GlusterFS 的释放过程
=============================

创建压缩文件
--------------

1.增加发行说明文档-发行 / 目录中
来源
2.合并后发行说明，创建类似 v3.6.2 这样的标记
3.将标签推送到 git.gluster.org
4.创建压缩包 [释放工作中
詹金斯] (http://build.gluster.org/job/release/)

通知成套设备供应商
----------------

通知的包装，我们需要创建的包。提供指向的链接

从詹金斯释放工作到 [包装的源文件
邮寄名单] (mailto:packaging@gluster.org)。所涉及的人的名单，

the package maintenance for the different distributions is in the `MAINTAINERS`
源中的文件。

创建一个新的跟踪 Bug 下一版本
---------------------------------------------

跟踪 bug 作为指导用于阻滞剂 bug 和应该被创造出来时才发放。创建一个


-文件 [Bugzilla 新的 bug] (https://bugzilla.redhat.com/enter_bug.cgi?product=GlusterFS)
-内容基于先前跟踪 bug，像是 [glusterfs-3.5.5] (https://bugzilla.redhat.com/show_bug.cgi?id=glusterfs-3.5.5)
-设置 '' 别名 '' （这是一个文本字段） 对 bug 的 ' glusterfs-a.b.c' a.b.c 哪里下一次要版本
-保存新的 bug
-你现在应该能够使用 'glusterfs-a.b.c' 来访问该 bug，请使用别名代替 BZ # 在 Url，或' '块' ' 字段
-不固定在此版本中，但被添加到追踪器的 bug 应该移到新的跟踪器


创建发布公告
---------------------------

创建发布公告 （这经常做虽然有的人
使得包）。发布公告的内容可以是

基于的发行说明，或至少应该指向它们的指针。

例子︰

-[博客] (http://blog.gluster.org/2014/11/glusterfs-3-5-3beta2-is-now-available-for-testing/)
-[释放
注意到] (https://github.com/gluster/glusterfs/blob/v3.5.3/doc/release-notes/3.5.3.md)

发送发布公告
-------------------------

Fedora/EL Rpm 一旦准备好了 （和任何其他的人准备的
然后），发送发布公告︰

-Gluster 邮件列表
-gluster-宣布，gluster-发展，gluster 用户
-Gluster 博客
-Gluster Twitter 帐户
-Gluster Facebook 页面
-Gluster LinkedIn 集团-贾斯汀已访问
-Gluster G +

关闭 Bug
----------

关闭了所有他们版本中包含的修补程序的 bug。
给释放与指针在 bug 报告中留一个便条
公告。

其他的事情要考虑
------------------------

-翻译吗？-是否有需要翻译的字符串？

