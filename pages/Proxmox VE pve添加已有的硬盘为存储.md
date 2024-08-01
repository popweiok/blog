- [佛西博客 - Proxmox VE pve添加已有的硬盘为存储](https://foxi.buduanwang.vip/virtualization/pve/2736.html/)
  tags:: [[SendToLogseq]]
	- 使用Proxmox VE web面板只能添加新盘，无法添加已经格式化的硬盘，除非将它初始化一次。
	- 若是磁盘里面有数据，则很糟糕。
	- Promxox VE官方内核，默认支持常见的文件系统，如brtfs-ext-xfs-ntfs-EXFAT-FAT。如果你是上面提到的文件系统，那么就可以通过手动挂载的方式，实现为pve添加存储。
	- 我们这里以NTFS 磁盘为例。我这里有1个磁盘，有2个分区，插到了pve上。
	- **![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678766790548.png){:width 1061 :height 177}**
	- 我们要使用ntfs，需要安装一个软件包`ntfs-3g`
	- ```
	  apt update && apt install ntfs-3g
	  ```
	- 创建一个挂载点
	- ```
	  mkdir /mnt/pve/hdd
	  ```
	- 将磁盘挂载过去
	- ```
	  mount /dev/sdb2 /mnt/pve/hdd
	  ```
	- 如果有下面提示，
	- ```
	  The disk contains an unclean file system (0, 0).
	  Metadata kept in Windows cache, refused to mount.
	  Falling back to read-only mount because the NTFS partition is in an
	  unsafe state. Please resume and shutdown Windows fully (no hibernation
	  or fast restarting.)
	  Could not mount read-write, trying read-only
	  ```
	- 需要修复一下
	- ```
	  ntfsfix /dev/sdb2
	  ```
	- ![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678767073755.png){:width 758, :height 302}
	- 之后再重新挂载
	- `umount /dev/sdb2`
	- `mount /dev/sdb2 /mnt/pve/hdd`
	- 现在可以看到其中的文件了
	- ![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678767174732.png){:width 1061 :height 278}
	- 随后进pve 网页添加目录存储.
	- ID随便取，目录填写硬盘的挂载路径，内容全部勾选
	- ![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678767244075.png){:width 1061 :height 530}
	- 接下来就可以在网页上使用了。
	- ![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678767361407.png){:width 1061 :height 296}
	- 如果打算永久挂载，需要配置开机挂载。
	- 查看硬盘的PARTUUID，使用blkid查看，比如我的ntfs分区是/dev/sdb2
	- ![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678768054595.png){:width 1061 :height 150}将这个PARTUUID写进fstab，注意将我们常规的defaults选项换成`nofail,x-systemd.device-timeout=15s` 这样防止找不到硬盘，而卡引导
	- ![image](https://foxi.buduanwang.vip/wp-content/uploads/2023/03/1678846432630.png){:width 1061 :height 141}保存就可以了。
-