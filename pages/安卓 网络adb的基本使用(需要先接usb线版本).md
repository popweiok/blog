- **一般情况下Android系统开发调试的时候，都是使用USB接口的adb工具进行调试，方便简单，除了能使用Mico USB进行数据流传输以外，还能使用网络进行adb调试，本文就介绍adb网络调试。**
- **网络adb调试开启步骤**
	- 1、把Android平板或者手机WiFi连接到跟PC机子同一个网段的网络，在设置-系统-关于-状态 下面查看设备IP,然后查看PC是否可以ping通手机的设备的IP。
	- 2、先通过手机充电线（USB线）连接平板，确保adb可以正常使用。
	- 3、开启tcp/ip调试端口 ，5555为网络 adb 的默认端口。
		- `adb tcpip 5555`
	- 4、使用网络进行adb调试。
		- `adb connect <Android设备IP地址>`
	- 5、进入到Android设备shell
		- `adb shell`
- **网络转Mico USB数据线**
	- ```
	  //断开网络adb调试
	  # adb disconnect
	   
	  //开启Mico USB调试
	  # adb usb
	   
	  //进入到Androdi终端设备的shell
	  # adb shell
	  ```
- [[安卓adb网络连接调试 重启之后失效]]
-