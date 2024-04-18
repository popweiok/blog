### 开启ssh
	- #### 安装
		- ```clojure
		  sudo systemctl enable sshd && sudo systemctl start sshd
		  ```
		- ```clojure
		  #检查服务状态 
		  systemctl status sshd.service
		  ```
	- #### 允许root访问
	  collapsed:: true
		- ```clojure
		  # SSH 放行
		  sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
		  sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config
		  
		  # 重启服务
		  sudo service sshd restart
		  ```
- ### 桌面启用root用户
	- ```clojure
	  #!/bin/bash 
	  #set root password
	  sudo passwd root
	   
	  #notes Document content
	  sudo sed -i "s/.*root quiet_success$/#&/" /etc/pam.d/gdm-autologin
	  sudo sed -i "s/.*root quiet_success$/#&/" /etc/pam.d/gdm-password
	   
	  #modify profile
	  sudo sed -i 's/^mesg.*/tty -s \&\& mesg n \|\| true/' /root/.profile
	   
	  #install openssh
	  sudo apt install openssh-server
	   
	  #delay
	  sleep 1
	   
	  #modify conf
	  sudo sed -i 's/^#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
	   
	  #restart server
	  sudo systemctl restart sshd
	  ```
- ### 修改时区
	- ```
	  sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
	  
	  #或者用timedatectl设置
	  timedatectl set-timezone Asia/Shanghai
	  
	  ```
	-
- ### 常用软件安装
	- ```
	  apt install zsh git vim curl -y
	  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
	  ```
- ### 网络配置
	- #### 打开网卡配置文件进行设置
		- ```clojure
		  sudo vim /etc/netplan/00-installer-config.yaml
		  # This is the network config written by 'subiquity'
		  network:
		    ethernets:
		      ens32:
		        dhcp4: no
		        dhcp6: no
		        addresses:
		          - 192.168.74.103/24
		        routes:
		          - to: default
		            via: 192.168.74.2
		        nameservers:
		          addresses: [114.114.114.114, 223.5.5.5]
		    version: 2
		  ```
	-
	- #### 手动配置多IP
		- ```clojure
		  network:
		    version: 2
		    renderer: networkd
		    ethernets:
		      eno1:
		        addresses:
		          - 111.222.111.222/32
		          - 111.222.111.223/32	#新增的IPv4 1
		          - 111.222.111.224/32	#新增的IPv4 2
		          - 2a01:2a01:2a01:2a01::2/64
		          - 2a01:2a01:2a01:2a02::2/64	#新增的IPv6 1
		          - 2a01:2a01:2a01:2a03::2/64	#新增的IPv6 2
		        routes:
		          - on-link: true
		            to: 0.0.0.0/0
		            via: 111.222.111.1
		          - to: default
		            via: 2a01::1
		        nameservers:
		          addresses:
		            - 192.12.64.2
		            - 2a01:2a01:2a01::add:1
		            - 192.12.64.1
		            - 2a01:2a01:2a01::add:2
		  ```
		-
	- #### 使配置生效
		- ```clojure
		  sudo netplan apply
		  ```
		-
- ### 配置国内源
	- **Universe 和 multiverse 存储库**
		- Universe 库提供社区维护的免费开源软件，Ubuntu 不保证其定期的安全更新
		- Multiverse 提供的软件不是免费和开源的，因此，Ubuntu 不提供更新和 bug 修复
	- ```clojure
	  #中科大
	  sudo bash -c "cat << EOF > /etc/apt/sources.list && apt update 
	  deb https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
	  deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
	  deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
	  deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
	  deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
	  deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
	  deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
	  deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
	  deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
	  deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
	  EOF"
	  ```
- ### 软件包的依赖关系
	- **APT**
		- ```clojure
		  apt show <Package_name>
		  
		  apt-cache depends <Package_name>
		  ```
	- **DPKG**
		- ```clojure
		  dpkg --info < path_of_deb_file >
		  
		  #例如：我们有一个超级终端的 deb 文件。要获取此 deb 文件的依赖项详细信息
		  dpkg --info hyper_3.2.3_amd64.deb
		  ```
- ### 查找软件安装路径
	- ```clojure
	  ps -aux 
	  find 或 whereis
	  dpkg-query -L APPLICATION-NAME #查看已安装软件包
	  dpkg -l softwarename #查看软件版本
	  dpkg -S softwarename #显示包含此软件包的所有位置
	  dpkg -L softwarename #显示安装路径
	  which [-a] filename
	  whereis [options] [-BMS directory... -f] filename
	  ```
- ### 常用命令
	- #### 解压文件 `tar -zxvf 文件名`
	- #### 复制源目录 为 dir1 ,目标目录为dir2
		- ```clojure
		  # 如果dir2目录不存在
		  [root@zcwyou ~]# cp -r dir1 dir2
		  
		  如果dir2目录已存在
		  [root@zcwyou ~]# cp -r dir1/. dir2
		  ```
	- **测试ipv6/4优先**  `curl test.ipw.cn`
	  id:: 64c77e10-49a8-4379-af03-c488927c0b6f
	- **网络侦听**
		- ```clojure
		  #显示主机名
		  ss -tunlp 
		  #显示数字端口
		  ss -tulp
		  
		  ss -ano |grep 端口或进程名
		  ```
	- **APT命令**
		- ```clojure
		  #刷新本地软件包索引 
		  apt update
		  #浏览待升级的包列表
		  apt list --upgradable
		  #升级所有包
		  apt upgrade
		  ```
	- **进程**    `ps -aux|grep 进程名`
	-
- ### 防火墙
	- ```clojure
	  #查看防火墙状态
	  sudo ufw status
	  #安装防火墙
	  sudo apt install ufw -y
	  #卸载防火墙
	  sudo apt remove ufw -y
	  #启用防火墙
	  sudo ufw enable
	  #禁用防火墙
	  sudo ufw disable
	  #临时停用防火墙
	  sudo systemctl stop ufw
	  
	  #举例：
	  ##添加允许通过防火墙的规则
	  例1：输入命令：sudo ufw allow 6379
	  ##允许端口号为6379的端口访问
	  例2：输入命令：sudo ufw allow 80:90/tcp
	  ##允许80-90之间的端口访问，如80，81，82 ··· 90
	  例3：输入命令：sudo ufw delete allow 22
	  删除允许端口为22的规则
	  
	  大多数系统只需要打开少量的端口接受传入连接，并且关闭所有剩余的端口。 
	  从一个简单的规则基础开始，ufw default命令可以用于设置对传入和传出连接的默认响应动作。 
	  要拒绝所有传入并允许所有传出连接，那么运行：
	  sudo ufw default allow outgoing
	  sudo ufw default deny incoming
	  ufw default 也允许使用 reject 参数。
	  警告：除非明确设置允许规则，否则配置默认 deny 或 reject 规则会锁定你的服务器。确保在应用默认 deny 或 reject 规则之前，已按照下面的部分配置了 SSH 和其他关键服务的允许规则。
	  
	  高级规则：
	  除了基于端口的允许或阻止，UFW 还允许您按照 IP 地址、子网和 IP 地址/子网/端口的组合来允许/阻止。
	  允许从一个 IP 地址连接：
	  sudo ufw allow from 123.45.67.89
	  允许特定子网的连接：
	  sudo ufw allow from 123.45.67.89/24
	  允许特定 IP/ 端口的组合：
	  sudo ufw allow from 123.45.67.89 to any port 22 proto tcp
	  proto tcp 可以删除或者根据你的需求改成 proto udp，所有例子的 allow 都可以根据需要变成 deny。
	  ————————————————
	  版权声明：本文为CSDN博主「SunshineScorpio、」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
	  原文链接：https://blog.csdn.net/SunshineScorpio/article/details/124153109
	  
	  ```
	-
-
- ### 查看Ubuntu版本号
	- `lsb_release -a `
-
- ### ubuntu 安裝Qemu guest agent
	- ```clojure
	  apt update && apt install qemu-guest-agent -y
	  systemctl enable qemu-guest-agent
	  systemctl start qemu-guest-agent
	  
	  ```
	- 查看狀態使用 systemctl status qemu-guest-agent
- ### [[Ubuntu20.04 设置开机自启]]
-