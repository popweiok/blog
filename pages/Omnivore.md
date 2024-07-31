## 🔖 文章
	- [windows通过浏览器访问noVNC（基于web的远程桌面）-CSDN博客](https://omnivore.app/me/windows-no-vnc-web-csdn-191074a300e)
	  site:: [blog.csdn.net](https://blog.csdn.net/weixin_58448088/article/details/129834143)
	  author:: 成就一亿技术人!
	  date-saved:: [[Wed, 2024/07/31]]
	  date-published:: [[Sat, 2024/07/27]]
		- ### 内容
			- ![](https://proxy-prod.omnivore-image-cache.app/0x0,sb9FtdCb_T4b1O697eqoXY9k3NVVgRsw8SlTyVuCA3pc/https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)
			  
			  版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。
			  
			  **目录**
			  
			  [一、什么是VNC 和 noVNC？](\#t0)
			  
			  [二、Windows10安装及配置noVNC](\#t1)
			  
			  [2.0、注释](\#t2)
			  
			  [2.1、下载UltraVNC](\#t3)
			  
			  [2.2、下载Node.js](\#t4)
			  
			  [2.3、下载安装git](\#t5)
			  
			  [2.4、创建一个存放文件的文件夹](\#t6)
			  
			  [2.5、安装ws、optimist、mime-types模块（执行websockify.js文件所需）](\#t7)
			  
			  [2.6、下载noVNC、下载websockify-js](\#t8)
			  
			  [2.7、修改websockify.js文件](\#t9)
			  
			  [ 2.8、查看自己电脑主机IP](\#t10)
			  
			  [2.9、执行websockify.js](\#t11)
			  
			  [三、成果展示](\#t12)
			  
			  ---
			  
			  \#\# 一、什么是VNC 和 noVNC？
			  
			  VNC (Virtual Network Console)是虚拟网络控制台的缩写，分为server端和client端两部分，分别部署完成后在server端简单的配置即可使用，基于TCP的通信。noVNC项目是通过取消VNC Client的安装，直接通过浏览器访问noVNC，然后由noVNC间接访问VNC server来达到client web化。VNC server处理的始终是TCP流量，但是浏览器和noVNC之间是在http基础上使用WebSocket交互，由于VNC server 无法处理websocket流量，因此引入了 [websockify](https://link.zhihu.com/?target=https%3A//github.com/novnc/websockify "websockify") ，noVNC的姐妹项目，负责把WebSocket流量转换为普通的TCP流，使VNC server正常工作。noVNC其实是一个HTML形式的APP，[websockify](https://link.zhihu.com/?target=https%3A//github.com/novnc/websockify "websockify")并充当了一个mini web server的角色，当浏览器访问时，会通过网络加载运行noVNC。
			  
			  \#\# 二、Windows10安装及配置noVNC
			  
			  \#\#\# 2.0、注释
			  
			  我会上传和我版本一样的UltraVNC、Node.js安装包，可以免费下载，所需积分为0。
			  
			  UltraVNC安装包下载：
			  
			  [UltraVNC安装包资源-CSDN文库https://download.csdn.net/download/weixin\_58448088/87626312?spm=1001.2014.3001.5503](https://download.csdn.net/download/weixin%5F58448088/87626312?spm=1001.2014.3001.5503 "UltraVNC安装包资源-CSDN文库")Node.js安装包下载：
			  
			  [node.js(v16.16.0)安装包资源-CSDN文库https://download.csdn.net/download/weixin\_58448088/87626324?spm=1001.2014.3001.5503](https://download.csdn.net/download/weixin%5F58448088/87626324?spm=1001.2014.3001.5503 "node.js(v16.16.0)安装包资源-CSDN文库")
			  
			  \#\#\# 2.1、下载UltraVNC
			  
			  [Home - UltraVNC VNC OFFICIAL SITE, Remote Desktop Free Opensource (uvnc.com)https://uvnc.com/](https://uvnc.com/ "Home - UltraVNC VNC OFFICIAL SITE, Remote Desktop Free Opensource (uvnc.com)")
			  
			  选择你想要下载的版本，选择好下载的路径，傻瓜式安装即可，一直next就好。
			  
			  安装好后，找到安装的文件夹，找到uvnc\_settings.exe鼠标右键，点击以管理员身份运行，就会弹出如下页面，第一张为默认端口号，第二张设置远程访问VNC密码，设置远程只查看密码。
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/865x649,sYeg4f__Jtykh4sngbIvIC191TPsX89U4JKVE8hMbsas/https://i-blog.csdnimg.cn/blog_migrate/0c86a538d5817cde35b2018c558b0664.png){:height 530, :width 696}
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/866x648,sfkw273JSAxc0BsWiB3LT0uum9J2Fmw7HwrCA5ZJJsnc/https://i-blog.csdnimg.cn/blog_migrate/20535fbaa2abb40330e5aa4c953ad631.png)
			  
			  \#\#\# 2.2、下载Node.js
			  
			  下载node.js是为了执行websockify.js
			  
			  [配置Node.js环境变量\_王昭没有君啊的博客-CSDN博客https://blog.csdn.net/weixin\_58448088/article/details/129838885?spm=1001.2014.3001.5501](https://blog.csdn.net/weixin%5F58448088/article/details/129838885?spm=1001.2014.3001.5501 "配置Node.js环境变量_王昭没有君啊的博客-CSDN博客")
			  
			  \#\#\# 2.3、下载安装git
			  
			  [git安装和使用\_git安装使用\_王昭没有君啊的博客-CSDN博客详细介绍git工具如何安装，手把手教你一步步怎样创建远程仓库和远程仓库分支，怎样解决代码冲突，怎样回退版本，怎样克隆代码https://blog.csdn.net/weixin\_58448088/article/details/123187457?spm=1001.2014.3001.5501](https://blog.csdn.net/weixin%5F58448088/article/details/123187457?spm=1001.2014.3001.5501 "git安装和使用_git安装使用_王昭没有君啊的博客-CSDN博客")
			  
			  在D盘新建VNC文件夹，准备存放noVNC所需的文件
			  
			  \#\#\# 2.5、安装ws、optimist、mime-types模块（执行websockify.js文件所需）
			  
			  打开cmd，并进入到D盘下的VNC文件夹中，执行如下命令
			  
			  ```cmake
			  
			  npm install ws
			  
			  npm install optimist
			  
			  npm install mime-types
			  
			  ```
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/992x529,s3Ff5kkW--eiLslLct56XHTy6diSfT1flH5o54v9IP54/https://i-blog.csdnimg.cn/blog_migrate/0ea58f26c92c00404ecc944c6b3dc2c3.png)
			  
			  安装好这些 模块后，会在VNC文件中自动生成如下文件
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/863x650,sgwc7pxfJI7_dihjqqxfs94e4PDx_RZsRoBdJT0W-GHM/https://i-blog.csdnimg.cn/blog_migrate/a96f243b1ae04a56bc05e1562150935f.png)
			  
			  \#\#\# 2.6、下载noVNC、下载websockify-js
			  
			  进入VNC文件中的node\_modules文件夹中,鼠标右键选择 Git Bash Here
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/296x426,saACIUaDTWX6vD_7Jvo0DnaazNUUuqDLpGh2gKt1HHGI/https://i-blog.csdnimg.cn/blog_migrate/7beeb5e22e5634be8349cc7ed8fffac4.png)
			  
			  进入到如下窗口，通过git下载noVNC、websockify-js
			  
			  ```crmsh
			  
			  git clone https://github.com/novnc/noVNC
			  
			  git clone https://github.com/novnc/websockify-js.git
			  
			  ```
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/873x665,sxygKJyDqjLSa3zi73L5Ou5umjEqWjLnL2bearCyY5n4/https://i-blog.csdnimg.cn/blog_migrate/6eb76ed12f8d5ca6780e85b1c959d84d.png)
			  
			  \#\#\# 2.7、修改websockify.js文件
			  
			  修改 D:\\VNC\\node\_modules\\websockify-js\\websockify中的websockify.js，将
			  
			  ```makefile
			  filename += '/index.html';
			  ```
			  
			  改为
			  
			  ```makefile
			  filename += '/vnc.html';
			  ```
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1022x726,svUo0Nh_0DoGf3ATO0ZknHkIgiTgxeIvJVnITwtwAfdE/https://i-blog.csdnimg.cn/blog_migrate/5a23a410c00c02dc187a6b74d8fc965f.png)
			  
			  \#\#\#  2.8、查看自己电脑主机IP
			  
			  打开cmd，输入如下命令行，如图红圈框住的就是主机IP
			  
			  ```dos
			  ipconfig
			  ```
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1009x537,sE_qWv4gsFnoo87E9WESiERJtbtsRCS7WpWI0BTVxUcA/https://i-blog.csdnimg.cn/blog_migrate/469ec5410b803303da56f7a75406bd7e.png)
			  
			  \#\#\# 2.9、执行websockify.js
			  
			  说明一下整个过程，我这边通过安装UltraVNC（提供VNC Server），然后在VNC Server中通过node执行websockify.js：转发9000端口的http链接到5900端口，就可以正常运行noVNC了。
			  
			  打开cmd输入如下命令,启动代理服务：
			  
			  ```taggerscript
			  node D:\VNC\node_modules\websockify-js\websockify\websockify.js --web D:\VNC\node_modules\noVNC 8000 localhost:5900
			  ```
			  
			  命令行解释：
			  
			  ```taggerscript
			  
			  node  // 使用node执行websockify.js
			  
			  D:\VNC\node_modules\websockify-js\websockify\websockify.js // websockify.js文件路径
			  
			  --web D:\VNC\node_modules\noVNC // noVNCD文件路径
			  
			  8000 // 启动端口为8000，这个可以自己设，不一定就是8000
			  
			  localhost:5900 // 转发的VNC地址和端口
			  
			  ```
			  
			  \#\# ![](https://proxy-prod.omnivore-image-cache.app/1036x541,sfsDxNxhGFNiemXiPr0Eu4-BCiSoSqhixayyZmp7Kgxk/https://i-blog.csdnimg.cn/blog_migrate/8b89f085a61f2d160196d92c8cee758a.png)
			  
			  \#\# 三、成果展示
			  
			  3.1、在浏览器输入http:// 主机ip:启动端口/vnc.html
			  
			  示例：
			  
			  ```dts
			  http://172.16.8.107:8000/vnc.html
			  ```
			  
			  3.2、输入网址后，进入如下页面点击连接
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1151x758,s17ifOKRJBvhDbH5c8OFjYSuC8L73viQjOfd3BfWnK3o/https://i-blog.csdnimg.cn/blog_migrate/bd05c99c015205b0bd857d8326ea7c3d.png)
			  
			  3.3、点击连接跳转到如下页面，输入之前设置的远程访问VNC密码，回车
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1151x758,ssCsGbNLVgjAtLbR5bMGtV8PXzZD7K4N7ImHd9110JQ8/https://i-blog.csdnimg.cn/blog_migrate/31855fd29243816a8bb589bb3415ca9b.png)
			  
			  3.4、成功连接到本机
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1200x1080,sYFGfYgJizKo-lZzaPA6YvVtRxW_RWTluRy2nmpL59MU/https://i-blog.csdnimg.cn/blog_migrate/b3b8537452e8ed8d59d54d639708149a.png)
			  
			  3.5、注意连接后，不要关掉cmd，要保持web server代理服务一直开启
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1053x597,sbMRaH3At-vNMC2c0QDmX6dzfB7tGVkeOx3SGxfFUbMc/https://i-blog.csdnimg.cn/blog_migrate/65e1dce5ea0fb452eeded2302572aa09.png)
			  
			  3.6、关闭代理服务，ctrl+c
			  
			  ![](https://proxy-prod.omnivore-image-cache.app/1048x591,sH9YySK5zZjviWRzDSKqRuNvqNGZc_85MOAyPrRCN2x0/https://i-blog.csdnimg.cn/blog_migrate/31c8e50a3ee892c0536de5222bb9e05e.png)
			  
			  文章知识点与官方知识档案匹配，可进一步学习相关知识