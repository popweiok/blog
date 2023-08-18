- ### 修改IP、网关，文件名为 /etc/network/interfaces
	- ```clojure
	  auto lo
	  iface lo inet loopback 
	  iface ens33 inet manual 
	  auto vmbr0
	  iface vmbr0 inet static
	  	address 192.168.100.3/24
	  	gateway 192.168.100.2
	  	bridge-ports ens33
	  	bridge-stp off
	  	bridge-fd 0
	  
	  ```
- ### 修改DNS服务器，文件名为 /etc/resolv.conf
	- ```clojure
	  search localdomain
	  nameserver 192.168.100.2
	  ```
- ### 修改主机名解析的IP，文件名为 /etc/hosts
	- ```clojure
	  127.0.0.1 localhost.localdomain localhost
	  192.168.100.3 pve.localdomain pve
	   
	  # The following lines are desirable for IPv6 capable hosts
	   
	  ::1     ip6-localhost ip6-loopback
	  fe00::0 ip6-localnet
	  ff00::0 ip6-mcastprefix
	  ff02::1 ip6-allnodes
	  ff02::2 ip6-allrouters
	  ff02::3 ip6-allhosts
	  
	  ```
- ### 修改开机界面提示的URL内容，文件名为：/etc/issue ，其实改不改都不影响使用的。
	- ```clojure
	  ------------------------------------------------------------------------------
	  Welcome to the Proxmox Virtual Environment. Please use your web browser to 
	  configure this server - connect to:
	    https://192.168.100.3:8006/
	   ------------------------------------------------------------------------------
	  
	  ```
- ###