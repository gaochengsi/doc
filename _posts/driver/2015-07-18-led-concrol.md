---
layout: post
title: 控制Radxa Rock上的LED灯  
category: 硬件操作
tags: [产品]
keywords: radxa
description: 
---

# 控制Radxa Rock上的LED灯  

Radxa Rock上有三个LED灯  
	* LED   --   GPIOref  --  GPIO number  
	* 绿灯  --  GPIO0_B4  --  172  
	* 蓝灯  --  GPIO0_B6  --  174  
	* 红灯  --  GPIO0_B7  --  175  

我们可以控制他们通过/sys/class/leds下的文件  

比如如下操作  
* 关闭红灯 `echo none > /sys/class/leds/red/trigger`   
* 常亮红灯 `echo default-on > /sys/class/leds/red/trigger`  
* 闪烁绿灯 `echo timer > /sys/class/leds/green/trigger`  
* 心跳蓝灯 `echo heartbeat > /sys/class/leds/blue/trigger`  

你还可以常看tgigger文件，了解其他对LED灯的工作方式  

`cat /sys/class/leds/red/trigger`  


=

=

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
