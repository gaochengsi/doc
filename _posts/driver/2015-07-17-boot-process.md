---
layout: page
title: nand、sdcard双启动
category: 硬件操作
tags: [双启动]
keywords: 产品, 配置
description:
---


# 启动流程  

## NAND上面的双启动  

* 在NAND flash上用于双启动的boot镜像可以从下面的网址得到：  
   http://dl.radxa.com/rock/images/dual_boot/  

* 下载之后，按照 http://radxa.com/Rock/flash_the_image 烧写。 

## 在SD卡、U盘/移动硬盘上的双启动  

如果你想在SD卡、U盘/移动硬盘上进行双启动，那么按照下面的步骤：

* 把整个Nand给Android使用  

* 使用SD卡/U盘/移动硬盘安装Lubuntu  

* 双启动
下面是存档的步骤：

从 http://dl.radxa.com/rock/images/android/ 下载纯净的android，并烧写  
从SD卡或者 U盘或者移动硬盘 下载parameter文件，并烧写到 “parameter”分区。  
从 http://dl.radxa.com/rock/images/ubuntu/partitions/ 下载最新的boot-linux.img，并烧写到 “recovery”分区。  
从 http://dl.radxa.com/rock/images/ubuntu/partitions/ 下载最新的rootfs，烧写到SD卡/U盘/移动硬盘的“first partition”  
从 http://dl.radxa.com/rock/images/android/BootUbuntu.apk 下载并安装用于从Android引导到linux的应用程序。  




- 


