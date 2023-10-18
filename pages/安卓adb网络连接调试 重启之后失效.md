#### 解决重启失效的方法：
- *需要root权限*
- **第一种方法：**
	- 在/system/build.prop 文件中加入service.adb.tcp.port=5555
	- 1，cmd命令行中执行adb shell
	- 2，执行su 获取root权限；*注意：获取root权限不同的设备方式不同*
	- 3，执行 `echo service.adb.tcp.port=5555 >> /system/build.prop` 把service.adb.tcp.port=5555挂在到build.prop文件中,并且是以追加的方式；
	- >：表示输出，会覆盖文件原有的内容
	- >>：表示追加，会将内容追加到已有文件内容的末尾
	- 重新挂载
	- 4，如果执行echo service.adb.tcp.port=5555 >> /system/build.prop 提示build.prop是只读文件，那么需要重现挂在system目录
	- 5，挂在也是需要root权限，获取root权限之后执行 mount -o remount rw /system （重新挂载system目录为可读可写）
	- 6，最后再执行 echo service.adb.tcp.port=5555 >> /system/build.prop
	- 注意：4，5，6不是非必须，build.prop为只读文件时才要执行挂在命令；
	- 重启生效
	  ————————————————
	  版权声明：本文为CSDN博主「Ang_qq_252390816」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
	  原文链接：https://blog.csdn.net/ezconn/article/details/103358710