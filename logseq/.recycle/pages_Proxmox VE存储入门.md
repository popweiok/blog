- collapsed:: true
	- [佛西博客 - Proxmox VE存储入门](https://omnivore.app/me/proxmox-ve-18eefe39472)
	  site:: [佛西博客](https://foxi.buduanwang.vip/linux/2044.html/)
	  author:: 佛西
	  date-saved:: [[Thu, 2024/04/18]]
	  date-published:: [[Sun, 2022/11/06]]
		- ### 内容
			- \#\# 简介
			  
			  大多数朋友用了很久的Proxmox VE却还是不理解Proxmox VE的存储。可想而知，对于新入门且没有虚拟化基础的朋友应该很难理解。由于之前的教程过于零散，叙述可能也有些问题，所以诞生了本文。
			  
			  本文旨在能够让读者理解Proxmox VE存储是什么样的，怎么使用，
			  
			  怎么维护。偏向原理，所以需要读者去多去理解它。
			  
			  为了码字方便，下面将Proxmox VE简称为PVE。
			  
			  \#\# PVE支持的存储类型
			  
			  下表是PVE支持的后端存储类型。
			  
			  | 名称             | PVE名称        | 级别 | 支持共享 | 支持快照 | 是否稳定 |
			  | -------------- | ------------ | -- | ---- | ---- | ---- |
			  | ZFS（本地）        | zfspool      | 块  | 否    | 是    | 是    |
			  | 目录             | dir          | 文件 | 否    | 否1   | 是    |
			  | BTRFS          | btrfs        | 文件 | 否    | 是    | 实验性  |
			  | Proxmox Backup | pbs          | 均是 | 是    | n/a  | 是    |
			  | NFS            | nfs          | 文件 | 是    | 否1   | 是    |
			  | CIFS           | cifs         | 文件 | 是    | 否1   | 否    |
			  | GlusterFS      | glusterfs    | 文件 | 是    | 否1   | 是    |
			  | CephFS         | cephfs       | 文件 | 是    | 是    | 是    |
			  | LVM            | lvm          | 块  | 否2   | 否    | 是    |
			  | LVM-thin       | lvmthin      | 块  | 否    | 是    | 是    |
			  | iSCSI/kernel   | iscsi        | 块  | 是    | 否    | 是    |
			  | iSCSI/libiscsi | iscsidir ect | 块  | 是    | 否    | 是    |
			  | Ceph/RBD       | rbd          | 块  | 是    | 是    | 是    |
			  | ZFS over iSCSi | zsf          | 块  | 是    | 是    | 是    |
			  
			  表注：
			  1. 在基于文件系统的存储上，可通过使用 qcow2 格式虚拟磁盘来实现快照。
			  2. 可以在 iSCSI 存储上配置 LVM，从而获得共享 存储。
			  
			  我们可以通过上表看到有很多的存储。从类别上，我们可以看到主要有2种：
			  
			  * 块  
			  虚拟机磁盘是**类似于磁盘分区**的形式，他不能进行文件操作。代表后端lvm/lvm-thin，ceph，zfs。新手可以理解他直接使用的是E盘，而不是E盘里的一个文件。
			  * 文件  
			  文件存储是虚拟机作为一种文件存在，如qcow2，raw，vmdk文件，可以像普通文件一样操作。
			  
			  现在我们一起来了解一下这些存储后端是什么。
			  
			  在这个环节中，我们需要带着一个疑问，这些存储他们为什么是块存储或者为什么文件存储。
			  
			  \#\# 目录
			  
			  目录是最基本的后端存储方式，PVE支持将系统上的某一个目录作为存储后端（前提是有读写权限）。
			  
			  你可以随意创建一个文件夹，
			  
			  如`mkdir /data`随后去PVE面板上添加这个目录即可
			  
			  [![](https://proxy-prod.omnivore-image-cache.app/600x222,sSO-bsJpAEaX3H7CSPy76dy6vUjtHnLtF1SYJdlvoWc4/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815104807.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815104807.png)
			  
			  此种方式，是将虚拟机磁盘作为文件方式存储，如qcow2，raw，vmdk格式。也得注意！仅磁盘格式为qcow2支持快照
			  
			  \#\# LVM和LVM-thin
			  
			  LVM有3个概念，一个是PV，一个是VG，一个是LV。
			  
			  * PV 物理卷
			  * VG 卷组
			  * LV 逻辑卷
			  
			  PV可以是一整个磁盘，也可以是一个分区。VG是一个PV或者多个PV组成的卷组。LV是一个VG下面的逻辑卷。
			  
			  [![](https://proxy-prod.omnivore-image-cache.app/720x375,sNG-Nrb9uokJJMcDZPQ_sdQ2OjOhPiAjgOpCFpAl0wYU/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/v2-1a90c2975d253a669a29b9b0b4e35938_720w.jpg)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/v2-1a90c2975d253a669a29b9b0b4e35938%5F720w.jpg)
			  
			  更多请参考
			  
			  [LVM——让Linux磁盘空间的弹性管理 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/67166867)
			  
			  [LVM\_百度百科 (baidu.com)](https://baike.baidu.com/item/LVM/6571177)
			  
			  [系统运维|Linux LVM简明教程](https://linux.cn/article-3218-1.html)
			  
			  [LVM管理 - 苦逼运维 - 博客园 (cnblogs.com)](https://www.cnblogs.com/diantong/p/10554831.html)
			  
			  [LVM (简体中文) - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/LVM%5F%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29)
			  
			  \#\#\# LVM
			  
			  我们先说LVM。我们可以前往网页上，选中节点，展开磁盘选项，选择LVM，点击创建VG，选择磁盘和名称即可。
			  
			  [![](https://proxy-prod.omnivore-image-cache.app/982x694,s0IjM3tLh695IPV5oPWJrlifYhaIACS4tWDMPDo8s8_o/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815105906.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815105906.png)
			  
			  我这里创建一个名为lvm的lvm存储后端。
			  
			  [![](https://proxy-prod.omnivore-image-cache.app/892x344,su-gCUHpgtWX0t97Hfps6jbusC63MJBz3C2bD03-Vzmg/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815110120.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815110120.png)
			  
			  创建一个虚拟机，他的磁盘为`vm-100-disk-0` ，位置在`/dev/lvm/vm-100-disk-0`
			  
			  如上面的例子，我们通过命令查看lv
			  
			  ```angelscript
			  root@pve:~\# lvs
			    LV            VG  Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
			    vm-100-disk-0 lvm -wi-a----- 32.00g
			  root@pve:~\# lvdisplay
			    --- Logical volume ---
			    LV Path                /dev/lvm/vm-100-disk-0
			    LV Name                vm-100-disk-0
			    VG Name                lvm
			    LV UUID                d3iw1e-2I8R-VfG7-wWQZ-UNNV-sTEd-EKjIkv
			    LV Write Access        read/write
			    LV Creation host, time pve, 2022-08-15 03:02:07 +0000
			    LV Status              available
			    \# open                 0
			    LV Size                32.00 GiB
			    Current LE             8192
			    Segments               1
			    Allocation             inherit
			    Read ahead sectors     auto
			    - currently set to     256
			    Block device           253:0
			  
			  ```
			  
			  按照正常的逻辑，我们要使用lvm，创建好lv之后，就需要对lv进行格式化，但是PVE并不这样。
			  
			  PVE将LVM下的LV作为虚拟磁盘 
			  
			  \#\#\# LVM-thin
			  
			  这是使用iso安装PVE后，默认的虚拟磁盘后端就是LVM-thin。
			  
			  LVM-thin是一个非常精巧的文件系统，最直接的表现就是LVM-thin它可以超配，例如你的物理磁盘只有1T，使用LVM-thin之后，你可以分配总大小超过1T的磁盘。它是用多少，占用多少空间，且支持快照。
			  
			  下面是我将一个100G的磁盘创建成了lvm-thin，并给100虚拟机创建了一个2T的磁盘。
			  
			  ```routeros
			  root@pve:~\# lvs
			    LV            VG       Attr       LSize  Pool     Origin Data%  Meta%  Move Log Cpy%Sync Convert
			    thinpool      thinpool twi-aotz-- 97.87g                 0.00   1.61                            
			    vm-100-disk-0 thinpool Vwi-a-tz--  2.00t thinpool        0.00       
			  
			  ```
			  
			  确实很强。
			  
			  我们可以看到这个磁盘也是属于LV，那么对于LVM-thin后端来说，PVE也是直接将lv作为磁盘后端。
			  
			  所以，当我们使用LVM-thin后端时，要记住PVE是直接创建并LV，前往不要自己去创建LV，挂载LV，再将挂载的目录存储虚拟机。
			  
			  \#\# ZFS本地文件系统
			  
			  ZFS是在笔者眼里，是最强大的本地文件系统。具体怎么个强大法，我们不去深究，这是课外知识，有兴趣大家可以阅读下面文章
			  
			  [ZFS──瑞士军刀般的文件系统 - EAimTY 的博客](https://www.eaimty.com/2020/02/zfs-file-system.html)
			  
			  [提升ZFS性能的10个简便方法\_weixin\_34092455的博客-CSDN博客](https://blog.csdn.net/weixin%5F34092455/article/details/92211389)
			  
			  [Oracle Solaris ZFS 文件系统（介绍） - Oracle Solaris 管理：ZFS 文件系统](https://docs.oracle.com/cd/E26926%5F01/html/E25826/zfsover-1.html\#scrolltoc)
			  
			  [提高truenas上zfs性能的几个小tips - 广陌 (utopiafar.com)](https://www.utopiafar.com/2022/03/26/how%5Fto%5Fimprove%5Fzfs%5Fperformance%5Fon%5Ffreenas/)
			  
			  https://www.youtube.com/channel/UC0IK6Y4Go2KtRueHDiQcxow
			  
			  本节重点来了。PS.上面大多都没着重说明。
			  
			  ZFS储存数据有2个迷糊点：
			  
			  * zvol（卷）：类似于分区
			  * dataset（数据集）：文件夹
			  
			  \#\#\# dataset
			  
			  `dataset`可以认为是一个特殊的文件夹，这个文件夹可以自定义zfs的一些特性，如去重策略，压缩策略，快照等等。注意，这里说的是文件夹，那么就可以存放文件，进行文件操作。
			  
			  例如，我在zff池中，创建一个名为data的数据集，
			  
			  ```haskell
			  zfs create zff/data
			  
			  ```
			  
			  它在zfs的结构如下
			  
			  ```angelscript
			  root@pve:~\# zfs list
			  NAME                USED  AVAIL     REFER  MOUNTPOINT
			  zff                21.7G  74.7G       96K  /zff
			  zff/data             96K  74.7G       96K  /zff/data
			  
			  ```
			  
			  挂载点在`/zff/data`
			  
			  我们在这个目录中创建3个文件，可以通过tree命令看到目录树。
			  
			  ```elixir
			  root@pve:/zff/data\# touch this is data
			  root@pve:/zff/data\# cd ..
			  root@pve:/zff\# tree
			  .
			  └── data
			      ├── data
			      ├── is
			      └── this
			  
			  1 directory, 3 files
			  
			  ```
			  
			  随后我们将data这个数据集给挂载到/data位置
			  
			  ```elixir
			  root@pve:/zff\# zfs set mountpoint=/data zff/data
			  root@pve:/zff\# tree
			  .
			  
			  0 directories, 0 files
			  root@pve:/data\# ls
			  data  is  this
			  
			  ```
			  
			  可以看到zff目录下已经没有data了，在/data目录中有着相应的文件。
			  
			  所以我们可以认为zfs的数据集（dataset）是一种特殊的文件夹，这有点类似于btrfs的子卷。
			  
			  他不同于一般的文件夹，他可以单独快照，不能被`rm` 删除，不同的数据集，可以配置不同的策略。
			  
			  \#\#\# zvol
			  
			  `zvol` 也叫zfs 卷。当创建一个`zvol`卷时，他会被标识为一个独立`块设备` ，位于`/dev/zvol/{dsk,rdsk}/pool` 。
			  
			  在PVE上使用默认方式创建ZFS存储池后，虚拟机磁盘则会是一个zvol，如我创建了一个vmid为100的虚拟机，他的磁盘在zfs的位置是`zff/vm-100-disk-0`
			  
			  ```angelscript
			  root@pve:~\# zfs list
			  NAME                USED  AVAIL     REFER  MOUNTPOINT
			  zff                21.7G  74.7G       96K  /zff
			  zff/vm-100-disk-0  21.7G  96.4G       56K  -
			  root@pve:~\#
			  
			  ```
			  
			  在系统上的位置是`/dev/zvol/zff/vm-100-disk-0`
			  
			  `zvol`通常用于iscsi，swap以及虚拟机存储。
			  
			  `zvol`可以被创建在任意的一个数据集中，如我创建一个zvol
			  
			  ```elixir
			  root@pve:/zff\# zfs create -V 30G zff/data/zvoltest
			  
			  ```
			  
			  我们再去查看zfs的结构
			  
			  ```angelscript
			  root@pve:/zff\# zfs list
			  NAME                USED  AVAIL     REFER  MOUNTPOINT
			  zff                52.6G  43.8G       96K  /zff
			  zff/data           30.9G  43.8G       96K  /data
			  zff/data/zvoltest  30.9G  74.7G       56K  -
			  zff/vm-100-disk-0  21.7G  65.5G       56K  -
			  root@pve:/zff\# ls /data
			  root@pve:/zff\# ls /dev/zff/data/zvoltest
			  /dev/zff/data/zvoltest
			  
			  
			  ```
			  
			  可以发现，创建的`zvoltest` 并不存在于`/data` 目录下，而是在`/dev/zff/data/zvoltest`，linux中，`/dev` 目录下，均是设备，所以大家一定要记住，`zvol`对系统来说是一个设备。
			  
			  同时这个`zvol`也和`dataset`一样，可以设置单独的zfs策略，快照，块大小等等，尽管它可能存在某个`dataset`下，但就是可以单独搞，这有点像套娃。不错，zfs的`dataset`就是可以套娃，可以在一个`dataset`下创建新的`dataset`，然后在这个新的`dataset`下，继续创建`dataset`。
			  
			  那么`zvol`可以套娃吗？
			  
			  \#\#\# PVE与zfs
			  
			  经过上面的叙述，是否明白了，zfs的zvol和dataset的主要区别？
			  
			  `zvol`是块设备。`dataset`是特殊的文件夹。`zvol`可以创建在`dataset`下。
			  
			  PVE对zfs的支持仅只有`zvol`这一种方式。例如，我们创建了一个zfs之后，在面板上添加存储时，ZFS池有2个选项，这一个是zff大池，一个是zff下名为data的数据集。
			  
			  [![](https://proxy-prod.omnivore-image-cache.app/678x276,smBXN8zyh7TZvKhgf2SLHo_Ub-7Vqe_J-wta7LNJnxjc/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815102851.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815102851.png)
			  
			  这意味着PVE支持将磁盘放置到池顶级`dataset`中，也可以放到子`dataset`中。
			  
			  在内容选择上，只支持磁盘映像和容器。这直接说明了，PVE的ZFS并不是以文件方式存储磁盘映像的，而是通过块存储——`zvol`。
			  
			  [![](https://proxy-prod.omnivore-image-cache.app/632x238,shP5WlL5W3iteRxSNV62PFI_H7f3YFUcsu15OI3yLDFo/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815103049.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815103049.png)
			  
			  如果你想要PVE上的ZFS池使用文件方式存储虚拟机镜像怎么办？
			  
			  前面说了，zfs可以将`dataset`挂载到一个目录，那么在PVE的面板上，以目录方式添加存储，选择`dataset`路径即可。
			  
			  此种方式，这将失去PVE对zfs的自动优化，你必须需要手动进行设置zfs的策略，且仅磁盘格式为qcow2才支持快照
			  
			   版权声明：  
			  作者：佛西  
			  链接：https://foxi.buduanwang.vip/linux/2044.html/  
			  文章版权归作者所有，未经允许请勿转载  
			  如需获得支持，请点击网页右上角 
			  
			   THE END
			  
			  文章目录
			  
			  简介
			  
			  PVE支持的存储类型
			  
			  目录
			  
			  LVM和LVM-thin
			  
			  LVM
			  
			  LVM-thin
			  
			  ZFS本地文件系统
			  
			  dataset
			  
			  zvol
			  
			  PVE与zfs
			  
			  关闭
			  
			   目 录
				- currently set to     256
				  Block device           253:0
				  
				  ```
				  
				  按照正常的逻辑，我们要使用lvm，创建好lv之后，就需要对lv进行格式化，但是PVE并不这样。
				  
				  PVE将LVM下的LV作为虚拟磁盘 
				  
				  \#\#\# LVM-thin
				  
				  这是使用iso安装PVE后，默认的虚拟磁盘后端就是LVM-thin。
				  
				  LVM-thin是一个非常精巧的文件系统，最直接的表现就是LVM-thin它可以超配，例如你的物理磁盘只有1T，使用LVM-thin之后，你可以分配总大小超过1T的磁盘。它是用多少，占用多少空间，且支持快照。
				  
				  下面是我将一个100G的磁盘创建成了lvm-thin，并给100虚拟机创建了一个2T的磁盘。
				  
				  ```routeros
				  root@pve:~\# lvs
				  LV            VG       Attr       LSize  Pool     Origin Data%  Meta%  Move Log Cpy%Sync Convert
				  thinpool      thinpool twi-aotz-- 97.87g                 0.00   1.61                            
				  vm-100-disk-0 thinpool Vwi-a-tz--  2.00t thinpool        0.00       
				  
				  ```
				  
				  确实很强。
				  
				  我们可以看到这个磁盘也是属于LV，那么对于LVM-thin后端来说，PVE也是直接将lv作为磁盘后端。
				  
				  所以，当我们使用LVM-thin后端时，要记住PVE是直接创建并LV，前往不要自己去创建LV，挂载LV，再将挂载的目录存储虚拟机。
				  
				  \#\# ZFS本地文件系统
				  
				  ZFS是在笔者眼里，是最强大的本地文件系统。具体怎么个强大法，我们不去深究，这是课外知识，有兴趣大家可以阅读下面文章
				  
				  [ZFS──瑞士军刀般的文件系统 - EAimTY 的博客](https://www.eaimty.com/2020/02/zfs-file-system.html)
				  
				  [提升ZFS性能的10个简便方法\_weixin\_34092455的博客-CSDN博客](https://blog.csdn.net/weixin%5F34092455/article/details/92211389)
				  
				  [Oracle Solaris ZFS 文件系统（介绍） - Oracle Solaris 管理：ZFS 文件系统](https://docs.oracle.com/cd/E26926%5F01/html/E25826/zfsover-1.html\#scrolltoc)
				  
				  [提高truenas上zfs性能的几个小tips - 广陌 (utopiafar.com)](https://www.utopiafar.com/2022/03/26/how%5Fto%5Fimprove%5Fzfs%5Fperformance%5Fon%5Ffreenas/)
				  
				  https://www.youtube.com/channel/UC0IK6Y4Go2KtRueHDiQcxow
				  
				  本节重点来了。PS.上面大多都没着重说明。
				  
				  ZFS储存数据有2个迷糊点：
				  
				  * zvol（卷）：类似于分区
				  * dataset（数据集）：文件夹
				  
				  \#\#\# dataset
				  
				  `dataset`可以认为是一个特殊的文件夹，这个文件夹可以自定义zfs的一些特性，如去重策略，压缩策略，快照等等。注意，这里说的是文件夹，那么就可以存放文件，进行文件操作。
				  
				  例如，我在zff池中，创建一个名为data的数据集，
				  
				  ```haskell
				  zfs create zff/data
				  
				  ```
				  
				  它在zfs的结构如下
				  
				  ```angelscript
				  root@pve:~\# zfs list
				  NAME                USED  AVAIL     REFER  MOUNTPOINT
				  zff                21.7G  74.7G       96K  /zff
				  zff/data             96K  74.7G       96K  /zff/data
				  
				  ```
				  
				  挂载点在`/zff/data`
				  
				  我们在这个目录中创建3个文件，可以通过tree命令看到目录树。
				  
				  ```elixir
				  root@pve:/zff/data\# touch this is data
				  root@pve:/zff/data\# cd ..
				  root@pve:/zff\# tree
				  .
				  └── data
				  ├── data
				  ├── is
				  └── this
				  
				  1 directory, 3 files
				  
				  ```
				  
				  随后我们将data这个数据集给挂载到/data位置
				  
				  ```elixir
				  root@pve:/zff\# zfs set mountpoint=/data zff/data
				  root@pve:/zff\# tree
				  .
				  
				  0 directories, 0 files
				  root@pve:/data\# ls
				  data  is  this
				  
				  ```
				  
				  可以看到zff目录下已经没有data了，在/data目录中有着相应的文件。
				  
				  所以我们可以认为zfs的数据集（dataset）是一种特殊的文件夹，这有点类似于btrfs的子卷。
				  
				  他不同于一般的文件夹，他可以单独快照，不能被`rm` 删除，不同的数据集，可以配置不同的策略。
				  
				  \#\#\# zvol
				  
				  `zvol` 也叫zfs 卷。当创建一个`zvol`卷时，他会被标识为一个独立`块设备` ，位于`/dev/zvol/{dsk,rdsk}/pool` 。
				  
				  在PVE上使用默认方式创建ZFS存储池后，虚拟机磁盘则会是一个zvol，如我创建了一个vmid为100的虚拟机，他的磁盘在zfs的位置是`zff/vm-100-disk-0`
				  
				  ```angelscript
				  root@pve:~\# zfs list
				  NAME                USED  AVAIL     REFER  MOUNTPOINT
				  zff                21.7G  74.7G       96K  /zff
				  zff/vm-100-disk-0  21.7G  96.4G       56K  -
				  root@pve:~\#
				  
				  ```
				  
				  在系统上的位置是`/dev/zvol/zff/vm-100-disk-0`
				  
				  `zvol`通常用于iscsi，swap以及虚拟机存储。
				  
				  `zvol`可以被创建在任意的一个数据集中，如我创建一个zvol
				  
				  ```elixir
				  root@pve:/zff\# zfs create -V 30G zff/data/zvoltest
				  
				  ```
				  
				  我们再去查看zfs的结构
				  
				  ```angelscript
				  root@pve:/zff\# zfs list
				  NAME                USED  AVAIL     REFER  MOUNTPOINT
				  zff                52.6G  43.8G       96K  /zff
				  zff/data           30.9G  43.8G       96K  /data
				  zff/data/zvoltest  30.9G  74.7G       56K  -
				  zff/vm-100-disk-0  21.7G  65.5G       56K  -
				  root@pve:/zff\# ls /data
				  root@pve:/zff\# ls /dev/zff/data/zvoltest
				  /dev/zff/data/zvoltest
				  
				  
				  ```
				  
				  可以发现，创建的`zvoltest` 并不存在于`/data` 目录下，而是在`/dev/zff/data/zvoltest`，linux中，`/dev` 目录下，均是设备，所以大家一定要记住，`zvol`对系统来说是一个设备。
				  
				  同时这个`zvol`也和`dataset`一样，可以设置单独的zfs策略，快照，块大小等等，尽管它可能存在某个`dataset`下，但就是可以单独搞，这有点像套娃。不错，zfs的`dataset`就是可以套娃，可以在一个`dataset`下创建新的`dataset`，然后在这个新的`dataset`下，继续创建`dataset`。
				  
				  那么`zvol`可以套娃吗？
				  
				  \#\#\# PVE与zfs
				  
				  经过上面的叙述，是否明白了，zfs的zvol和dataset的主要区别？
				  
				  `zvol`是块设备。`dataset`是特殊的文件夹。`zvol`可以创建在`dataset`下。
				  
				  PVE对zfs的支持仅只有`zvol`这一种方式。例如，我们创建了一个zfs之后，在面板上添加存储时，ZFS池有2个选项，这一个是zff大池，一个是zff下名为data的数据集。
				  
				  [![](https://proxy-prod.omnivore-image-cache.app/678x276,smBXN8zyh7TZvKhgf2SLHo_Ub-7Vqe_J-wta7LNJnxjc/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815102851.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815102851.png)
				  
				  这意味着PVE支持将磁盘放置到池顶级`dataset`中，也可以放到子`dataset`中。
				  
				  在内容选择上，只支持磁盘映像和容器。这直接说明了，PVE的ZFS并不是以文件方式存储磁盘映像的，而是通过块存储——`zvol`。
				  
				  [![](https://proxy-prod.omnivore-image-cache.app/632x238,shP5WlL5W3iteRxSNV62PFI_H7f3YFUcsu15OI3yLDFo/https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220815103049.png)](https://foxi.buduanwang.vip/wp-content/uploads/2022/08/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE%5F20220815103049.png)
				  
				  如果你想要PVE上的ZFS池使用文件方式存储虚拟机镜像怎么办？
				  
				  前面说了，zfs可以将`dataset`挂载到一个目录，那么在PVE的面板上，以目录方式添加存储，选择`dataset`路径即可。
				  
				  此种方式，这将失去PVE对zfs的自动优化，你必须需要手动进行设置zfs的策略，且仅磁盘格式为qcow2才支持快照
				  
				  版权声明：  
				  作者：佛西  
				  链接：https://foxi.buduanwang.vip/linux/2044.html/  
				  文章版权归作者所有，未经允许请勿转载  
				  如需获得支持，请点击网页右上角 
				  
				  THE END
				  
				  文章目录
				  
				  简介
				  
				  PVE支持的存储类型
				  
				  目录
				  
				  LVM和LVM-thin
				  
				  LVM
				  
				  LVM-thin
				  
				  ZFS本地文件系统
				  
				  dataset
				  
				  zvol
				  
				  PVE与zfs
				  
				  关闭
				  
				  目 录