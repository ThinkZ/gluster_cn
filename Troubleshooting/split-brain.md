从文件裂恢复的步骤。
======================================

快速启动︰
============
1.获得在裂是该文件的路径︰
> 它可以获得通过
>       a) The command `gluster volume heal info split-brain`.  
> b） 标识为其执行文件操作的文件
从客户端保持输入/输出错误。

2.关闭的应用程序打开此文件的装入点。
如越南船民，他们需要被关闭。

3.决定正确的副本︰
> 这是通过观察空燃比扩展的更新日志文件的属性
使用 getfattr 命令; 砖然后确定类型的裂

（裂，元数据裂，数据项裂或裂致
gfid-不匹配）;和最后确定哪些砖块包含正确的副本

文件。
>   `getfattr -d -m . -e hex <file-path-on-brick>`.  
也有可能一砖可能包含正确的数据时
其他可能包含正确的元数据。

4.重置上包含 brick(s) 的有关扩展的属性
坏副本的文件数据元使用 setfattr 命令。
>   `setfattr -n <attribute-name> -v <attribute-value> <file-path-on-brick>`

5.通过从客户端执行查找触发自我修复的文件︰
>   `ls -l <file-path-on-gluster-mount>`

步骤 3 到 5 步骤的详细的说明︰
===========================================
要理解如何解决裂我们需要知道如何解释
扩展属性的空燃比更新日志。

Execute `getfattr -d -m . -e hex <file-path-on-brick>`

* 示例︰
[root@store3 ~] # getfattr-d-e 十六进制-file.txt 下午砖 /
\#file: /: file.txt 砖一
security.selinux=0x726f6f743a6f626a6563745f723a66696c655f743a733000
trusted.afr.vol-客户端-2 = 0x000000000000000000000000
trusted.afr.vol-客户端-3 = 0x000000000200000000000000
trusted.gfid=0x307a5c9efddd4e7c96e94fd4bcdcbd1b

The extended attributes with `trusted.afr.<volname>-client-<subvolume-index>`
由空燃比用于维护文件的更新日志。值

`trusted.afr.<volname>-client-<subvolume-index>` are calculated by the glusterfs
客户端 （保险丝或 nfs 服务器） 进程。当 glusterfs 客户端修改文件

或目录，客户端将联系每个砖和更新的更新日志扩展
根据砖的响应特性。

'子宗卷指数' （砖数-1） 只不过是在
`gluster volume info <volname>` output.

* 示例︰
[root@pranithk-laptop ~] # gluster 卷信息 vol
卷名︰ 卷
类型︰ 分布式复制
卷 ID: 4f2d7849-fbd6-40a2-b346-d13420978a01
状态︰ 创建
砖的数目︰ 4 x 2 = 8
传输类型︰ tcp
砖头︰
砖答︰ pranithk 笔记本电脑︰ gfs/砖一
砖 b: pranithk 笔记本电脑: / gfs/砖-b
砖 c: pranithk 笔记本电脑: / gfs/砖-c
砖-d: pranithk 笔记本电脑: / gfs 模数砖
砖-e: pranithk 笔记本电脑: / gfs/砖-e
砖-f: pranithk 笔记本电脑: / gfs 楼砖
砖-g: pranithk 笔记本电脑: / gfs/砖-g
砖-h: pranithk 笔记本电脑: / gfs/砖-h

在上面的示例︰
```
砖 |   副本集 |   砖子宗卷索引

----------------------------------------------------------------------------
-政府飞行服务队/砖一 |      0               |      0

-/ gfs/砖-b |      0               |      1

-/ gfs/砖-c |      1               |      2

-/ gfs 模数砖 |      1               |      3

-/ gfs/砖-e |      2               |      4

-/ gfs/砖-f |      2               |      5

-/ gfs/砖-g |      3               |      6

-/ gfs/砖-h |      3               |      7

```

每个文件在砖维护本身的更新日志和文件
目前在其副本集所看到的那块砖头上所有的砖块。

上面给出的示例货量，砖一将中的所有文件都有 2 个条目，
一个用于本身和其他的文件存在于其副本双 i.e.brick b:
trusted.afr.vol-客户端-0 = 0x000000000000000000000000--> (砖 a) 本身的更新日志
trusted.afr.vol-客户端-1 = 0x000000000000000000000000--> 砖 b 一样被砖一更新日志

同样，在砖 b 中的所有文件都都将︰
trusted.afr.vol-客户端-0 = 0x000000000000000000000000-更新日志-> 砖 — — 一个一样被砖 b
trusted.afr.vol-客户端-1 = 0x000000000000000000000000--> 更新日志本身 (砖-b)

同样可以扩展其他副本双。

口译 （大约挂起操作计数） 的更新日志值︰
每个扩展的属性已是 24 十六进制小数位数的值。
第一次 8 位数字代表数据的更新的日志。第二，8 位数字代表的更新日志

元数据。最后 8 位数字代表目录条目的更新的日志。 


构想代表一样，我们有︰
```
0 x 000003 d 7 00000001 00000000
|     |      |

|     |       河边的目录条目的更新日志

|      河边的元数据的更新日志

\ _ 更新日志的数据
```
         

目录元数据和条目更新历史是有效的。
对于常规文件数据和元数据更新历史是有效的。
为特殊的文件，如设备文件等元数据更改日志是有效的。
当一个文件裂发生它可能是任何数据裂或
裂的元数据或两者。当裂发生的更新日志

文件将这样的事情︰

* 示例: （让我们考虑这两个数据，裂上同一文件的元数据）。
[root@pranithk-laptop vol] # getfattr-d-m。-e 六角/gfs/砖-？ /a 

getfattr︰ 删除前导 '/' 从绝对路径名称
\#file: gfs/砖-a/a
trusted.afr.vol-客户端-0 = 0x000000000000000000000000
trusted.afr.vol-客户端-1 = 0x000003d70000000100000000
trusted.gfid=0x80acdbd886524f6fbefa21fc356fed57
\#file: gfs/砖-b/a
trusted.afr.vol-客户端-0 = 0x000003b00000000100000000
trusted.afr.vol-客户端-1 = 0x000000000000000000000000
trusted.gfid=0x80acdbd886524f6fbefa21fc356fed57

# # #Observations:

# # #According 到更新日志扩展属性对文件 /gfs/brick-a/a:
Trusted.afr.vol-客户端-0 的第 8 位数字是所有
零 (0x00000000......) 和第 8 位
trusted.afr.vol-客户端-1 并不是所有零 (0x000003d7...)。
所以在 /gfs/brick-a/a 上的更新日志隐含着一些数据操作成功
在本身但失败在 /gfs/brick-b/a 上。

Trusted.afr.vol-客户端-0 第二 8 位数字是
全部为零 (0 x......00000000......)，和第二 8 位数

trusted.afr.vol-客户端-1 并非全部为零 (0 x......00000001...)。

所以在 /gfs/brick-a/a 上的更新日志隐含着一些元数据操作成功
在本身但失败在 /gfs/brick-b/a 上。

# # #According 到更新日志扩展属性对文件 /gfs/brick-b/a:
Trusted.afr.vol-客户端-0 的第 8 位数字都是不
零 (0x000003b0...) 和第 8 位
trusted.afr.vol-客户端-1 的所有零 (0x00000000...)。
所以在 /gfs/brick-b/a 上的更新日志隐含着一些数据操作成功
在本身但失败在 /gfs/brick-a/a 上。

Trusted.afr.vol-客户端-0 第二 8 位数不是
全部为零 (0 x......00000001......)，和第二 8 位数

trusted.afr.vol-客户端-1 的所有零 (0x...00000000...)。

所以在 /gfs/brick-b/a 上的更新日志隐含着一些元数据操作成功
在本身但失败在 /gfs/brick-a/a 上。

由于这两个副本都有数据，元数据的更改，但不在其他
文件，它是在数据和元数据裂。

决定正确的副本︰
-----------------------------
用户可能需要检查统计，getfattr 输出的文件，以决定哪
要保留元数据和文件来决定要保留的数据的内容。
上例中，让我们继续说我们想要保留的数据
/gfs/brick-a/a 和 /gfs/brick-b/a 的元数据。

重置相关的更新历史解决裂︰
-------------------------------------------------------------
为解决数据拆分大脑︰
我们需要更改扩展文件属性，仿佛有些更新日志数据
操作上成功 /gfs/brick-a/a，但在 /gfs/brick-b/a 上失败。但

/gfs/brick-b/a 不应该说一些数据操作的任何更新日志
成功地在 /gfs/brick-b/a 但失败在 /gfs/brick-a/a 上。我们需要重置

对 trusted.afr.vol-客户端-0 /gfs/brick-b/a 的更新日志的数据部分。

为解决大脑-分裂-元数据︰
我们需要更改扩展文件属性，仿佛有些更新日志
元数据操作上成功 /gfs/brick-b/a，但在 /gfs/brick-a/a 上失败。
但 /gfs/brick-a/a 不应该说一些元数据的任何更新日志
操作上成功 /gfs/brick-a/a，但在 /gfs/brick-b/a 上失败。
我们需要重置元数据部分中的更新日志上
trusted.afr.vol-客户端-1 的 /gfs/brick-a/a

因此，预期的变化如下︰
在 /gfs/brick-b/a: 上
为 trusted.afr.vol-客户端-0
0x000003b00000000100000000 至 0x000000000000000100000000
（请注意，元数据部分仍然不全为零）
因此执行
`setfattr -n trusted.afr.vol-client-0 -v 0x000000000000000100000000 /gfs/brick-b/a`

在 /gfs/brick-a/a: 上
Trusted.afr.vol-客户端-1
0x000003d70000000100000000 至 0x000003d70000000000000000
（请注意，数据部分仍然不全为零）
因此执行
`setfattr -n trusted.afr.vol-client-1 -v 0x000003d70000000000000000 /gfs/brick-a/a`

因此上述操作完成后，更新历史看起来像这样︰
[root@pranithk-laptop vol] # getfattr-d-m。-e 六角/gfs/砖-？ /a 

getfattr︰ 删除前导 '/' 从绝对路径名称
\#file: gfs/砖-a/a
trusted.afr.vol-客户端-0 = 0x000000000000000000000000
trusted.afr.vol-客户端-1 = 0x000003d70000000000000000
trusted.gfid=0x80acdbd886524f6fbefa21fc356fed57

\#file: gfs/砖-b/a
trusted.afr.vol-客户端-0 = 0x000000000000000100000000
trusted.afr.vol-客户端-1 = 0x000000000000000000000000
trusted.gfid=0x80acdbd886524f6fbefa21fc356fed57


触发自我修复︰
---------------------
Perform `ls -l <file-path-on-gluster-mount>` to trigger healing.

固定的目录条目裂︰
----------------------------------
空燃比有能力保守合并不同目录中的条目
当裂上有目录。
如果一个砖目录上有 '有条目 '1'，'2' 并且有条目 3'，'4' 上
其他的砖然后空燃比，将合并所有目录中的条目
'1'，'2'，'3' '4' 个条目相同的目录中。
(注意︰ 这可能会导致删除的文件，重新出现在案例裂
发生删除的目录上的文件）
裂的决议需要人为干预时至少一个条目
哪个有相同的文件名字但不同 gfid 在该目录中。
示例︰
对砖一目录有 '1' （与 gfid g1)，'2' 和砖 b 上的条目
目录有条目 （用 gfid g2) 的 ' 1' 和 '3'。
这种目录拆分 — — 大脑需要人类的干预来解决。
用户需要删除上砖一文件 '1' 或 '1' 砖 b 上的文件
若要解决裂。此外，相应的 gfid 链接文件也

需要被删除。Gfid 链接文件存在.glusterfs 文件夹中

在砖的顶级目录。如果文件的 gfid 是

0x307a5c9efddd4e7c96e94fd4bcdcbd1b （trusted.gfid 扩展的属性得到
从 getfattr 命令早些时候），可以在找到 gfid 链接文件
> /gfs/brick-a/.glusterfs/30/7a/307a5c9efddd4e7c96e94fd4bcdcbd1b

# # #Word 的警告︰
在删除之前 gfid 链接，我们必须确保没有硬链接
到那块砖头上的现有文件。如果硬链接存在，他们必须作为删除

很好。
