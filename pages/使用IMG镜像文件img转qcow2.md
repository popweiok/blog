- ```clojure
  chmod +x img2kvm
   ./img2kvm LEDE.img 101 vm-101-disk-1 local-lvm
  #上面2个101是虚拟机的VM ID， LEDE.img是需要引导的img文件，local-lvm是你的存储ID
  
  qemu-img convert -f raw -O qcow2 <firmware_name> <output_diskname>
  #例子：qemu-img convert -f raw -O qcow2  LEDE.img vm-101-disk-1.qcow2
  qm importdisk <vmid> <source> <storage>
  #例子：qm importdisk 101 vm-101-disk-1.qcow2 local-lvm
  ```