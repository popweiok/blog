- https://juejin.cn/post/7091136261800329252
	-
- ## 一、背景介绍
	- 通常 CentOS、Ubuntu 早期版本Linux系统开机自动加载程序或者脚本，我们都放在**/etc/rc.local** 执行。 拿 Ubuntu系统来说，Ubuntu 16.04 版本开始去除了 rc.local 文件，自启动服务方面基本由 systemd 全面接管了。 想要添加一些开机运行的操作只能创建 systemd 服务或者添加 desktop 文件，有点麻烦。
	- 那么在 Ubuntu 20.04 LTS 版本系统中如何实现开机自启动呢？我们通过 **systemd** 服务来创建它。
- ## 二、实现原理
	- 其实，Ubuntu 20.04 系统已经预制了一个叫做 rc-local.service的服务，只不过默认没有执行。我们可以在以下目录中找到该服务文件：
	- ```clojure
	  root@360che:~# ll /usr/lib/systemd/system/ | grep rc-
	  
	  -rw-r--r--  1 root root   716 Apr 21 12:54 rc-local.service
	  
	  drwxr-xr-x  2 root root  4096 Apr 26 04:29 rc-local.service.d/
	  
	  root@360che:~# ll /lib/systemd/system/ | grep rc-
	  
	  -rw-r--r--  1 root root   716 Apr 21 12:54 rc-local.service
	  
	  drwxr-xr-x  2 root root  4096 Apr 26 04:29 rc-local.service.d/
	  
	  ```
	- 可能你要问 /usr/lib/systemd/system 和 /lib/systemd/system 有什么区别，我就简单说一下：
		- ### 1\. 区别：
			- 我们看下 /usr/lib/systemd/system 和 /lib/systemd/system 、/etc/systemd/system 这三个目录有什么区别呢。
			- > * /usr/lib/systemd/system/ 软件包安装的单元
			- > * /lib/systemd/system 实际就是/usr/lib/systemd/system，因为 /lib 实际是 /usr/lib 的一个软链接,可以通过 ll / 命令察看。
			- ![image.png](J:\Logseq\assets\91d1b68334df40c4a90e1bd632debf5d~tplv-k3u1fbpfcp-zoom-in-crop-mark_3024_0_0_0.png)
			- > * /etc/systemd/system/ 系统管理员安装的单元, 优先级更高
			- ### 2\. 优先级：
				- systemd的使用大幅提高了系统服务的运行效率, 而unit的文件位置一般主要有三个目录：
				- > /etc/systemd/system
				- >  /run/systemd/system
				- > /lib/systemd/system
				- 这三个目录的配置文件优先级依次从高到低，如果同一选项三个地方都配置了，优先级高的会覆盖优先级低的。
				- 系统安装时，默认会将unit文件放在/lib/systemd/system目录。如果我们想要修改系统默认的配置，比如nginx.service，一般有两种方法：
					- > 1）在/etc/systemd/system目录下创建nginx.service文件，里面写上我们自己的配置。
					- > 2）在/etc/systemd/system下面创建nginx.service.d目录，在这个目录里面新建任何以.conf结尾的文件，然后写入我们自己的配置。推荐这种做法。
					- > /run/systemd/system这个目录一般是进程在运行时动态创建unit文件的目录，一般很少修改，除非是修改程序运行时的一些参数时，即Session级别的，才在这里做修改。
	- **了解了以上信息，我们可以创建一个 systemd 服务，开机执行指定脚本（rc.local）的内容来实现开机自启动。**
	- ## 三、配置实现
	- 将rc-local服务设置为开机自启动（本文操作都在root用户下，或使用sudo）。
		- **1\. 首先将rc-local.service文件复制到system目录下**
			- ```clojure
			  cp /usr/lib/systemd/system/rc-local.service /etc/systemd/system/
			  ```
			- * rc-local.service 最下面增加以下内容：
				- ```clojure
				  [Install] 
				  WantedBy=multi-user.target 
				  Alias=rc-local.service
				  ```
			- **2\. 新建rc.local文件**
				- 大家不用奇怪，ubuntu20.04中 /etc/ 目录下是没有 rc.local 文件的，需要我们手动建立一个。
					- ```touch /etc/rc.local chmod 755 /etc/rc.local echo '''#!/bin/bash''' >> /etc/rc.local```
				- 设置开机启动rc-local
					- ```systemctl start rc-local systemctl enable rc-local init 6```
		- 重启系统后，通过命令systemctl status rc-local查看服务已经正常开启了。