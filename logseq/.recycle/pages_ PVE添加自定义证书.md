- https://www.gaoxiaobo.com/web/server/158.html
	- **简介**PVE安装完毕后，打开web管理地址总是提示不安全，此时我们需要添加域名证书解决。
	- PVE添加自定义证书后台打开不怎么办？
	- PVE安装完毕后，打开web管理地址总是提示不安全，此时我们需要添加域名证书解决。
	- ![pve安装证书](https://img.gaoxiaobo.com/images/98faf8c03eafe.png)
	- 1、首先我们为自己的域名申请免费的ssl证书，此步骤略，腾讯云、阿里云后台都可以免费申请一年的域名ssl证书。 2、申请到证书后下载，得到xxxx.crt和xxxx.key
	- 3、将xxxx.crt和xxxx.key重命名为 pve-ssl.crt pve-ssl.key，然后将pve-ssl.crt和pve-ssl.key上传到pve主机的/etc/pve/nodes/xx目录下，xx表示pve节点名称，此处我用scp进行上传
	- ```clojure
	  scp pve-ssl.crt pve-ssl.key root@192.168.3.2:/etc/pve/nodes/xx
	  ```
	- 4、使用openssl命令，将pve-ssl.crt转换为pve-ssl.pem
	- ```clojure
	  openssl x509 -in pve-ssl.crt -out pve-ssl.pem -outform PEM
	  ```
	- 5、删除目录下pveproxy-ssl.key和pveproxy-ssl.pem，重新生成。此步骤非常重要，否则进入不了PVE后台。
	- ```clojure
	  - rm -rf pveproxy-ssl.key pveproxy-ssl.pem
	  - systemctl restart pveproxy
	  ```
	- 至此，我们已经成功添加PVE后台ssl证书，再次打开后台管理地址已经没有烦人的不安全提示。
-