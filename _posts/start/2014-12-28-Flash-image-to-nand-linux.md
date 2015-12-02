---
layout: post
title: 将image烧写到NAND（Linux）
category: 上手指南
tags: [入门, linux, image, NAND]
keywords: image,NAND
description: 
---



从Android 4.4起，Rockchip提供了一个在Linux系统下的升级工具（闭源）。叫做“upgrade_tool”。 它支持烧写update.img、parameter、bootloader和其他分区，注意它是基于命令行的工具。 


* 下载工具  

从[这里][2]下载Rockchip的Linux升级工具并解压，然后你会得到一个在Linux下二进制可执行文件，叫做“upgrade_tool”。 

* 让板子进入[Loader模式][1]，并运行命令烧写升级包updata.img

`sudo ./upgrade_tool uf  /path/to/update.img`      #(UF Upgrade Flash)
	
* 如果你在升级过程中有一个错误，你可以首先使用以下命令来低格nandflash

`sudo ./upgrade_tool lf  #(LF Lowlevel Format)`		# will erase everything on nand

* 烧写 parameter

`sudo ./upgrade_tool di -p /path/to/parameter`    #(DI Download Image)

* 烧写 bootloader

`sudo ./upgrade_tool ul /path/to/bootloader`	  #(UL Upgrade Loader)

* 烧写内核(boot.img)

`sudo ./upgrade_tool di -b /path/to/boot.img`	    #(DI Download Image)


其他
有一些在upgrade_tool中使用的其他命令，比如：   
   UL: 升级Loader  
   EF: 擦除nand flash  


[1]:/2014/12/31/enter-recovery-mode.html  

[2]:http://dl.radxa.com/rock/tools/linux/Linux_Upgrade_Tool_v1.21.zip

