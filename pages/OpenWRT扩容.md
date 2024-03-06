### OpenWRT各种固件版本
	- combined-ext4.img.gz
		-
	- combined-squashfs.img.gz
	- generic-rootfs.tar.gz
	- rootfs-ext4.img.gz
	- rootfs-squashfs.img.gz
	- vmlinuz
- ### EXT4固件
  collapsed:: true
	- ![image.png](../assets/image_1690855820626_0.png)
	- #### 挂载分区方式
		- ***未格式化分区-->新建主分区-->复制根目录内容到新分区-->把原系统根目录默认挂载点变成新分区***
		- ![image.png](../assets/image_1690856012649_0.png)
			- 需要用到两个插件 ***fdisk**** 磁盘分区插件   ***block-mount*** 挂载点插件
				- 查看现有分区，fdisk -l
					- ![image.png](../assets/image_1690856636204_0.png)
				- 注意最后一个分区的结束，新建分区必须大于最后一个分区数值
				  collapsed:: true
					- ![image.png](../assets/image_1690856680893_0.png)
				- 新建主分区，数值大于前结束分区
				  collapsed:: true
					- ![image.png](../assets/image_1690856714572_0.png)
				- 扩大容量可以直接输入数值，并存盘
				  collapsed:: true
					- ![image.png](../assets/image_1690856756048_0.png)
				- 查看新分区容量情况，此时并没有生效
				  collapsed:: true
					- ![image.png](../assets/image_1690856800547_0.png)
				- 格式化新分区，如果没有MKFS.ext4命令，请安装e2fsprogs插件
				  collapsed:: true
					- ![image.png](../assets/image_1690856840529_0.png)
				- 在UI界面挂载为根分区，并复制命令
				  collapsed:: true
					- ![image.png](../assets/image_1690856877516_0.png)
				- 粘贴命令，实现复制旧根分区文件到新分区
				  collapsed:: true
					- ![image.png](../assets/image_1690856920881_0.png)
	- #### 直接扩容原来的分区
		- ![image.png](../assets/image_1690857214619_0.png)
			- 需要用到三个插件 ***fdisk*** ***resize2fs*** ***losetup***
				- 查看硬盘分区情况
					- ![image.png](../assets/image_1690857381078_0.png)
				- 注意第二分区开始数值
				  collapsed:: true
					- ![image.png](../assets/image_1690857409297_0.png)
				- 删除第二分区，新建主分区，开始数值必须为原分区开始数值
				  collapsed:: true
					- ![image.png](../assets/image_1690857452372_0.png)
				- 注意，不要移除原分区签名
				  collapsed:: true
					- ![image.png](../assets/image_1690857487500_0.png)
				- 用losetup挂载空循环设备到第二分区，并用resize2fs完成分区扩容
				  collapsed:: true
					- ![image.png](../assets/image_1690857524326_0.png)
					-
- ### SQUASHFS固件
  collapsed:: true
	- ![image.png](../assets/image_1690855861283_0.png)
	- 需要用到两个插件  ***fdisk***  ***resize2fs***
		- 查看分区情况，注意循环设备盘符
			- ![image.png](../assets/image_1690857710417_0.png)
		- 删除原第二分区，并新建分区（注意第二分区的开始值）
			- ![image.png](../assets/image_1690857768281_0.png)
		- 新建主分区，输入原第二分区开始值，并存盘
			- ![image.png](../assets/image_1690857804085_0.png)
		- 用resize2fs扩容循环设备
			- ![image.png](../assets/image_1690857837104_0.png)
- ### UEFI格式的硬盘
  collapsed:: true
	- BLKID插件
		- 查看第二分区的UUID号
		  collapsed:: true
			- ![image.png](../assets/image_1690858427584_0.png){:height 76, :width 688}
		- 修改grub.cfg
		  collapsed:: true
			- ![image.png](../assets/image_1690858474202_0.png)