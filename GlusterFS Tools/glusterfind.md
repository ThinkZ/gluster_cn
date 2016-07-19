# glusterfind-找到已修改的文件/目录的工具

这有助于从 GlusterFS 卷使用更新日志/查找得到完全/增量列表的文件/目录的工具。在 Gluster 卷检测已修改的文件具有挑战性。目录上的 Readdir 导致网络的多个调用，因为跨节点目录中的文件进行分布。


此工具应运行在一个节点，并将获取卷信息获取的节点和砖路径的列表。每个砖，它产生的过程的中各自的节点运行爬网程序的命令。爬网程序将运行在砖 FS (xfs，ext4 等) 而不是在 Gluster 山。爬网程序生成列表的最后运行后修改的文件或后会话创建的输出文件。


# # 会话管理

Create a glusterfind session to remember the time when last sync or processing complete. For example, your backup application runs every day and gets incremental results on each run. The tool maintains session in `$GLUSTERD_WORKDIR/glusterfind/`, for each session it creates and directory and creates a sub directory with Volume name. (Default working directory is /var/lib/glusterd, in some systems this location may change. To find Working dir location run `grep working-directory /etc/glusterfs/glusterd.vol` or `grep working-directory /usr/local/etc/glusterfs/glusterd.vol` if source install)

For example, if the session name is "backup" and volume name is "datavol", then the tool creates `$GLUSTERD_WORKDIR/glusterfind/backup/datavol`. Now onwards we refer this directory as `$SESSION_DIR`.

=> => [删除] 后预创建

一旦创建了会话，我们可以用两个步骤 Pre 和 Post 运行该工具。若要收集的修改过的文件列表后的创建时间或上次运行时间，我们需要调用前命令。前命令查找修改过的文件，并生成输出文件。消费者可以检查前命令的退出代码，并开始处理这些文件。作为邮政处理步骤运行 post 命令来更新会话时间按最新运行。


例如，备份实用程序运行前命令和获取的文件/目录更改的列表。那些将文件同步到备份目标并通过调用 Post 命令通知到 glusterfind。


At the end of Pre command, `$SESSION_DIR/status.pre` status file will get created. Pre status file stores the time when current crawl is started, and get all the files/dirs modified till that time. Once Post is called, `$SESSION_DIR/status.pre` will be renamed to `$SESSION_DIR/status`. content of this file will be used as start time for the next crawl.

During Pre, we can force the tool to do full find instead of incremental find. Tool uses `find` command in brick backend to get list of files/dirs.

When `glusterfind create`, in that node it generates ssh key($GLUSTERD_WORKDIR/glusterfind.secret.pem) and distributes to all Peers via Glusterd. Once ssh key is distributed in Trusted pool, tool can run ssh commands and copy files from other Volume nodes.

When `glusterfind pre` is run, it internally runs `gluster volume info` to get list of nodes and respective brick paths. For each brick, it calls respective node agents via ssh to find the modified files/dirs which are local them. Once each node agents generates output file, glusterfind collects all the files via scp and merges it into given output file.

When `glusterfind post` is run, it renames `$SESSION_DIR/status.pre` file to `$SESSION_DIR/status`.

# # 更新日志模式和路径转换为 GFID

增量查找使用更新历史得到的 GFIDs 修改创建列表。任何应用程序期望而不是 GFID 的文件路径。他们是没有标准/轻松地将从 GFID 转换为路径。


如果我们设置在卷 GlusterFS 生成 pgfid 选项启动作为 xattr 上任何条目 fop 文件中记录每个文件的父目录 GFID。

trusted.pgfid。<GFID>= NUM_LINKS


从 GFID 转换为路径，我们可以装入卷与 aux-gfid-装载选项，并通过 getfattr 查询获取路径信息。

getfattr-n glusterfs.ancestry.path-e 文本 /mnt/datavol/.gfid/ <GFID>

这种方法很慢，为通过 xattr 和读取请求的文件获取父 GFID 对该目录获取的文件具有相同的 inode 号到 GFID 文件。为了提高性能，glusterfind 使用生成 pgfid 选项，但不要使用 getfattr 上装载它从砖后台获取详细信息。glusterfind 一次收集所有父 GFIDs，开始爬网的每个目录。而不是处理路径转换为一个 GFID，它获取所有的编号输入 GFIDs 和筛选一边读父目录的 inode。


Above method is fast compared to `find -samefile` since it crawls only required directories to find files with same inode number as GFID file. But pgfid information only available when a lookup is made or any ENTRY fop to a file after enabling build-pgfid. The files created before build-pgfid enable will not get converted to path from GFID with this approach.

Tool collects the list of GFIDs failed to convert with above method and does a full crawl to convert it to path. Find command is used to crawl entire namespace. Instead of calling find command for every GFID, glusterfind uses an efficient way to convert all GFID to path with single call to `find` command.

# # 使用

# # # 创建会话

glusterfind 创建 SESSION_NAME VOLNAME [— — 力]
glusterfind 创建 — — 帮助

Where, SESSION_NAME is any name without space to identify when run second time. When a node is added to Volume then the tool expects ssh keys to be copied to new node(s) also. Run Create command with `--force` to distribute keys again.

例子，

glusterfind 创建 — — 帮助
glusterfind 创建备份 datavol
glusterfind 创建 antivirus_scanner datavol
glusterfind 创建备份 datavol — — 力

# # # 前命令

glusterfind 预 SESSION_NAME 文件的卷名称输出
glusterfind 前 — — 帮助

我们不需要指定卷名称，因为会话已经有细节。文件列表中的将填充到存档。


To trigger the full find, call the pre command with `--full` argument. Multiple crawlers are available for incremental find, we can choose crawl type with `--crawl` argument.

例子，

glusterfind 前备份 datavol /root/backup.txt
glusterfind 前备份 datavol /root/backup.txt — — 全

# 更新日志基于履带，只适合增量
glusterfind 前备份 datavol /root/backup.txt — — 履带式 = 更新日志

# 查找基于履带，作品的完整和增量
glusterfind 前备份 datavol /root/backup.txt — — 履带式 = brickfind

输出文件包含列表中的文件/目录与卷装入，如果我们需要与任何路径，然后，有绝对路径前缀

glusterfind 前备份 datavol /root/backup.txt — — 文件前缀 = / 产妇和新生儿破伤风/datavol /

# # # 列表命令

获取会话和各自会话时间列表

glusterfind 列表 [— — 会议 SESSION_NAME] [— — 卷卷名称]

例子，

glusterfind 列表
glusterfind 列表--会话备份

示例输出，

会议卷会话时间
---------------------------------------------------------------------------
备份 datavol 2015-03-04 17:35:34

# # # Post 命令

glusterfind 邮政 SESSION_NAME 卷名称

例子，

glusterfind 后备份 datavol

# # # 删除命令

glusterfind 删除 SESSION_NAME 卷名称

例子，

glusterfind 删除备份 datavol


# # 添加更多的抓取工具

Adding more crawlers is very simple, Add an entry in `$GLUSTERD_WORKDIR/glusterfind.conf`. glusterfind can choose your crawler using `--crawl` argument.

[爬虫]
changelog=/usr/libexec/glusterfs/glusterfind/changelog.py
brickfind=/usr/libexec/glusterfs/glusterfind/brickfind.py

For example, if you have a multithreaded brick crawler, say `parallelbrickcrawl` add it to the conf file.

[爬虫]
changelog=/usr/libexec/glusterfs/glusterfind/changelog.py
brickfind=/usr/libexec/glusterfs/glusterfind/brickfind.py
parallelbrickcrawl = / 根/parallelbrickcrawl

自定义爬网程序可以是可执行的脚本/二进制接受卷名称、 砖路径，output_file 和开始时间 （和可选的调试标志）

例如，

/ 根/parallelbrickcrawl SESSION_NAME 体积 BRICK_PATH 文件的输出 START_TIME [— — 调试]

Where `START_TIME` is in unix epoch format, `START_TIME` will be zero for full find.

# # 已知问题

1.已删除的文件不会上市，因为我们不能将 GFID 转换为路径，如果删除文件/目录。
2.如果重命名将上市只有新名称。
3.所有的 hardlinks 将上市。
