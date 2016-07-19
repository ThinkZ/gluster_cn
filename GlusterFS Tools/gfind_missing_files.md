介绍
========
工具 gfind_missing_files.sh 可以用于查找丢失的文件中
土力工程处复制奴隶卷 GlusterFS。该工具使用多线程的履带

经营上的 brickpath 作为一个传递后端.glusterfs
工具参数。它没有一个奴隶卷中每个条目的属性

山要检查文件存在。该工具使用 aux-gfid-山

从而避免路径转换并潜在地节省了时间。

应在每个节点和每个 brickpath 在土力工程处复制上运行此工具
找到丢失的文件奴隶卷上的主音量。

脚本 gfind_missing_files.sh 是反过来使用包装器脚本
gcrawler 二进制做后端进行爬网。该脚本检测的 gfids

缺少文件并列出运行 gfid 路径转换脚本
丢失的文件，使用完整的路径名。

使用
=====
```sh
$bash gfind_missing_files.sh <BRICK_PATH> <SLAVE_HOST> <SLAVE_VOL> <OUTFILE>
BRICK_PATH-砖的完整路径
SLAVE_HOST-gluster 卷的主机名
SLAVE_VOL-Gluster 卷名
存档-输出文件，其中包含 gfids 丢失的文件
```

Gfid 路径转换为转换到 gfids 使用一种快速算法
路径，它是在某些情况下所有失踪 gfids 可能不是
转换为他们各自的路径。

示例输出 （126733 丢失的文件）
===================================
```sh
$ionice-c 2-n 7./gfind_missing_files.sh/砖/m3 糟糕奴隶 vol ~/test_results/m3-4.txt
电话履带式...
爬网完成。
gfids 跳过的文件都是可用的文件 /root/test_results/m3-4.txt
开始路径转换为 gfid
跳过的文件的路径名称都可用在文件 /root/test_results/m3-4.txt_pathnames
警告︰ 无法将一些 GFIDs 转换为路径，GFIDs 记录到 /root/test_results/m3-4.txt_gfids
使用 bash gfid_to_path.sh <brick-path> /root/test_results/m3-4.txt_gfids 将这些 GFIDs 转换为路径
缺少的文件总数︰ 126733
```
在这种情况下，需要额外的步骤将那些 gfids 转换为路径。
这可以使用如下所示︰
```sh
$bash gfid_to_path.sh <BRICK_PATH> <GFID_FILE>
BRICK_PATH-砖的完整路径。
GFID_FILE-OUTFILE_gfids 接到 gfind_missing_files.sh
```
运行该工具时要牢记的事情
============================================
1.运行此工具可能导致在每个后端文件系统的爬网
可以是密集的砖。确保不会影响上有正在运行的 I/O 上

RHS 卷，我们建议，此工具运行在一个低的 I/O 调度类
（尽力而为） 和优先事项。
```sh
$ionice-c 2-p < pid gfind_missing_files.sh >
```

2.我们建议不要打断的工具，它运行时
(例如，做 CTRL ^ C)。它是更好地等待该工具完成

执行。万一它中断，手动卸载奴隶卷。

```sh
<MOUNT_POINT> umount
```
