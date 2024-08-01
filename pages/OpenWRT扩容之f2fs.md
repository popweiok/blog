- [OpenWrt扩容(Overlay扩容 启动前后两种)_openwrt 扩容overlay-CSDN博客](https://blog.csdn.net/Miss_Mario/article/details/136598925)
  tags:: [[SendToLogseq]]
	- OpenWrt扩容有多种方法。
	- 1、直接修改镜像文件分区大小
	- linux中操作
	- 解压镜像文件 并且扩容
	- ```bash
	  gzip -kd openwrt.img.gzdd if=/dev/zero bs=1M count=500 >>openwrt.imgparted openwrt.imgprintresizepart 2 100%printquit
	  ```
	- 2、直接修改硬盘分区大小
	- Gparted.iso 官网下载
	- 启动之前！！！！！！！
	- ```bash
	  parted /dev/sdaprintresizepart 2 100%printquit
	  ```
	- 3、启动后扩容
	- 先获取偏移量
	- openwrt上获取：
	- losetup 软件没有可以opkg安装。
	- ```bash
	  losetup
	  ```
	- 获取offset 记录。
	- 然后进入Gparted
	- 其中loop100 自定义 只要不重复就可以了
	- ```bash
	  parted /dev/sdaprintresizepart 2 100%printquitlosetup -o 上面获取的偏移量(offset) /dev/loop100 /dev/sda2fsck.f2fs /dev/loop100resize.f2fs /dev/loop100
	  ```
	- 如果最后一步报错 执行下面命令 再执行最后一步。
	- ```bash
	  mkdir /mnt/loop100mount /dev/loop100 /mnt/loop100umount /dev/loop100
	  ```
	- Over。尽情享用吧！！！
-