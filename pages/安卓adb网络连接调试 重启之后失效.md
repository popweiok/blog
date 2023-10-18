#### 解决重启失效的方法：
- *需要root权限*
- **第一种方法：**
	- 在/system/build.prop 文件中加入service.adb.tcp.port=5555
	- 1，cmd命令行中执行adb shell
	- 2，执行su 获取root权限；*注意：获取root权限不同的设备方式不同*
	- 3，执行 `echo service.adb.tcp.port=5555 >> /system/build.prop` 把service.adb.tcp.port=5555挂在到build.prop文件中,并且是以追加的方式；
		- `>`  表示输出，会覆盖文件原有的内容
		- `>>` 表示追加，会将内容追加到已有文件内容的末尾
	- 重新挂载
	- 4，如果执行`echo service.adb.tcp.port=5555 >> /system/build.prop` 提示build.prop是只读文件，那么需要重现挂在system目录
	- 5，挂在也是需要root权限，获取root权限之后执行` mount -o remount rw /system` （重新挂载system目录为可读可写）
	- 6，最后再执行` echo service.adb.tcp.port=5555 >> /system/build.prop`
	- *注意：4，5，6不是非必须，build.prop为只读文件时才要执行挂在命令；*
	- 重启生效
- 第二种方法：
	- 也是在/system/build.prop 文件中加入service.adb.tcp.port=5555，只是执行方式不一样；
	- 具体步骤
	- 1，`adb pull /system/build.prop C:\Users\Administrator\Desktop` 把build.prop文件导出到桌面
	- 2，以文本的方式打开build.prop文件
	- 3，在文件中加入service.adb.tcp.port=5555，保存
	- 4，`adb push C:\Users\Administrator\Desktop\build.prop /system/`
	- 注意：如果build.prop文件为只读，测也需要通过`mount -o remount rw /system`（重新挂载system目录为可读可写）
	- 重启生效
- 第三种方式：
	- 1，adb shell 进入Android系统命令
	- 2，获取root权限
	- 3，执行`adb shell su -c setprop service.adb.tcp.port 5555`
	- 4，如果执行3 没效果，执行` adb shell su 0 "setprop service.adb.tcp.port 5555"` 试一下
	- 关于挂载的问题：
	- 有些板子只需要执行
	- ```
	  adb root
	  adb remount  /system
	  ```
	- 即可完成挂载；
- 总结：
- 三种方式都是修改/system/build.prop文件，增加或者修改setprop service.adb.tcp.port属性值；关于build.prop的adb命令：
	- ```
	  ```   adb shell 
	  getprop  //列出所有配置属性值
	  getprop [key]  //取得对应的key的属性值
	  如果要修改属性的话，只需修改键值对的值（字典值）就可以了，如：setprop [key] [value] 设置指定key的属性值。