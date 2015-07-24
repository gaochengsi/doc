---
layout: page
title: android开发的一些小技巧
category: Android
tags: [android]
keywords: 产品, 配置
description:
---

# android开发小技巧  

本文将介绍一些rock android开发的小技巧。  

### 备份android的APP应用  

有时我们需要频繁的更换系统固件，但是安装在系统上的app又是必要的，待会又要重新安装。这时候你可以使用adb工具来节省一点时间。

* 首先，你需要先安装adb。如果你之前没安装，可以参考http://radxa.com/Rock/linux_adb（linux).  
	或者http://radxa.com/Rock/windows_adb（windows).  

* 然后连接上设备后，使用如下命令备份  
	`adb backup -apk -shared -all`  

* 当回复备份的时候，则使用以下命令  
	`adb restore backup.ab`  

### adb主机中，对android设备进行截图  

只需要执行以下命令就可以  
	`adb shell  "cat /dev/graphics/fb0 > /sdcard/fb.raw"`  
	`adb pull /sdcard/fb.raw`  
	`ffmpeg -vcodec rawvideo -f rawvideo -pix_fmt rgb32 -s 1920x1080  -i fb.raw -f image2 -vcodec png fb-%d.png`  





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
