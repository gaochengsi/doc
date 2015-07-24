---
layout: post
title: Rock变砖了怎么办
category: 上手指南
tags: [砖机]
keywords: radxa
description: 
---

# 拯救Rock  

怎样就算是变成砖机？  

当你使用工具“RKBatch Tool”往rock中写入镜像文件时失败，并且提示信息“Prepare IDB Fail”（或者说使用其他工具显示类似“IDB download failed ”这样的信息），Radxa就是“砖机”。  

但是实际上，它也不是真的成了一块转头。我们在拯救机器的时候，首先确认没有SD卡插在卡槽中，不然bootloader在这个时候会被破坏。这样，我们就总能恢复rock。  

## 怎样恢复设备（in windows)     

* 关掉电源  
* 把nand的8P和9P短接（使用导电物体连接两个引脚），并且把板子用OTG-USB线连接上PC机。  
![image](http://radxa.com/mw/images/e/ee/7w6fu.png)  
* 在连接上电脑之后，就可以把短接的针拿掉。在RKBatch Tool的方框内，现在应该变成蓝色了（如果不是，则重试一次）  
* 点击“restore”按钮，然后载入一个update.img的完整镜像。  

## 提示  

* 你可以使用放大镜或者变焦相机来帮助你完成短接这个步骤  
* 针脚的计数开始位置是芯片上有个小圆圈的一侧  

## In linux  

同样的操作对板子，连接OTG-USB线之后，使用upgrade_tool工具，会显示类似下面的信息  

> sudo upgrade_tool  
   List of rockusb connected  
   No found rockusb,Rescan press <R>,Quit press :r  
   List of rockusb connected  
   DevNo=1	Vid=0x2207,Pid=0x310b,LocationID=21f	   **Maskrom**  
   Found 1 rockusb,Select input DevNo,Rescan press <R>,Quit press :1  
   Rockusb>uf /tmp/radxa-rock-alip-desktop.img  
   Loading firmware...  

当你看见Maskrom(更多时候显示‘Loader’），那就意味着你成功短接了Rock的引脚  


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
