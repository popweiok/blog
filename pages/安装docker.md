- ### 一键安装Docker
	- https://gitee.com/SuperManito/LinuxMirrors
	- ```clojure
	  bash <(curl -sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/DockerInstallation.sh)
	  ```
- ### portainer-ce中文版
	- 源码仓库https://github.com/eysp/portainer-ce
	- ```clojure
	  docker run -d --restart=always --name="portainer" -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock 6053537/portainer-ce
	  ```
-