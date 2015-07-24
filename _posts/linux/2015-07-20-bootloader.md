---
layout: page
title: 关于bootloader和U-boot
category: 上手指南
tags: [启动,bootloader]
keywords: 产品, 配置
description:
---


# bootloader  

目前绝大多数的基于RK3188的设备，包括Radxa Rock，都是使用Rockchip自身的bootloader。不幸的是，Rockchip并不提供bootloader的源代码。  

少数RK3188的设备使用U-boot。  

bootloader的主要任务是  

* 导入并且校验parameter文件  
* 导入并且运行misc分区的命令  
* 导入，校验，启动内核，randisk，boot等各个分区,或者是recovery分区刷机  
* 连接PC主机，下载镜像、固件用以升级  

下载用于Radxa Rock的bootloader可以从这个地址  

http://dl.radxa.com/rock/images/loader/  

假如你感兴趣的话，你可以从这个地方查看一些别人对它的分析  

https://gist.github.com/sarg/5028505  

或者说是下面的国内的地址  

http://blog.csdn.net/faithsws/article/details/17245699  



# U-boot for rockchip  


## 在NAND上载入U-boot  

#### 编译U-boot  

* 首先你要确保你拥有编译链工具arm-eabi，如果没有，参考[这个](http://radxa.com/Rock/install_toolchain),并且保证环境变量“ARCH”、“CROSS——COMPILE”,的值分别为‘arm’、编译链工具的路经。  

* 获取源码  
	`git clone -b u-boot-rk3188 https://github.com/radxa/u-boot-rockchip.git`  

* 进入目录，开始编译（'xx'不需要更换）  
	`cd u-boot-rockchip`  
	`make rk30xx`  

编译结束后，我们会在文件当前目录下得到两个二进制文件，‘RK3188Loader_miniall.bin’ 还有 ‘uboot.img’.


#### 刷boot  

* 首先需要获取工具，并且了解一些工具的使用常识，你可以参考[这里](http://radxa.com/Rock/flash_the_image)  

* 使用擦除旧的程序  
	`sudo upgrade_tool ef RK3188Loader_miniall.bin`  

* 刷入新的loader  
	`sudo upgrade_tool ul RK3188Loader_miniall.bin`  

* 格式化NAND（只需要第一次这么做）  
	`sudo upgrade_tool lf`  

* 刷入paremeter  
	`upgrade_tool di -p parameter`  

	paramter file  

>  FIRMWARE_VER:4.2.2  
   MACHINE_MODEL:radxa_rock  
   MACHINE_ID:007  
   MANUFACTURER:RADXA  
   MAGIC: 0x5041524B  
   ATAG: 0x60000800  
   MACHINE: 3066    
   CHECK_MASK: 0x80  
   KERNEL_IMG: 0x60408000  
   CMDLINE:console=ttyFIQ0,115200 console=tty0 root=/dev/block/mtd/by-name/linuxroot rw rootfstype=ext4 init=/sbin/init mac_addr=de:ad:de:ad:be:ef initrd=0x62000000,0x00800000 mtdparts=rk29xxnand:0x00002000@0x00002000(uboot),0x00008000@0x00004000(boot),-@0x000c0000(linuxroot)  


* 输入U-boot  
	`sudo upgrade_tool wl 0x2000 uboot.img`  
	`sudo upgrade_tool rd`  

* 刷入内核+虚拟磁盘  
	`upgrade_tool di -b boot-linux.img`  

* 刷入文件系统  
	`upgrade_tool di linuxroot rootfs.img`  


## 载入U-boot到SD卡中  

#### 编译u-boot  

* 首先你要确保你拥有编译链工具arm-eabi，如果没有，参考[这个](http://radxa.com/Rock/install_toolchain),并且保证环境变量“ARCH”、“CROSS——COMPILE”,的值分别为‘arm’、编译链工具的路经。  

* 获取源码  
	`git clone -b u-boot-rk3188-sdcard https://github.com/radxa/u-boot-rockchip.git`  

* 进入目录，开始编译（'xx'不需要更换）  
	`cd u-boot-rockchip`  
	`make rk30xx`  

* 现在我们需要进行最后一个操作  
	`./pack-sd.sh`  

我们会得到另一个我们需要的二进制文件‘u-boot-sd.img'  

#### 导入SD卡中  

我们只需要执行以下命令就可以完成这个操作  
`sudo dd if=u-boot-sd.img of=/dev/sdx seek=64`  


## 操作失误导致“变砖”  

如果你在进行上述操作的时候，出现某些错误，导致设备无法启动了，你需要参考[如何解砖](http://radxa.com/Rock/unbrick)  

# 一些参考链接  

* http://androtab.info/rockchip/u-boot/ (working binaries)  

* https://github.com/linux-rockchip/u-boot-rockchip (some improvement)  


=
=


--------------------------------------------------------------------
* 如果需要更详细更全面的信息，请登陆  
	http://radxa.com  						官方网站  
	339567728         						QQ讨论群  
	http://cn.radxa.com/forum.php					中文论坛  
* 另外，本手册所使用的所有源码、固件、工具，都可以登陆以下地址下载  
	http://dl.radxa.com/                             	      国外服务器  
	http://pan.baidu.com/share/home?uk=3108273493#category/type=0	 百度云  
* 手册内容经小编实际操作，均可正常使用，但因系统以及整理文档等原因，若出现错误，请谅解，并使用以下邮箱联系我们  
	kevin@radxa.com  

## Radxa团队  

### 2015年7月  
--------------------------------------------------------------------
