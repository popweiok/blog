- [佛西博客 - Proxmox VE 限制zfs的使用量](https://foxi.buduanwang.vip/virtualization/pve/kb/3046.html/)
  tags:: [[SendToLogseq]]
	- 默认情况下，ZFS会使用宿主机50%的内存做为ARC缓存。
	- 为 ARC 分配足够的内存对于 IO 性能至关重要，因此请谨慎减少内存。根据一般情况，建议，1TB空间使用1G内存，同时预留2G基本内存。如果池有8TB空间，那应该给ARC分配8G+2G共10G的内存。
	- 您可以通过直接写入zfs\_arc\_max模块参数来更改当前引导的 ARC 使用限制（重新启动会失效）：
	- ```
	  echo "$[10 * 1024*1024*1024]" >/sys/module/zfs/parameters/zfs_arc_max
	  ```
	- 要永久生效，请将以下行添加到 `/etc/modprobe.d/zfs.conf`中。 如要限制为8G内存，则添加如下：
	- ```
	  options zfs zfs_arc_max=8589934592
	  ```
	- **注意**:如果所需的zfs\_arc\_max值小于或等于 zfs\_arc\_min（默认为系统内存的 1/32），则将忽略zfs\_arc\_max，除非您还将zfs\_arc\_min设置为最多 zfs\_arc\_max - 1。
	- 如下，在256G内存的系统上，限制ARC为8GB，需要设置zfs\_arc\_min -1。只设置zfs\_arc\_max是不行的
	- ```
	  echo "$[8 * 1024*1024*1024 - 1]" >/sys/module/zfs/parameters/zfs_arc_min
	  echo "$[8 * 1024*1024*1024]" >/sys/module/zfs/parameters/zfs_arc_max
	  ```
	- 如果根文件系统是 ZFS，则每次更改此值时都必须更新初始化接口： `update-initramfs -u`，同时重新启动才能激活这些更改。
-