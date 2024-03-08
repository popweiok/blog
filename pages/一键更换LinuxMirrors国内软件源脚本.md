### `GNU/Linux` 一键更换国内软件源脚本
	- 中国大陆
		- ```
		  bash <(curl -sSL https://linuxmirrors.cn/main.sh)
		  # bash <(curl -sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh)
		  ```
	- 大陆教育网
		- ```
		  bash <(curl -sSL https://linuxmirrors.cn/main.sh) --edu
		  # bash <(curl -sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh) --edu
		  ```
	- 海外
		-
	- 注意
		- *Debian 系 Linux 默认禁用了源码仓库和预发布软件源，若需启用请将 list 源文件中相关内容的所在行 取消注释。*
		- *RedHat 系 Linux 配置了所有可以配置的仓库，但有一些仓库默认没有启用，若需启用请将 repo 源文件中的 enabled=0修改成 enabled=1*。
- ### `Docker` 一键安装脚本
	- `bash <(curl -sSL https://cdn.jsdelivr.net/gh/SuperManito/LinuxMirrors@main/DockerInstallation.sh)`
	- *脚本集成安装 [`Docker Engine`](https://docs.docker.com/engine) 和 [`Docker Compose`](https://docs.docker.com/compose)，可选择安装版本、下载软件源、镜像加速器，支持海内外服务器环境和 `arm` 架构处理器环境使用*
	- 注意
		- Docker CE：*Docker Community Edition 镜像仓库，用于下载并安装 Docker 相关软件包*。
		- Docker Hub：*Docker Hub 镜像仓库，默认为官方提供的公共库，用于切换下载镜像时的来源仓库，简称镜像加速器。*
-