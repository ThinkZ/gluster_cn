# # # 维护指南

GlusterFS 维护人员、 具有分维护人员维护人员释放
管理项目的代码库。子的维护者是为业主

特定领域/组件的源代码树。维护人员操作跨

在源树中的所有组件。释放的维护者是为业主

各种各样的发行分支机构 (释放 x.y) 目前在 GlusterFS
存储库。

在下面的指南，释放的维护者和小组的维护者是
同时也暗示时有维护人员指的是，除非它是
显式调用。

# # # 的维护者是的准则应遵守

1.确保高质量、 及时管理修补程序发送进行审阅。

2.对于合并到存储库中的修补程序，它被预期的
维护人员︰

> 合并仅拥有组件的修补程序。
b > 合并跨越多个组件补丁之前寻求所有维护人员的批件。
c > 确保回归测试通过合并之前的所有修补程序。
d > 确保回归测试陪所有修补程序提交。
e > 确保为用户感知行为或设计中一个引人注目的变化更新文档。
f > 鼓励从修补程序提交者将提高整体素质的基本代码的代码单元测试。
g > 不合并自己写的才 + 2 代码评审表决其他审阅人的修补程序。

3.合并成一个发布分支中的修补程序的责任
正常情况下将是释放维护。只有在

特殊情况下，维护人员 & 分维护人员将合并的修补程序
到一个发布分支。

4.释放维护人员将确保适当的批准
之前将修补程序合并到一个发布分支的维护者。

5.维护者有对社会的责任，它预期
维护人员︰

> 促进社区的所有方面。
b > 非常积极，在社区中可见。
c > 的目的，考虑更大的利益置于个人利益之上的社会。
d >，乐于听取用户的反馈。
e > 解决问题及影响用户的问题。
f > 以身作则。

# # # 查询准则

任何疑问或意见这些准则可以路由到
在 gluster 点组织 gluster 发展

# # # 在 Gerrit 修补程序

Gerrit 可用于列表中的修补程序需要审查和 （或） 可以得到
合并。某些查询已为此，编辑中的搜索框

Gerrit，使自己的变化︰

-[3.5 打开审查/验证 （非
rfc)](http://review.gluster.org/#/q/project:glusterfs+branch:release-3.5+status:open+%28label:Code-Review%253D%252B1+OR+label:Code-Review%253D%252B2+OR+label:Verified%253D%252B1%29+NOT+topic:rfc+NOT+label:Code-Review%253D-2,n,z)
-[所有打开的 3.5 补丁 （非
rfc)](http://review.gluster.org/#/q/project:glusterfs+branch:release-3.5+status:open+NOT+topic:rfc,n,z)
-[打开 NFS （硕士
分公司）](http://review.gluster.org/#/q/project:glusterfs+branch:master+status:open+message:nfs,n,z)

结合 Gerrit 查询，可以使用其他选项和
具有支持文件名/目录匹配 （上面的查询不这样做）。
转到 [设置] (http://review.gluster.org/#/settings/projects)
你 Gerrit 配置文件，并输入像这些筛选器︰

![gerrit 看项目]() https://cloud.githubusercontent.com/assets/10970993/7411584/1a26614a-ef57-11e4-99ed-ee96af22a9a1.png

