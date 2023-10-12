- 查看每个USB设备及其配置描述
- ```
  127|root@rk3288_box:/proc # cat /sys/kernel/debug/usb/devices
  
  T:  Bus=03 Lev=00 Prnt=00 Port=00 Cnt=00 Dev#=  1 Spd=480  MxCh= 1
  B:  Alloc=  0/800 us ( 0%), #Int=  0, #Iso=  0
  D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=01 MxPS=64 #Cfgs=  1
  P:  Vendor=1d6b ProdID=0002 Rev= 3.10
  S:  Manufacturer=Linux 3.10.0 dwc_otg_hcd
  S:  Product=DWC OTG Controller
  S:  SerialNumber=ff540000.usb
  C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=  0mA
  I:* If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
  E:  Ad=81(I) Atr=03(Int.) MxPS=   4 Ivl=256ms
  
  T:  Bus=02 Lev=00 Prnt=00 Port=00 Cnt=00 Dev#=  1 Spd=480  MxCh= 1
  B:  Alloc=  0/800 us ( 0%), #Int=  0, #Iso=  0
  D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=01 MxPS=64 #Cfgs=  1
  P:  Vendor=1d6b ProdID=0002 Rev= 3.10
  S:  Manufacturer=Linux 3.10.0 dwc_otg_hcd
  S:  Product=DWC OTG Controller
  S:  SerialNumber=ff580000.usb
  C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=  0mA
  I:* If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
  E:  Ad=81(I) Atr=03(Int.) MxPS=   4 Ivl=256ms
  
  // 第一层 EHCI 控制器
  T:  Bus=01 Lev=00 Prnt=00 Port=00 Cnt=00 Dev#=  1 Spd=480  MxCh= 1
  B:  Alloc=  3/800 us ( 0%), #Int=  4, #Iso=  0
  D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=00 MxPS=64 #Cfgs=  1
  P:  Vendor=1d6b ProdID=0002 Rev= 3.10
  S:  Manufacturer=Linux 3.10.0 ehci_hcd
  S:  Product=EHCI Host Controller
  S:  SerialNumber=ff500000.usb
  C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=  0mA
  I:* If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
  E:  Ad=81(I) Atr=03(Int.) MxPS=   4 Ivl=256ms
  
   //第二层 HUB               
  T:  Bus=01 Lev=01 Prnt=01 Port=00 Cnt=01 Dev#=  2 Spd=480  MxCh= 4
  D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=02 MxPS=64 #Cfgs=  1
  P:  Vendor=05e3 ProdID=0610 Rev=32.98
  S:  Product=USB2.0 Hub
  C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=100mA
  I:  If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=01 Driver=hub
  E:  Ad=81(I) Atr=03(Int.) MxPS=   1 Ivl=256ms
  I:* If#= 0 Alt= 1 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=02 Driver=hub
  E:  Ad=81(I) Atr=03(Int.) MxPS=   1 Ivl=256ms
  
  //第三层 连接到HUB上的设备
  T:  Bus=01 Lev=02 Prnt=02 Port=00 Cnt=01 Dev#=  3 Spd=12   MxCh= 0
  D:  Ver= 1.10 Cls=00(>ifc ) Sub=00 Prot=00 MxPS= 8 #Cfgs=  1
  P:  Vendor=248a ProdID=8366 Rev= 1.00
  S:  Manufacturer=Telink
  S:  Product=Wireless Receiver
  C:* #Ifs= 1 Cfg#= 1 Atr=a0 MxPwr= 50mA
  I:* If#= 0 Alt= 0 #EPs= 1 Cls=03(HID  ) Sub=01 Prot=02 Driver=usbhid
  E:  Ad=82(I) Atr=03(Int.) MxPS=   8 Ivl=4ms
  
  T:  Bus=01 Lev=02 Prnt=02 Port=01 Cnt=02 Dev#=  4 Spd=480  MxCh= 4
  D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=01 MxPS=64 #Cfgs=  1
  P:  Vendor=1a40 ProdID=0101 Rev= 1.11
  S:  Product=USB 2.0 Hub
  C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=100mA
  I:* If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
  E:  Ad=81(I) Atr=03(Int.) MxPS=   1 Ivl=256ms
  
  //usb鼠标
  T:  Bus=01 Lev=03 Prnt=04 Port=00 Cnt=01 Dev#=  5 Spd=12   MxCh= 0
  D:  Ver= 1.10 Cls=00(>ifc ) Sub=00 Prot=00 MxPS=64 #Cfgs=  1
  P:  Vendor=067b ProdID=2303 Rev= 3.00
  S:  Manufacturer=Prolific Technology Inc.
  S:  Product=USB-Serial Controller
  C:* #Ifs= 1 Cfg#= 1 Atr=80 MxPwr=100mA
  I:* If#= 0 Alt= 0 #EPs= 3 Cls=ff(vend.) Sub=00 Prot=00 Driver=(none)
  E:  Ad=81(I) Atr=03(Int.) MxPS=  10 Ivl=1ms
  E:  Ad=02(O) Atr=02(Bulk) MxPS=  64 Ivl=0ms
  E:  Ad=83(I) Atr=02(Bulk) MxPS=  64 Ivl=0ms
  
  //全时USB摄像头
  T:  Bus=01 Lev=03 Prnt=04 Port=02 Cnt=02 Dev#=  6 Spd=480  MxCh= 0
  D:  Ver= 2.00 Cls=ef(misc ) Sub=02 Prot=01 MxPS=64 #Cfgs=  1
  P:  Vendor=eba4 ProdID=7585 Rev= 1.50
  S:  Manufacturer=FBS-SYSTEM
  S:  Product=USB2.0 Camera
  S:  SerialNumber=35060591
  C:* #Ifs= 3 Cfg#= 1 Atr=80 MxPwr= 32mA
  A:  FirstIf#= 0 IfCount= 2 Cls=0e(video) Sub=03 Prot=00
  I:* If#= 0 Alt= 0 #EPs= 1 Cls=0e(video) Sub=01 Prot=00 Driver=uvcvideo
  E:  Ad=81(I) Atr=03(Int.) MxPS= 128 Ivl=32ms
  I:* If#= 1 Alt= 0 #EPs= 0 Cls=0e(video) Sub=02 Prot=00 Driver=uvcvideo
  I:  If#= 1 Alt= 1 #EPs= 1 Cls=0e(video) Sub=02 Prot=00 Driver=uvcvideo
  E:  Ad=82(I) Atr=01(Isoc) MxPS=3036 Ivl=125us
  I:* If#= 4 Alt= 0 #EPs= 1 Cls=03(HID  ) Sub=00 Prot=00 Driver=(none)
  E:  Ad=83(I) Atr=03(Int.) MxPS=  64 Ivl=64ms
  
  //Jabra SPEAK 410 USB麦克风 
  T:  Bus=01 Lev=02 Prnt=02 Port=02 Cnt=03 Dev#=  7 Spd=12   MxCh= 0
  D:  Ver= 2.00 Cls=00(>ifc ) Sub=00 Prot=00 MxPS=64 #Cfgs=  1
  P:  Vendor=0b0e ProdID=0412 Rev= 1.09
  S:  Product=Jabra SPEAK 410 USB
  S:  SerialNumber=1C48F9EBA9A1x010900
  C:* #Ifs= 4 Cfg#= 1 Atr=80 MxPwr=500mA
  I:* If#= 0 Alt= 0 #EPs= 0 Cls=01(audio) Sub=01 Prot=00 Driver=snd-usb-audio
  I:* If#= 1 Alt= 0 #EPs= 0 Cls=01(audio) Sub=02 Prot=00 Driver=snd-usb-audio
  I:  If#= 1 Alt= 1 #EPs= 1 Cls=01(audio) Sub=02 Prot=00 Driver=snd-usb-audio
  E:  Ad=03(O) Atr=01(Isoc) MxPS= 192 Ivl=1ms
  I:* If#= 2 Alt= 0 #EPs= 0 Cls=01(audio) Sub=02 Prot=00 Driver=snd-usb-audio
  I:  If#= 2 Alt= 1 #EPs= 1 Cls=01(audio) Sub=02 Prot=00 Driver=snd-usb-audio
  E:  Ad=83(I) Atr=01(Isoc) MxPS=  32 Ivl=1ms
  I:* If#= 3 Alt= 0 #EPs= 1 Cls=03(HID  ) Sub=00 Prot=00 Driver=usbhid
  E:  Ad=81(I) Atr=03(Int.) MxPS=  16 Ivl=1ms
  
  ```
- cat /proc/bus/input/devices ：查看连接的输入设备信息
- cat sys/bus/usb/devices/2-1.1/ ：查看对应USB设备的详细信息，例如 2-1.1:1.0 命名规则是：roothub-port:configuration.interface.）
- cat /sys/bus/usb/devices/2-1.1\:1.0/bInterfaceClass ：查看当前设备所支持的特性，例如： 01 表示支持audio
- ls /sys/bus/usb/drivers/usb/