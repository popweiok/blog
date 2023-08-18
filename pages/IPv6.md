-
- fe80 是本地链路，只用来和路由器直接 通信，剩余两个都是通过分发前缀生成的一个是基于EUI64用网卡的MAC生成，是基于网卡固定的 一般为/64
- 另一个是隐私保护地址，后缀是随机生成，有效期短些，是用来对外发起访问的r所以一般动态域名都绑定这个地址 /128
	- ### IPv4 IPv6 优先
		- **优先IPv4：**
			- 默认的安装中，IPV4 和 IPV6 并存，并且 IPV6 却优先于 IPV4。如果不需要彻底关闭 IPV6，可以设置让 IPV4 优先于 IPV6。配置方式如下：
				- ```clojure
				  echo "precedence ::ffff:0:0/96 100" >>/etc/gai.conf
				  
				  #一句话命令：
				  sed -i 's/#precedence ::ffff:0:0\/96  100/precedence ::ffff:0:0\/96  100/' /etc/gai.conf
				  ```
		- **设置 GRUB 启动参数禁用 IPv6**
			- 编辑 /etc/default/grub，找到 GRUB_CMDLINE_LINUX_DEFAULT=&quot;quiet&quot;
			- 修改为：`GRUB_CMDLINE_LINUX_DEFAULT=&quot;ipv6.disable=1 quiet&quot;`
			- 随后执行命令 update-grub 更新 grub 启动参数，重启系统即可。
	- ### 查看IPV6的IP，路由，邻居信息
		- ```clojure
		  # 添加IPV6地址
		  ip -6 addr add <ipv6address>/<prefixlength> dev <interface>
		  ip -6 addr add 2001:0db8:0:f101::1/64 dev eth0
		  
		  ifconfig <interface> inet6 add <ipv6address>/<prefixlength>
		  ifconfig eth0 inet6 add 2001:0db8:0:f101::1/64
		  
		  # 查看路由
		  ip -6 route show
		  route -A 'inet6'
		  route -6
		  
		  # 查看邻居缓存
		  ip -6 neighbor show
		  
		  # 添加默认路由
		  ip -6 route add <ipv6network>/<prefixlength> via <ipv6address>
		  ip -6 route add default via 2001:0db8:0:f101::1
		  
		  route -A inet6 add <ipv6network>/<prefixlength> gw
		  route -A inet6 add default gw 2001:0db8:0:f101::1
		  ```
	- ### pve开启IPv6
		- ```clojure
		  # 查看内核也已经开启ipv6自动配置：
		  cat /proc/sys/net/ipv6/conf/vmbr0/accept_ra
		  1
		  cat /proc/sys/net/ipv6/conf/vmbr0/autoconf
		  1
		  # 查看已开启ipv6转发：
		  cat /proc/sys/net/ipv6/conf/vmbr0/forwarding
		  1
		  # 需要将accept_ra值改成2才能自动配置SLAAC ipv6地址：
		  /etc/sysctl.conf
		  net.ipv6.conf.all.accept_ra=2
		  net.ipv6.conf.default.accept_ra=2
		  net.ipv6.conf.vmbr0.accept_ra=2
		  net.ipv6.conf.all.autoconf=1
		  net.ipv6.conf.default.autoconf=1
		  net.ipv6.conf.vmbr0.autoconf=1
		  ```
	- ### 启用 IP 转发 (IPv4 / IPv6) | 内核转发
		- #### 临时激活
			- ```clojure
			  # 查看目前状态
			  cat /proc/sys/net/ipv4/ip_forward              #IPv4
			  cat /proc/sys/net/ipv6/conf/all/forwarding     #IPv6
			  # 0 是禁用  1 是开启
			  
			  # 临时切换
			  sysctl -w net.ipv4.ip_forward=1
			  sysctl -w net.ipv6.conf.all.forwarding=1
			  #  临时修改,在重启或者使用 sysctl 时 会恢复默认
			  ```
		- #### 永久激活
			- ```clojure
			  # 永久激活需要编辑配置文件 /etc/sysctl.conf 在文件末尾加入
			  net.ipv4.ip_forward = 1
			  net.ipv6.conf.all.forwarding=1
			  
			  # 立即生效
			  sysctl -p /etc/sysctl.conf
			  ```
-