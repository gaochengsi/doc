---
layout: post
title: 固件命名介绍
category: 上手指南
tags: [入门, Flash, image, NAND]
keywords: image,NAND
description: 
---

## update.img

update.img是一个包含了所有需要部分的升级镜像。也就是说，只要导入这一个镜像文件，就可以达到正常启动系统的目的，当然，这个名字一般用在当你自己编译固件的时候由编译文件自动生成，如果你是从某个地方下载固件的话，一般会有特殊的命名方式。你可以下载完整的update.img在这里http://dl.radxa.com  

## 如何去下载正确的镜像  

* 首先确认你的板子是什么版本，可以参考这个（http://radxa.com/Rock/hardware_revision）  

* 所有的镜像都可以从这里下载http://dl.radxa.com/  

* 不一样的板子，你需要到各自的文件夹下寻找镜像。例如，你的板子是rock pro，那么你应该进入类似rock_pro/images/...  

* 镜像的命名是有规律的，你可以通过分析文件名判断是否是你所需要的镜像。例如：  

> 	radxa_rock_android_kitkat_r2_140911_sdcard.zip  
	radxa_rock  : 硬件的名字  
	android   ：  固件的OS  
	kitkat   ：固件所特有的别名，android4.4  
	r2  	： 发布版本  
	140911	：发布日期  
	sdcard	: 说明镜像是准备用来从SD卡启动的，没有这个名字，则是从nand。  

**注意：不一样版本的硬件，不能使用同一份镜像文件，否则会导致不能启动或者没有视频输出**  



## 创建镜像  

下面会有一些关于Radxa Rock固件组成的介绍，如果你不感兴趣，或者看不太明白，没关系，可以直接去下面两个链接，学习编译固件的步骤

[Linux固件编译][1]  

[android固件编译][2]


#### Linux固件    

一个完整可用的Rock固件，需要包含这几个分区，它们可以在打包固件时的package-file文件中查看  

* bootloader		
启动引导程序，Rockchip提供了闭源的多个版本  
* boot.img		
这个是由android定义的格式，其中包含了Linux的内核，还有虚拟磁盘的程序  
* pagameter		
这个文件的作用在于区分各个分区的位置，我们在使用瑞芯微提供的upgrade_tool工具时，就是根据这个文件的提供的地址去更新擦写分区  
* rootfs.img		
文件系统，他也是一个固件占用最大的部分，我们所使用的系统，能被看到的文件，都在这个里面  



#### Android固件  

Android是一个基于Linux内核的系统，所以相比于Linux固件，Android会多出几个特有的部分

* recovery		
由android定义的格式，包含了Linux内核，加上一个用来恢复系统的程序  
* misc			
这个分区保存了一些硬件相关的数据，很重要  
* data			
用户数据区，程序就是安装在这  
* cache			
用户缓存区  
* system		
系统的文件存放在这，如果损坏，可以进入recovery分区，重新引导新的固件  


[1]:/2015/07/17/build-linux-image.html
[2]:/2015/07/17/build-android-source-code.html
