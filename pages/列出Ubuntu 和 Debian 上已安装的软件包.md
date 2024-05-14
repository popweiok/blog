- 所有基于Debian分支理论上都支持，Debian使用apt及dpkg进行软件包管理
- APT
	- ```
	  #列出通过apt命令安装的软件包
	  apt list --installed
	  ```
		- ![image.png](../assets/image_1715652171847_0.png)
		- ```
		  #可以加 grep 过滤软件名
		  apt list --installed | grep program_name
		  ```
	- ```
	  #查看 apt 历史命令。但不会显示被依赖安装的软件包
	  grep " install " /var/log/apt/history.log
	  ```
		- ![image.png](../assets/image_1715652544448_0.png)
- DPKG
	-