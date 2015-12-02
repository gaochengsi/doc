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

	adb shell  "cat /dev/graphics/fb0 > /sdcard/fb.raw"`  
	adb pull /sdcard/fb.raw`  
	ffmpeg -vcodec rawvideo -f rawvideo -pix_fmt rgb32 -s 1920x1080  -i fb.raw -f image2 -vcodec png fb-%d.png 





- 
