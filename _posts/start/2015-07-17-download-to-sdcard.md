---
layout: post
title: 往SD卡中载入启动镜像
category: 上手指南
tags: [产品]
keywords: radxa
description: 
---

# 烧录ROCK SD卡启动镜像  

首先要下载或者制作一个从SD卡启动的镜像，下载可以从(http://dl.radxa.com/)  
制作可以参考本github中的其他文档，或者访问Radxa的官网http://radxa.com  
**（注意：rock lite从SD卡启动的android镜像只能由Windows平台烧录）**  

## Windows下操作  

开始之前，你需要准备一张至少8GB的高质量SD卡，劣质SD卡会导致启动失败（追求质量一向是IT从业者优先考虑的事物）  

* 下载[Win32DiskImager](http://sourceforge.net/projects/win32diskimager/files/latest/download)  
* 使用工具将镜像写入SD卡中  
![image](http://radxa.com/mw/images/3/38/Win32DiskImager.png)  

## Linux  

* 我们使用dd命令将镜像写入SD卡中  
  `sudo dd if=radxa_rock_xxxx_sdcard.img of=/dev/sdx`  

* 需要注意的是，sdx是SD卡插入PC后，PC自动命名的，所以我们在SD插入前，以及插入后，执行  
  `ls /dev/sd*`  
  通过对比得到SD卡的命名，如果出现sdb,sdb1,sdb2等类似名字，使用sdb。  

* 有时，我们只需要烧录某个分区，并且不想为烧录整个镜像而浪费太多时间，则需要查看该镜像打包时的patamater文件，上面会记录分区的地址。如boot分区   
*{
	CMDLINE:console=ttyFIQ0,115200 console=tty0 root=/dev/block/mtd/by-name/linuxroot rw 	rootfstype=ext4 init=/sbin/init mac_addr=de:ad:de:ad:be:ef initrd=0x62000000,0x00800000 	mtdparts=rk29xxnand:0x00008000@0x00002000(boot),-@0x0000A000(linuxroot)
}*

* 在使用dd命令的时候，则需要加上一个偏移量+0x2000  
  `sudo dd if=booting.img of=/dev/sdx  seek=$(0x2000+0x2000)`  





--------------------------------------------------------------------
* 如果需要更详细更全面的信息，请登陆  
	http://radxa.com  						官方网站  
	339567728         						QQ讨论群  
	http://cn.radxa.com/forum.php					中文论坛  
* 另外，本手册所使用的所有源码、固件、工具，都可以登陆以下地址下载  
	http://dl.radxa.com/                             	      国外服务器  
	http://pan.baidu.com/share/home?uk=3108273493#category/type=0    百度云  
* 手册内容经小编实际操作，均可正常使用，但因系统以及整理文档等原因，若出现错误，请谅解，并使用以下邮箱联系我们  
	kevin@radxa.com  

## Radxa团队  

### 2015年7月  
--------------------------------------------------------------------
