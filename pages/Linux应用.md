## DDNS-GO
	- 从 Releases 下载并解压 ddns-go,如：https://github.com/jeessy2/ddns-go/releases/download/v5.6.0/ddns-go_5.6.0_linux_x86_64.tar.gz
	- 双击运行, 如没有找到配置, 程序将自动打开 http://127.0.0.1:9876
	- [可选] 安装服务
	- Mac/Linux: sudo ./ddns-go -s install
	- Win(以管理员打开cmd): .\ddns-go.exe -s install
	- [可选] 服务卸载
	- Mac/Linux: sudo ./ddns-go -s uninstall
	- Win(以管理员打开cmd): .\ddns-go.exe -s uninstall
	- [可选] 支持安装或启动时带参数 -l监听地址 -f同步间隔时间(秒) -c自定义配置文件路径 -noweb不启动web服务 -skipVerify跳过证书验证。如：./ddns-go -l :9877 -f 600 -c /Users/name/ddns-go.yaml
- ## 下载命令
	- ```clojure
	  # wget
	  # 是非交互式的，可以轻松地在后台工作 
	  wget -N --no-check-certificate https://xxxx.xx.com/xx.sh && bash xx.sh   
	  
	  curl -LJO https://github.com/ctripcorp/apollo/releases/download/v1.5.1/apollo-adminservice-1.5.1-github.zip
	  ```
- ## 查找文件
	- ```clojure
	  # 查文件：
	  find ./ -name "text.txt"
	  
	  # 查含文字 的文件
	  find ./ -type f -name "*.*" |xargs grep "hello"
	  ```
- ## 文本重定向
	- ```clojure
	  # cat 命令后的 >> 即为添加文件内容，如果使用 > 则是覆盖文件内容。 以 EOF 为结束标记
	  cat >> /etc/sysctl.conf << EOF
	  net.ipv6.conf.all.autoconf = 0
	  net.ipv6.conf.default.autoconf = 0
	  net.ipv6.conf.all.accept_ra = 0
	  net.ipv6.conf.default.accept_ra = 0
	  net.ipv6.conf.all.disable_ipv6 = 1
	  net.ipv6.conf.default.disable_ipv6 = 1
	  net.ipv6.conf.lo.disable_ipv6 = 1
	  net.ipv6.conf.eth0.disable_ipv6 = 1
	  EOF
	  
	  #用 sed 命令取消文件中的注释符#，注意转义字符\
	  sed -i 's/#precedence ::ffff:0:0\/96  100/precedence ::ffff:0:0\/96  100/' /etc/gai.conf
	  
	  ```
- ## [[Ubuntu20.04 设置开机自启]]
- ## [[linux screen命令让程序在后台运行]]
-
-