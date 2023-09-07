# ESXi_on_RPi
记录在树莓派上安装EXSi的学习成果
参考视频：
1. https://youtu.be/6aLyZisehCU
2. https://youtu.be/Ui5GHnDv3rU
## 硬件
- 树莓派
- 电源
- micro HDMI线
- micro SD卡
- 读卡器
- U盘（用于系统安装）
- USB存储介质（存储池）
- 鼠标、键盘
## 更新树莓派EEPROM
1. 下载并安装Raspberry Pi Imager：https://www.raspberrypi.com/software/
2. SD卡刷入Raspberry Pi OS Lite (32-bit)  
3. 开机，联网（有线或者无线）
4. `sudo rpi-eeprom-update`或者`sudo rpi-eeprom-update -a`
5. `sudo reboot`
## 准备SD卡
1. Raspberry Pi Imager，ERASE 擦除SD卡  
2. 下载并解压最新的 Raspberry Pi 固件（遇到重复选择跳过）：https://codeload.github.com/raspberrypi/firmware/zip/refs/heads/master
3. 下载并解压 UEFI Raspberry Pi 固件（遇到重复选择跳过）：https://github.com/pftf/RPi4/releases
4. 把`firmware-master/boot`目录下的所有文件复制到SD卡，并删除所有`kernel`开头的4个文件
5. 把`RPi4_UEFI_Firmware`目录下的所有文件复制到SD卡，选择替换所有文件
6. `config.txt`最后一行添加`gpu_mem=16`，保存文件（8GB版本跳过此条）
7. 把SD卡弹出插回树莓派
## 创建ESXi安装盘
1. 下载VMWare ESXi Arm ISO：https://flings.vmware.com/esxi-arm-edition
2. 下载并安装RuFUS：https://rufus.ie/zh/
3. 打开RuFUS，将ISO刷入U盘，`Partition scheme`为`GPT`，`Target system`为`UEFI`，`File system`为`FAT32`，`Cluster Size`为`32 kilobytes` 
4. U盘插入树莓派
## 安装ESXi
1. 插入鼠标键盘
2. 开机，狂按`esc`进入UEFI
3. `Device Manager/Raspberry Pi Configuration/Advanced Configuration`解除RAM限制，`F10`保存，`Y`确认，退出到主菜单，选`Continue`
4. `esc`进入UEFI
5. `Boot Manager`选择U盘
6. 按`Shift + O`，输入` autoPartitionOSDataSize=8192`，回车
7. 按照流程安装
8. 插入存储介质，`F5`刷新显示存储介质，选择存储盘作为系统盘进行安装
9. 回车重启，拔出安装系统用U盘
10. 按`esc`进入UEFI，`Boot Maintenance Manager/Boot Options/Change Boot Order`，将系统盘移动到顶部，保存，`Continue`
11. 根据IP地址配置ESXi
