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

