## 思路
	- 连接TTL，GND接GND、路由器[[$green]]==RX==接TTL的[[$green]]==TX==、路由器[[$red]]==TX==接TTL的[[$red]]==RX==，也就是RX和TX反过来接
	-
	- 路由器重新上电可看到正常显示启动数据，在启动倒数几秒内按对应的快捷键中断启动
	-
	- 通过TFTP上传固件写入内存
	-
	- 从内存写入QSPI Flash
	-
-