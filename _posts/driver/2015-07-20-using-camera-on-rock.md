---
layout: post
title: 使用rock上的无线设备  
category: 硬件操作
tags: 
keywords: radxa
description: 
---

# 使用rock上的无线设备  

Radxa Rock提供了多种方式去连接，有线和无线。完整版有wifi和蓝牙功能，但是lite版只有wifi功能。所有的这些模块都通过USB接口与CPU连接。  

## 使用Android下的无线设备  

android设备下的无限和蓝牙，只需要在设置中进行简单的设置即可使用。

## 使用linux下的无线设备  

#### wifi

* rock 完整版上面的wifi模块为RTL8723AU，rock lite上面的wifi模块为RTL8188ETV.你可以使用下面的命令查看模块的信息  
	`sudo apt-get install usbutils`  
	`lsusb`  

* 完整版会输出类似如下信息
	> root@radxa:~# lsusb  
   	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub  
   	Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub  
   	Bus 002 Device 002: ID 05e3:0608 Genesys Logic, Inc. USB-2.0 4-Port HUB  
   	Bus 002 Device 003: ID 0bda:0724 Realtek Semiconductor Corp.  

* lite会输出类似下面信息
	> Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub   
   	Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub   
  	Bus 002 Device 002: ID 05e3:0608 Genesys Logic, Inc. USB-2.0 4-Port HUB   
   	Bus 002 Device 003: ID 0bda:0179 Realtek Semiconductor Corp.   

0bda:0724 是rtl8723的ID, 0bda:0179 是rtl8188etv 的ID。  

##### 桌面启动wifi  

	> Go to Start menu -> Perference -> Network Connections -> wifi  
	choose the wifi network you want to join and click edit -> wifi security  
	input the wifi password and save. Then click the right corner to join your wifi network  

##### 手动连接WIFI管理  

	你可以使用nmlci（命令行方式的网络管理工具）去配置你的网络  

* List wireless networks  
	`nmcli dev wifi list`  

	> SSID                              BSSID               MODE             FREQ       RATE   		SIGNAL    SECURITY   ACTIVE  
	'MY-ACCESS-POINT'                 12:34:56:78:90:AB   Infrastructure   2412 MHz   44 MB/s    		100      WPA   WPA2   yes  
	'OTHER-ACCESS-POINT'              34:12:78:56:AB:90   Infrastructure   2416 MHz   54 MB/s    		70       WPA   WPA2   yes  

* 连接网络  
	`nmcli dev wifi connect MY-ACCESS-POINT password MY-PASSWORD`  

* 网络设备列表  
	`nmcli device`  
	> DEVICE     TYPE              STATE        
	wlan1      802-11-wireless   connected    
	eth0       802-3-ethernet    unavailable  

##### 手动连接配置你的无线路由  

* 使用lite连接你的无线路由器，你可以按照下面这样做  

   `sudo apt-get install wireless-tools`  
   `sudo apt-get install wpasupplicant`  
   `sudo ifconfig wlan0 up`  

* 如果你不能够正常启用wlan0，意味着驱动没有被安装。你可以按照下列步骤去安装驱动。不过这是在rock连接上以太网的情况下。如果没有，你需要从其他地方下载，并且移动到rock的储存上。  

   `sudo mkdir -p /lib/modules && cd /lib/modules`  
   `sudo wget http://dl.radxa.com/rock/images/ubuntu/partitions/modules_3.0.36+_14-04-12.tar.gz`  
   `sudo tar zxvf modules_3.0.36+_14-04-12.tar.gz`  
   `sudo rm modules_3.0.36+_14-04-12.tar.gz`  
   `sudo modprobe 8723au`  
现在，你可以用ifconfig看到wlan0了。  

* 你可以看到wifi在扫描附近的热点工作了。  
   `iwlist wlan0 scan`  

* 对于新的wifi驱动，iwlist会提示"wlan0 Interface doesn't support scanning",你需要去使用新的工具iw替代它  
   `iw dev wlan0 scan`  

* 我们回到连接路由器的步骤上，编辑/etc/wpa_supplicant.conf  

   > ctrl_interface=/var/run/wpa_supplicant  
   network={  
   	ssid="your ssid name"  
   	psk="your wireless password"  
   }  

* 然后运行下面的命令去连接路由器  
	`wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant.conf `  


* 运行下面的命令从路由得到IP  
	`dhclient wlan0`  

* 假如你想要rock在每次启动的时候自动连接到无线网络，添加以下的文字到/etc/network/interfaces  
	> auto wlan0  
 	  iface wlan0 inet dhcp  
  	 wpa-conf /etc/wpa_supplicant.conf  


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
