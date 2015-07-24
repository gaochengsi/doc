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


--------------------------------------------------------------------
* 如果需要更详细更全面的信息，请登陆  
	http://radxa.com  						官方网站  
	339567728         						QQ讨论群  
	http://cn.radxa.com/forum.php					中文论坛  
* 另外，本手册所使用的所有源码、固件、工具，都可以登陆以下地址下载  
	http://dl.radxa.com/                             	      国外服务器  
	http://pan.baidu.com/share/home?uk=3108273493#category/type=0	百度云  
* 手册内容经小编实际操作，均可正常使用，但因系统以及整理文档等原因，若出现错误，请谅解，并使用以下邮箱联系我们  
	kevin@radxa.com  

## Radxa团队  

### 2015年7月  
--------------------------------------------------------------------

