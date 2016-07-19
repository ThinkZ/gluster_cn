FHS 2.3 仍不完全清楚由服务器共享的数据应该驻留。它并说明"_ / srv 包含特定于站点的数据由这系列服务"，而是特定于站点的 GlusterFS 数据吗？


The consensus seems to lean toward using `/data`. A good hierarchical method for placing bricks is:

```
/data/glusterfs/<volume>/<brick>/brick
```

In this example, `<brick>` is the filesystem that is mounted.

# # # 示例︰ 一个砖每台服务器

物理磁盘 * / dev/康体发展局 * 会为你要创建一个卷名，用于砖存储 * myvol1 *。你已经分区和格式化 * / dev/sdb1 * xfs 在每 4 服务器上。


在所有 4 个服务器︰

```bash
mkdir-p /data/glusterfs/myvol1/brick1
装载 /dev/sdb1 /data/glusterfs/myvol1/brick1
```

We're going to define the actual brick in the `brick` directory on that filesystem. This helps by causing the brick to fail to start if the XFS filesystem isn't mounted.

只是一个在服务器上︰

```bash
gluster 卷创建 myvol1 副本 2 server{1..4}:/data/glusterfs/myvol1/brick1/brick
```

This will create the volume *myvol1* which uses the directory `/data/glusterfs/myvol1/brick1/brick` on all 4 servers.

# # # 示例︰ 两个砖每台服务器

两个物理磁盘 * / dev/康体发展局 * 和 * / dev/sdc * 将要为你要创建一个卷名，用于砖存储 * myvol2 *。你已经分区和格式化 * / dev/sdb1 * 和 * / dev/sdc1 * xfs 在每 4 服务器上。


在所有 4 个服务器︰

```bash
mkdir-p /data/glusterfs/myvol2/brick {1，2}
装载 /dev/sdb1 /data/glusterfs/myvol2/brick1
装载 /dev/sdc1 /data/glusterfs/myvol2/brick2
```

Again we're going to define the actual brick in the `brick` directory on these filesystems.

只是一个在服务器上︰

```bash
gluster 卷创建 myvol2 副本 2 \
server{1..4}:/data/glusterfs/myvol2/brick1/brick \
server{1..4}:/data/glusterfs/myvol2/brick2/brick
```

**Note:** It might be tempting to try `gluster volume create myvol2 replica 2 server{1..4}:/data/glusterfs/myvol2/brick{1,2}/brick` but Bash would expand the last `{}` first, so you would end up replicating between the two bricks on each servers, instead of across servers.
