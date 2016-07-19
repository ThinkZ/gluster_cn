#Managing glusterd 服务

安装后 GlusterFS，您必须启动 glusterd 服务。的

glusterd 服务作为 Gluster 弹性卷管理器，
监督 glusterfs 进程和统筹动态卷
操作，如添加和删除卷跨多个存储
服务器无中断。

本节描述如何在启动 glusterd 服务
通过以下方式︰

-[启动和停止 glusterd Manually](#manual)
-[开始 glusterd Automatically](#auto)

> * * 注意 * *: 您必须启动 glusterd GlusterFS 的所有服务器上。

<a name="manual"></a>
##Starting 和手动停止 glusterd

本节介绍如何手动启动和停止 glusterd

-要手动启动 glusterd，输入以下命令︰

    `# /etc/init.d/glusterd start `

-对手动停止 glusterd，输入以下命令︰

    `# /etc/init.d/glusterd stop`

<a name="auto"></a>
##Starting glusterd 自动

本节描述如何配置到系统自动
每次系统启动时，启动 glusterd 服务。

# # #Red 帽子和 Fedora 发行版

基于 Red Hat 将系统配置为自动启动 glusterd
服务每次系统启动，输入从以下
命令行︰

`# chkconfig glusterd on `

# # #Debian 和衍生品喜欢 Ubuntu

基于 Debian 的将系统配置为自动启动 glusterd
服务每次系统启动，输入从以下
命令行︰

`# update-rc.d glusterd defaults`

除了顶红色的帽子和 Debain # # #Systems

若要配置系统但 Red Hat 或 Debian 自动启动
glusterd 服务每次系统启动时，输入以下内容
到 the*/etc/rc.local* 文件中的条目︰

`# echo "glusterd" >> /etc/rc.local `
