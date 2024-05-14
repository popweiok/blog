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
	- 其实 apt 与 apt-get 命令都是基于 dpkg
	- ```
	  dpkg-query -l
	  ```
		- ![image.png](../assets/image_1715652642020_0.png)
		- ```
		  #同样也可以通过 dpkg 命令的历史日志，会显示所有的软件安装包，其中包括最近安装的过程中所依赖的软件包。
		  grep " install " /var/log/dpkg.log
		  ```
			- ![image.png](../assets/image_1715652815223_0.png)