---
layout: page
title: ADB连接不上怎么办
category: Android
tags: [android,adb]
keywords: 产品, 配置
description:
---



很多人遇到这样的情况,android 起来之后,连接上数据线, 然后在命令提示符下运行adb命令 adb shell,adb kill-server. adb devices. 但是发现 device not found 或者 devices list 为空。 这种情况如何解决。  


1、首先要确认是否正确安装了 官方提供的adb驱动。虽然官方提供的驱动是基于google提供的驱动改的，但是有一些针对自己产品的改动。请确认是使用官方驱动。下载地址：http://dl.radxa.com/rock/tools/windows/radxa_adb_driver.zip  

2、使用官方提供的adb工具，下载地址:http://dl.radxa.com/rock/tools/windows/adb_tools.zip。你可以到这个目录下执行adb命令。如果不常用rock调试，不建议把这个adb 目录放到环境变量里面，防止与其他android设备冲突。  

3、如果1，2 步完成后，执行adb命令没有反应。看是否自己的PC的USB接口是否是USB3.0， 目前rock 对USB3.0兼容性不好，请用USB2.0 连接。如果还是不行，重启（只限于第一次安装驱动的时候重启，之后不需要）后再尝试  

4、前3步确认没有问题，依然无法连接，执行adb kill-server，或者调出任务管理器 干掉所有的adb进程。再尝试。  

5、前4个步骤确认后还存在问题，更换USB数据线后继续尝试。  

一般来说。 是adb驱动和adb工具使用不对造成的。

很多人说可以使用手机助手来解决这个问题，但是发现adb连接时好时坏，或者一旦退出手机助手 adb用不了等问题。

这些基本都是由于adb驱动或者工具冲突导致的。

按照上面的步骤一步步执行。应该是没有问题的。

如果还不行，请提出。





-------------------------------------------------------------------
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
