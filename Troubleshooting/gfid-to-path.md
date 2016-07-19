#Convert GFID 到路径

GlusterFS 内部文件标识符 (GFID) 不是独特的每一个 uuid
跨整个群集的文件。这是类似于在 inode 号

正常的文件系统。GFID 的文件存储在名为其 xattr

`trusted.gfid`.

# # #Special 装载使用 [gfid 访问转换器] [1]:
~~~
装载-t glusterfs-o aux-gfid-山 vm1:test /mnt/testvol
~~~

Assuming, you have `GFID` of a file from changelog (or somewhere else).
For trying this out, you can get `GFID` of a file from mountpoint:
~~~
getfattr-n glusterfs.gfid.string /mnt/testvol/dir/file
~~~


---
从 GFID (方法 1) # # #Get 文件路径︰
**(Lists hardlinks delimited by `:`, returns path as seen from mountpoint)**

# # #Turn pgfid 生成选项
~~~
gluster 卷设置的测试生成-pgfid 上
~~~
Read virtual xattr `glusterfs.ancestry.path` which contains the file path
~~~
getfattr-n glusterfs.ancestry.path-e 文本 /mnt/testvol/.gfid/ <GFID>
~~~

* * 示例: * *
~~~
[root@vm1 glusterfs] # ls-il/产妇和新生儿破伤风/testvol/dir /
总 1
10610563327990022372-rw-r-r — —。2 根根 3 Jul 17 18:05 文件

10610563327990022372-rw-r-r — —。2 根根 3 Jul 17 18:05 作品 3


[root@vm1 glusterfs] # getfattr-n glusterfs.gfid.string /mnt/testvol/dir/file
getfattr︰ 删除前导 '/' 从绝对路径名称
# 文件︰ 产妇和新生儿破伤风/testvol/目录/文件
glusterfs.gfid.string="11118443-1894-4273-9340-4b212fa1c0e4"

[root@vm1 glusterfs] # getfattr-n glusterfs.ancestry.path-e 文本 /mnt/testvol/.gfid/11118443-1894-4273-9340-4b212fa1c0e4
getfattr︰ 删除前导 '/' 从绝对路径名称
# 文件︰ mnt/testvol/.gfid/11118443-1894-4273-9340-4b212fa1c0e4
glusterfs.ancestry.path="/dir/file:/dir/file3"
~~~

---
从 GFID (方法 2) # # #Get 文件路径︰
**(Does not list all hardlinks, returns backend brick path) * *
~~~
getfattr-n trusted.glusterfs.pathinfo-e 文本 /mnt/testvol/.gfid/ <GFID>
~~~

* * 示例: * *
~~~
[root@vm1 glusterfs] # getfattr-n trusted.glusterfs.pathinfo-e 文本 /mnt/testvol/.gfid/11118443-1894-4273-9340-4b212fa1c0e4
getfattr︰ 删除前导 '/' 从绝对路径名称
# 文件︰ mnt/testvol/.gfid/11118443-1894-4273-9340-4b212fa1c0e4
trusted.glusterfs.pathinfo="(<DISTRIBUTE:test-dht> <POSIX(/mnt/brick-test/b):vm1:/mnt/brick-test/b/dir//file3>)"
~~~

---
从 GFID (方法 3) # # #Get 文件路径︰
https://gist.github.com/semiosis/4392640

---
# # #References 和链接︰
[posix︰ 占位符为 GFID 的路径转换]() http://review.gluster.org/5951

[1]: https://github.com/gluster/glusterfs/blob/master/doc/features/gfid-access.md
