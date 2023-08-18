- ## 先删除企业源
  logseq.order-list-type:: number
	- ```clojure
	  rm /etc/apt/sources.list.d/pve-enterprise.list
	  ```
	-
- ## 安装：
  logseq.order-list-type:: number
	- logseq.order-list-type:: number
	  ```clojure
	  export LC_ALL=en_US.UTF-8
	  apt update && apt -y install git && git clone https://github.com/ivanhao/pvetools.git
	  ```
	- 或一键安装
	  logseq.order-list-type:: number
		- ```clojure
		  echo "nameserver  8.8.8.8" >> /etc/resolv.conf && rm /etc/apt/sources.list.d/pve-enterprise.list && export LC_ALL=en_US.UTF-8 && apt update && apt -y install git && git clone https://github.com/ivanhao/pvetools.git && cd pvetools && ./pvetools.sh
		  ```
- ## 启动工具（cd到目录，启动工具）
  logseq.order-list-type:: number
	- ```clojure
	  cd pvetools
	  ./pvetools.sh
	  
	  #如果提示没有权限，输入chmod +x ./*.sh
	  ```
- ## 卸载：
  logseq.order-list-type:: number
	- 删除下载的pvetools目录即可
-