- ## ip相关
	- 使用DHCP将网络配置重置回默认连接配置文件
		- ```clojure
		  rm -r /mnt/overlay/etc/NetworkManager/system-connections
		  reboot
		  ```
	- 使用 nmcli 设置静态IPv4地址
		- ```clojure
		  #在 ha > 提示符下，键入 login
		  nmcli con show "Home Assistant OS default" #将列出连接的所有属性
		  nmcli con edit "Home Assistant OS default"   #开始编辑“家庭助手操作系统默认值”的配置设置
		    
		  nmcli> set ipv4.addresses 192.168.100.10/24  #添加您的静态IP地址（手动方法选择“是”）  
		    
		  nmcli> set ipv4.dns 192.168.100.1  
		  nmcli> set ipv4.gateway 192.168.100.1  
		    
		  nmcli> print ipv4           #将显示此连接的IPv4属性  
		    
		  nmcli> save                  #您将在之后保存更改  
		    
		  nmcli con reload       #重载网络，并不总是有效
		  ```
	- 查看默认连接
		- `cat /etc/NetworkManager/system-connections/default`
- HomeAssistant反向代理后访问出现400 BadRequest处理
	- ```clojure
	  # configuration.yaml加入以下代码:
	    http:  
	    use_x_forwarded_for: true  
	    trusted_proxies:  
	    192.168.123.0/24 # Add the IP address of the proxy server
	  ```