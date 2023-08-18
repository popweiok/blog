- ```clojure
  - #查看正在运行的容器
  - docker ps
  - #查看所有容器
  - docker ps -a
  - #启动容器
  - docker start <ContainerId(或者name)>
  - #停止容器
  - docker stop <ContainerId(或者name)>
  - #重启容器
  - docker restart <ContainerId(或者name)>
  - #删除容器
  - docker rm <ContainerId(或者name)>
  - #删除所有容器
  - docker rm $(docker ps -a -q)
  -
  - #**进入容器**
  - docker exec -it containerID /bin/bash
  - #管理员权限进入容器
  - docker exec -it -u root containerID /bin/bash
  - ctrl+d 退出容器且关闭
  - ctrl+p+q 退出容器但不关闭
  -
  - 查看容器日志
  - docker logs -f -t --tail 行数 容器名
  -
  - 网络操作
  - #在主机上创建一个网络
  - docker network create mynet
  - #查看所有网络
  - docker network ls
  - #查看某个网络详情
  - docker network inspect mynet
  - #容器连接某一个网络
  - docker network connect container
  - #容器断开已连接的网络
  - docker network disconnet mynet 容器ID
  - #移除某个网络
  - docker network rm mynet
  - #移除所有网络
  - docker network prune
  -
  - 拷贝操作
    宿主机和容器之间的拷贝操作使用 docker cp 命令，并且无论容器是否启动都生效  
  - #1.文件从宿主机拷贝到容器:    docker cp 宿主机文件路径   容器名:存放路径
    docker cp /home/jenkins/test.txt jenkins:/var/jenkins_home  
  - #2.文件从容器拷贝到宿主机   docker cp 容器名:要拷贝的文件路径  宿主机存放路径  
    docker cp jenkins:/var/jenkins_home/test.txt /home/jenkins  
  -
  - #审查默认bridge网络
  - docker network inspect bridge
  
  使用 docker run -v 进行本地目录挂载
  -v 主机目录:容器目录。
  ```
- ## Docker ipv6
	- Docker默认没有启用IPV6支持，需要通过修改配置文件启用
	- Docker启用IPV6支持后，默认会同时监听IPV4和IPV6
	- 如果只需要IPV6地址访问，-p参数后面需要通过[ipv6地址]来指定，IPV6地址需要使用[]
		- Docker启用ipv6
			- ```clojure
			  修改Docker配置文件/etc/docker/daemon.json，如果没有就自己新建一个
			  {
			    "ipv6": true,
			    "fixed-cidr-v6": "2001:db8:1::/64"
			  }
			  2001:db8:1::/64是一个虚拟的IPV6网段，保持上面默认的复制下来即可。
			  systemctl reload docker重载一次Docker服务                
			  ```
		- 仅监听IPV6地址
			- 默认情况下我们可以通过-p参数指定监听IP和映射端口，比如我们可以指定监听127.0.0.1:80可以这样做：`docker run -itd --name=&quot;onenav&quot; -p 127.0.0.1:80:80  -v /data/onenav:/data/wwwroot/default/data helloz/onenav:0.9.27`
			- 如果只需要支持IPV6监听和访问，那么在启动容器的时候我们需要在-p参数后面使用[]来指定IPV6的IP监听，比如：`docker run -itd --name=&quot;onenav&quot; -p [2a12:a301:2::1126]:80:80 -v /data/onenav:/data/wwwroot/default/data helloz/onenav:0.9.27`
				- 如果指定IPV6地址监听，需要使用[]括起来
				- 2a12:a301:2::1126改成您自己的公网IPV6地址
		- 如果需要同时支持IPV4和IPV6的监听和访问，那么只需要去掉-p参数后面的IP地址，仅保留端口即可，比如：`docker run -itd --name=&quot;onenav&quot; -p 80:80 -v /data/onenav:/data/wwwroot/default/data helloz/onenav:0.9.27`
-