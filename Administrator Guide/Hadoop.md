#Managing Hadoop 兼容存储

GlusterFS 为 Apache Hadoop 提供兼容性，它使用
标准的文件系统 Api 可用在 Hadoop 提供新的存储
对于 Hadoop 部署选项。现有的 MapReduce 应用 can

无缝地使用 GlusterFS。这种新功能打开内的数据

Hadoop 部署到任何基于文件或基于对象的应用程序。

# #Advantages

以下是与 Hadoop 兼容存储的优势
GlusterFS:

-提供内同时基于文件和基于对象的访问
Hadoop。
-消除了集中的元数据服务器。
-提供与 MapReduce 应用程序兼容性和重写
并不是必需的。
-提供故障容错文件系统。

# Requisites

以下是安装 Hadoop 兼容的先决条件
存储︰

Java 运行时环境
-getfattr-命令行实用程序

##Installing 和配置 Hadoop 兼容存储

请参阅设置在 https://forge.gluster.org/hadoop/pages/ConfiguringHadoop2 的详细的说明

# # # 资源

-[Apache Hadoop] (http://hadoop.apache.org/) 项目主页
-[社区 GlusterFS Q&A
测试版] (http://community.gluster.org/t/3-3-beta/) 和 Hadoop
-[下载 GlusterFS 3.3] (http://www.gluster.org/download/) 与
Hadoop 连接器
-[GlusterFS 3.3 测试版资源页] (https://github.com/shravantc/Gluster-Documentations/blob/master/Administrative-Guide/How_To_Guide/gluster3.3Beta.md)
-下载 GlusterFileSystem （hadoop 插件）︰
< > http://ec2-54-243-59-213.compute-1.amazonaws.com/archiva/browse/org.apache.hadoop.fs.glusterfs/glusterfs-hadoop
