- 摘自 https://jkboy.com/archives/44209.html
- ### 优缺点
	- ***优点：***需公网实现远程访问局域网任意设备，没有任何费用。
	- ***缺点：***速度慢，官方没提供openwrt软件包，需要从github下载二进制文件
- ### 安装
	- ```clojure
	  #下载二进制文件
	  VERSION="2023.7.2"
	  curl -O -L https://github.com/cloudflare/cloudflared/releases/download/${VERSION}/cloudflared-linux-amd64 && chmod +x cloudflared-linux-amd64 && mv cloudflared-linux-amd64 /usr/bin/cloudflared
	  #创建一个 init.d 服务
	  touch /etc/init.d/cloudflared
	  chmod +x /etc/init.d/cloudflared
	  
	  #将以下内容添加到/etc/init.d/cloud dflared
	  
	  #!/bin/sh /etc/rc.common
	  
	  USE_PROCD=1
	  START=95
	  STOP=01
	  
	  cfd_init="/etc/init.d/cloudflared"
	  cfd_token="<yourtoken>"
	  
	  boot()
	  {
	      ubus -t 30 wait_for network.interface network.loopback 2>/dev/null
	      rc_procd start_service
	  }
	  
	  start_service() {
	      if [ $("${cfd_init}" enabled; printf "%u" ${?}) -eq 0 ]
	      then
	          procd_open_instance
	          procd_set_param command /usr/bin/cloudflared --no-autoupdate tunnel run --token ${cfd_token}
	          procd_set_param stdout 1
	          procd_set_param stderr 1
	          procd_set_param respawn ${respawn_threshold:-3600} ${respawn_timeout:-5} ${respawn_retry:-5}
	          procd_close_instance
	      fi
	  }
	  
	  stop_service() {
	      pidof cloudflared && kill -SIGINT `pidof cloudflared`
	  } 
	  #确保用在 Cloudflare 的 web GUI 中创建通道时生成的实际令牌替换“ yourtoken ”并保存更改即可
	  ```
	- ```clojure
	  #启用 Cloudflred init.d  服务并以下列方式启动:
	  /etc/init.d/cloudflared enable
	  /etc/init.d/cloudflared start
	  
	  #查看运行状况
	  ps | grep cloudflared
	  logread | grep cloudflared
	  
	  ```