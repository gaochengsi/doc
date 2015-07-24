---
layout: post
title: Rock2 Square 介绍
category: Rock2
tags: [产品,rock2]
keywords: rock2 square
description: Rock2 Square 介绍
---

Rock2 square 基于Rochchip 最新的 RK3288 芯片,  关于RK3288芯片的介绍请参考 [RK3288百度百科][1]。
为了方便客户定制, 我们采用"核心板" + "底板"的方式。

SOM 核心板  (目前有两个版本)：
1)  2G RAM + 16G EMMC
2)  4G RAM + 32G EMMC

![此处输入图片的描述][2]

|指标 |Square Baseboard  | Full Baseboard|
|----- |-----:|---:|
|电源|5V/3A|5~12V/4A|
|存储|uSD up to 128GB, 2.5" hard drive up to 4TB *|uSD up to 128GB, 2.5"/3.5" HD up to 4TB|
|USB|1 x OTG, 3 x host|1 x OTG, 2 x host|
|网络|1000M|Dual 1000M **|
|显示|HDMI 2.0, eDP, LVDS***|HDMI 2.0, eDP, LVDS, MIPI|
|无线|Bluetooth 4.0(BLE support), 802.11 a/b/g/n/c wifi|Bluetooth 4.0(BLE support), 802.11 a/b/g/n/c wifi, HSPA/WCDMA, EDGE/GPRS/GSM
|
|音频|SPDIF, Headphone with line in, Mic|Headphone with line in, Mic|
|Misc|IR, debug UART, RTC|IR, debug UART, RTC|
|扩展口|50pin 2mm LVDS, 40pin 2.54mm for gpio, i2c, uart, spi|2 x 40pin 2.54mm header for gpio, i2c, uart, spi|

Base board 底板：

![此处输入图片的描述][3]

 
  [1]: http://baike.baidu.com/link?url=3_5NTBgwIDwgP1e0AudAXVFat7QSZqwDm1AaWGz9dgAM95ispDv28DcbTPAvLSctOMrdvzSeOcWXWE2WBhy-iq
  [2]: http://cn.radxa.com/data/attachment/forum/201504/09/120017cn80d661ozayjjnj.png
  [3]: http://cn.radxa.com/data/attachment/forum/201504/09/120110sor3e9ht8z8sjx8o.png
  
参考：
  
- http://radxa.com/Rock2
- http://cn.radxa.com/forum.php?mod=viewthread&tid=282



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


