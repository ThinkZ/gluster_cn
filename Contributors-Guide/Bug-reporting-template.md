Bug 描述模板
----------------------------
此模板应在行 [报告 guidelines](./Bug-Reporting-Guidelines.md) 的 bug。
该模板是替换为默认描述模板存在于 [Bugzilla] (https://bugzilla.redhat.com)

进展中的工作

------------------------------------------------------------------------

问题描述︰

安装 GlusterFS 软件包的版本︰

包用于从该位置︰

GlusterFS 群集信息︰

-卷的数量
-卷名
卷的特定问题看到 [如适用]
-类型的卷
如果可用容量选项
-   Output of `gluster volume info`
-   Output of `gluster volume status`
-得到带有问题的卷的 statedump

`   $ gluster volume statedump `<vol-name>

客户端信息
-OS 类型︰
-装载类型︰
-OS 版本︰

如何重现︰

重现的步骤︰

-   1.
-   2.
-   3.

实际的结果︰

预期的成果︰

日志信息︰

-作为注释提供可能出现的问题、 警告、 错误，到 bug
-寻找自我修复日志，再平衡日志，glusterd 日志、 砖日志，装载日志/nfs 日志/smb 日志中的问题/警告/错误
-添加整个日志作为附件，如果它是很大，无法粘贴作为注释

附加信息︰

[Bug\_reporting\_guidelines]: 故障报告指南"wikilink"
[Bugzilla]: https://bugzilla.redhat.com
