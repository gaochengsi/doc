---
layout: page
title: ROS在Ubuntu系统下的安装
category: Linux
tags: [产品, 配置]
keywords: 产品, 配置
description: 
---


# ROS Jade在Rock2 square上的安装 

本文将介绍如何在Rock2 square的Ubuntu系统安装ROS系统，其他关于ROS的介绍和教程请登录http://wiki.ros.org   
	
不过首先你需要准备好一个安装好ubuntu系统的rock2 square，并且能连上网络。

## 配置环境  

* 设置系统的安装库来源  

	备份apt的源配置文件  
	`sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup`  
	
	查看该文件可以看到类似这么几行，如果没有，请加上它，并且保证它不是被注释的信息  

	> deb http://us.archive.ubuntu.com/ubuntu/ saucy universe restricted multiverse   
	  deb-src http://us.archive.ubuntu.com/ubuntu/ saucy universe  restricted multiverse  
	  deb http://us.archive.ubuntu.com/ubuntu/ saucy-updates universe  restricted multiverse  
	  deb-src http://us.archive.ubuntu.com/ubuntu/ saucy-updates universe restricted multiverse  

	重点在于后面三个条件 universe restricted multiverse，补全他们  

* 设置环境变量  

	`sudo update-locale LANG=C LANGUAGE=C LC_ALL=C LC_MESSAGES=POSIX`  

* 增加来源  

	`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'`  

* 设置锁  
 
	`wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -`  

* 更新设置  

	`sudo apt-get updata`  

## 安装ROS Jade  

* 基础安装  
	`sudo apt-get install ros-jade-ros-base`  
	
* 个性化安装  
	需要注意的是，上面的安装下载包括了许多基础组件，都不是图形界面的，需要一定下载时间，如	果你想要安装一些个人需要的组件，可以先使用下面的命令搜索所有的组件  
	`apt-cache search ros-jade`  

	然后选择你所需要的包名字，使用下列命令  
	`sudo apt-get install ros-jade-PACKAGE`  

	例如你看到一个包叫做ros-jade-navigation，则使用下列命令  
	`sudo apt-get install ros-jade-navigation`  

## 初始化ROS  

在使用ROS之前，你将需要安装和初始化rosdep。当你安装运行新的ROS组件时，这个工具能够	帮助你方便地建立依赖软件依赖关系。  

`sudo apt-get install python-rosdep`  
`sudo rosdep init`  
`rosdep update`  

## 软件环境变量  

`echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc`  
`source ~/.bashrc`  

## 获取rosinstall

rosinstall是经常被使用的命令行工具，它能让你使用一行命令就下载好组件，使用下面命令安装  

	sudo apt-get install python-rosinstall  

## 最后设置

最后我们还需要一个设置，因为ROS不能识别Linaro的系统，所以我们需要更改/etc/lsb-release文件中的系统信息，只需要把Linaro改成Ubuntu就好，其他不用变，类似下面这样子  

> DISTRIB_ID=Ubuntu  
  DISTRIB_RELEASE=14.04  
  DISTRIB_CODENAME=trusty  
  DISTRIB_DESCRIPTION="Ubuntu 14.04"  

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
