---
layout: post
title: 从SD卡中向NAND引导镜像  
category: 上手指南
tags: [镜像，SD卡]
keywords: radxa
description: 
---

# 从SD卡中向NAND引导镜像  

**Rockchip提供了在windows下完成这一任务的工具，但是目前只支持用于烧录Android的镜像。Linux下需要做更多的工作**  


## WINDOWS  
### 准备设备  

* 一张至少4G的SD卡  
* 一个SD读卡器  
* windows系统的PC  

### 先去下载一个升级的镜像文件  

记住，是android的镜像，你可以从这里下载http://radxa.com  

### 制作一个用于升级的SD卡  

* 下载工具，并借解压它http://dl.radxa.com/rock/tools/windows/SD_Firmware_Tool.zip  
* SD卡插入读卡器，读卡器连接到电脑上  
* 在广告下载的工具文件夹中，寻找一个“SD_Firewware_Tool.exe"的文件，然后打开它  
* 在”choose removable disk"项中，选择你广告插入的SD卡  
* 勾选"upgrade Fireware"  
* 选择你想要烧录的镜像文件  
* 最后点击”create“按钮。当完成时，会有弹窗提示你。
![image](http://radxa.com/mw/images/b/bd/Upgrade_disk_1.jpg)  

### 用SD卡升级Rock  

* 把rock关机  
* 插入SD卡  
* 启动rock，接着程序运行，你会看见如下的界面  
	![image](http://radxa.com/mw/images/0/0d/Upgrade_disk_2.jpg)  
* 等待初始华结束，这个过程也许会持续几分钟，当升级成功后，你会看见下面的界面  
	![image](http://radxa.com/mw/images/7/73/Upgrade_disk_3.jpg)  
* 关闭rock，移除SD卡  
* 启动rock，现在系统就是新载入的版本了  


## Linux  

* 在Linux操作环境下，你先要往SD卡中载入一个sdboot-header.img，下载地址 http://files.androtab.info/radxa/sdboot.tgz 。  

* 将你的SD卡连接PC，划出一块分区，下列命令中的X，需要读者根据自己SD卡插入后，在/dev/目录下生成的名字替换  
`dd if=sdboot-header.img of=/dev/sdX bs=1M conv=fsync`  
* 为分区设置文件格式  
`mkfs.vfat /dev/sdX1`  
* 挂载该分区  
`mount /dev/sdX1 /mnt`  
* 将我们刚刚下载压缩包内的文件复制进分区中  
`cp rksdfw.tag sd_boot_config.config /mnt/`  
* 复制你想要升级的镜像文件到该分区,并且名字命令为“sdupdate.img"  
`cp path/to/radxa_update.img /mnt/sdupdate.img`  
* 取消挂载  
`umount /mnt`  

然后将SD卡插入Rock中，启动即可。  

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
