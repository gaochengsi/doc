---
layout: page
title: 使用ADB调试android Rock
category: Android
tags: [android,adb]
keywords: 产品, 配置
description:
---

# 使用ADB调试android Rock  

我们首先需要
* 运行android平台的rock  
* PC机  
* USB转OTG线  

## Linux  

* 我们首先要安装adb工具  
	`sudo apt-get install android-tools-adb`  

* 然后修改udev规则文件，填入以下内容  
	`sudo gedit /etc/udev/rules.d/51-android.rules`  
	> SUBSYSTEM=="usb", ATTR{idVendor}=="2207", MODE="0666", GROUP="plugdev"  

* 现在我们需要让之前的设置马上生效  
	`sudo udevadm control --reload-rules`  
	`sudo udevadm trigger`  

* 如果.android文件夹没有存在你的home目录下的话，我们需要创建它，然后在里面添加一个文件  
	`mkidr -p ~/.android`  
	`echo 0x2207 >> ~/.android/adb_usb.ini`  

* 然后确认你的android设备开启了debugging的选项  

* 重启一次adb工具  
	`sudo adb kill-server`  
	`sudo adb start-server`  

* 现在，我们使用以下命令就可以看到类似这样的一些连接信息  
	`sudo adb devices`  
	> 16EM8TBH0Z  device

连接成功 ！ 

## Windows  

### 安装adb usb驱动  

有两种方法做这件工作  

##### 1.使用工具RK_DriverAssitant tools安装  

此时不需要连上rockchip的设备。直接下载[RK_DriverAssitant](RK_DriverAssitant)  

然后解压、打开.exe格式的文件  

如下图，点击install Driver即可安装。如果你不能确定你之前是否曾经安装过，则先点击Uninstall Driver,再安装  
![u](http://radxa.com/mw/images/b/ba/RK_Driver_Assistant_Install_Uninstall.jpg)  
	
接着如下图所示，点击Install。由于Windows的版本不同，可能需要你进行两次这个操作。  
![image](http://radxa.com/mw/images/e/e6/RK_Driver_Assistant_Windows_Security.jpg)  
	
然后可以启动你的设备了。
	
**Windows XP在操作上有一些不一样，你需要参考压缩包中的readme，然后进行操作。**  
	
##### 2.手动安装驱动  

下载驱动（http://dl.radxa.com/rock/tools/windows/radxa_adb_driver.zip ），然后解压开。  

启动你的rock，并且连接上电脑  

当系统提示“找到新设备”的时候，根据提示找到驱动位置，系统会完成安装  
	
### 安装android SDK  

* 在Windows下安装adb，通常的做法是安装有google公司提供的android SDK。而SDK的管理需要JDK的支持。所以下载[JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html),并把它安装好。  

* 然后下载[SDK](http://dl.google.com/android/android-sdk_r22.3-windows.zip),并把它解压到C盘下，像这样 C：\android-sdk-windows。  

* 安装SDK时，首先选“Android SDK tools" ,然后是”Android sdk platform",接着是“install packages"，sdk就会被下载，被安装到"C：\android-sdk-windows\platform-tools"  

### 设置环境  

* 右键”我的电脑“  

### 设置USB ID  

* 打开windows的终端cmd，输入以下命令  
	`cd c:\android-sdk-windows`    **此处是你的安装目录**  
	`mkdir .android`  

* 进入我们刚刚创建的文件夹下，创建一个新的文件，名字为“adb_usb.ini",然后打开它，在里面输入以下内容，然后保存  
	> 0x2207  

* 然后开始测试adb  
	`cd c:\android-sdk-windows\platform-tools`  
	`adb kill-server`  
	`adb devices`  

	然后你应该会看到类似下面的信息  
	> List of devices attached  
	  16EM8P26M8      device  
	
完成！





 
