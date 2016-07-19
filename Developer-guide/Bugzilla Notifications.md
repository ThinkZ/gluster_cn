# 过滤电子邮件通知
Bugzilla 发送带有许多标题的电子邮件。下面的标题可以是非常有用的排序电子邮件︰

* X-Bugzilla-手表-原因︰ 包含字段 （如分配给 '，'抄送') 的 bug 和电子邮件地址被监视 (bugs@gluster.org)
* X-Bugzilla-产品︰ 总是应该 'GlusterFS' Gluster 的 bug
* X-Bugzilla-组件︰ 该组件的 bug （像 'glusterd')
* X-Bugzilla-状态︰ 你可能想要筛选特殊文件夹中的新建 bug
* X-Bugzilla-关键词︰ 新的捐助者可能想要筛选 bug 标记有 [EasyFix] (http://gluster.readthedocs.org/en/latest/Developer-guide/Easy-Fix-Bugs)

它建议筛选从在一个专用的文件夹列表中的电子邮件。一些用户只移动/标签上某些邮件头的邮件并删除所有内容，通过邮寄名单。


例如，若要接收邮件的组件 * glusterd *

使用 Zimbra 邮件客户端，
转到 * 首选项 *
* 选择 * 新筛选器 *
* 添加 * 筛选名称 *
* 包括和应用作为规则
* * * 标题命名 * * _X Bugzilla Component_ * * 包含 * * _glusterd_

使用 Mozilla 雷鸟邮件客户端，
转到 * 工具 *
* 选择 * 消息筛选器 *
* 选择 * 新。*

* 添加 * 筛选名称 *
* 在规则部分中，选择 * 自定义 *
* 添加新消息标头名称作为 * X-Bugzilla-组件 *
* 现在包括和应用作为规则
* _X Bugzilla Component_ * * 包含 * * _glusterd_


# 邮件列表
[Bugs@gluster.org] (http://gluster.org/mailman/listinfo/bugs) 邮寄列表获取默认添加到新的 bug。列表中接收所有更新在 Gluster 的所有 bug，任何人都可以订阅。


# '看' Bugzilla 中的用户
它是可能使用 Bugzilla"手表"函数来接收更新 bug 报告上 CC bugs@gluster.org 伪用户在哪里。

配置说明︰

1.转到 https://bugzilla.redhat.com 和登录

2.转到您的 [首选项] (https://bugzilla.redhat.com/userprefs.cgi)

3.去 [电子邮件选项卡] (https://bugzilla.redhat.com/userprefs.cgi?tab=email)

4.在页面底部的根据"用户观看"，添加"bugs@gluster.org"

![alt 文本](../images/Bugzilla-watching.png"Bugzilla 看")


5.其结果

![alt 文本](../images/ Bugzilla-watching_bugs@gluster.org.png"Bugzilla-watching_bugs@gluster.org")


# RSS 源
下面的链接可以添加到 RSS 阅读器 (喜欢 Mozilla 雷鸟或 [feedly] (https://feedly.com))。当你喜欢的 bug bugzilla 网站上的列表时，打开 RSS 提要在您的浏览器和删除 * ctype = 原子 * 从 URL 的末尾。


为 [Bug Triagers] (http://gluster.readthedocs.org/en/latest/Contributors-Guide/Bug-Triage/):
* [新 Gluster Bug 会审] (http://goo.gl/llNxe9) （任何版本）
* [新 Gluster 3.4 Bug] (http://goo.gl/QwF1cM)
* [新 Gluster 3.5 Bug] (http://goo.gl/agtGd2)
* [新 Gluster 3.6 Bug] (http://goo.gl/mta1s8)
* [新 GlusterD Bug] (http://goo.gl/8H3NMV)
* [新 Gluster/NFS Bug] (http://goo.gl/ZHKn3W)
* [新土力工程处 replucation Bug] (http://goo.gl/1mKTCf)

为开始开发人员想要发送其 [[EasyFix Bug | 先修补程序]]:
* [Gluster EasyFix Bug] (http://goo.gl/OpQwlv)

为组件的维护者和开发人员寻找 bug 就开始工作︰
* [会审 Gluster 3.4 Bug] (http://goo.gl/R9347I)
* [会审 Gluster 3.5 Bug] (http://goo.gl/hNHVnU)
* [会审 Gluster 3.6 Bug] (http://goo.gl/O8qdZo)
* [会审新 Gluster/NFS Bug] (http://goo.gl/MrHwb0)

可以得到测试的 bug
* ...

随时添加更多饲料到此页自己时，要求他们通过 [gluster-发展] (gluster-devel@gluster.org) 或 [Github 问题跟踪器] (https://github.com/gluster/glusterdocs/issues)。



