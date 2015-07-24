---
layout: page
title: 完善ubuntu的文件系统
category: Linux
tags: [linux,ubuntu]
keywords: 产品, 配置
description: 
---

# 完善ubuntu的文件系统

我们之前在Linux固件制作的文档中，已经有制作了一个Ubuntu的文件镜像，但是那个文件镜像是“别人的”，现在我们就来说说如何制作你想要的、更是你需要的文件系统。  

# 准备工作  

* 安装一个工具包  
	`sudo apt-get install qemu-user-static binfmt-support`  

* 然后如之前的操作一样，制作一个空的文件系统，并且挂载它到/mnt下  
	`dd if=/dev/zero of=rock_rootfs.img bs=1M count=1024`  
	`mkfs.ext4 -F -L linuxroot rock_rootfs.img `  
	`sudo mount -o loop rock_rootfs.img /mnt`  

* 接着下载一个[文件系统](https://releases.linaro.org)，然后“塞”进去。**下面的包名字和路经是随机的，大家根据自己的路经，视情况而进行替换**   
	`sudo tar zxvf linaro-raring-alip-20130826-474.tar.gz -C /mnt`  
	`cd /mnt`  
	`sudo mv binary/*`  
	`sudo rmdir binary`  

* 拷贝我们之前编译内核时，编译好的内核模块  
	`sudo mkdir -p /mnt/lib/modules`  
	`sudo cp -r /modules/lib/modules/3.0.36+ /mnt/lib/modules`  

* 准备chroot  
	`sudo cp /usr/bin/qemu-arm-static /mnt/usr/bin`  
	`sudo modprobe binfmt_misc`  
	`sudo mount -t devpts devpts /mnt/dev/pts`  
	`sudo mount -t proc proc /mnt/proc`  

* chroot进入文件系统  
	`sudo chroot /mnt`  

现在我们进来文件系统了，shell变成了”root@target：#“。  

* 修改默认的shell bash 
	`root@target:# rm /bin/sh && ln -s /bin/bash /bin/sh`  

* 然后，你就可以利用平时我们操作ubuntu系统的一切命令来操作这个系统，安装软件、更改配置等等   

* 我们建议你进行一项操作，就是增加一个开机脚本，让你可以增加一些开机启动的程序  
	`vim /usr/local/bin/mtd-by-name.sh`  

加入下列文本  
*{
   #!/bin/sh -e
   # mtd-by-name link the mtdblock to name
   # radxa.com, thanks to naobsd
   rm -rf /dev/block/mtd/by-name/
   mkdir -p /dev/block/mtd/by-name
   for i in `ls -d /sys/class/mtd/mtd*[0-9]`; do
       name=`cat $i/name`
       tmp="`echo $i | sed -e 's/mtd/mtdblock/g'`"	
       dev="`echo $tmp |sed -e 's/\/sys\/class\/mtdblock/\/dev/g'`"
       ln -s $dev /dev/block/mtd/by-name/$name
   done
}*  

* 在/etc/rc.local文件 “exit 0”前加入下面文字  
> /usr/local/bin/mtd-by-name.sh  

* 回到PC系统  
	`exit`  
	`sync`  
	`sudo umount /mnt`  

然后在继续Linux固件制作的步骤！  


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
