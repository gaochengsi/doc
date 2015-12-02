---
layout: post
title: Linux系统固件制作
category: Linux
tags: [linux]
keywords: radxa
description: 
---

# 制作Linux系统的固件

本篇文章将详细介绍如何制作Rock lite/pro上运行的Linux固件，你需要的，就是一台能上网的Ubuntu的电脑  

## 1.编译环境  

#### 下载交叉编译工具链  

##### 64bit 主机  
  `git clone -b kitkat-release --depth=1  https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.6`  
  
或者这里下载http://dl.radxa.com/rock/source/x86_64_arm-eabi-4.6.zip   

##### 32bit 主机  
  `git clone -b jb-release --depth=1 https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.6`  
  
或者在这里下载http://dl.radxa.com/rock/source/x86_32_arm-eabi-4.6.zip  



#### 设置环境变量  

  `export ARCH=arm`  
  `export CROSS_COMPILE=`pwd`/arm-eabi-4.6/bin/arm-eabi-`
  
  **需要注意的是，上面CROSS_COMPILE是表示编译器的路经，make的时候，会从该路经寻找编译器，可能需要改变，因为也许你已经安装过該编译器，或者说你想使用你其他编译器，这时候，就需要特别注意该变量的值。**  
  
## 2.获取Linux内核源码  

* Kernel v3.0:
`git clone -b radxa-stable-3.0 https://github.com/radxa/linux-rockchip.git`  


## 3.编译内核  

* 进入内核根目录内  
  `cd linux-rockchip`  
* 如果你的板子是2014版的pro或者lite，那么请使用radxa_rock_pro_linux_defconfig  
  `make radxa_rock_pro_linux_defconfig`  
* 如果你的板子是2013版的pro或者lite，那么请使用radxa_rock_linux_defconfig  
  `make radxa_rock_linux_defconfig`  
    **所有的配置文件都保存在/arch/arm/configs/目录下，你可以根据需要，修改配置文件，或者使用我们推荐的配置文件**  
* 开始编译  
  `make -j8`  

生成的内核镜像文件位置是在 arch/arm/boot/Image。  

## 4.编译内核模块  

  `mkdir modules`  
  `export INSTALL_MOD_PATH=./modules`  
  `make modules && make modules_install`  
  `cd ..`  
  
现在所以的内核模块都在 modules/lib/modules/3.0.36+/文件夹中。待会儿在制作文件系统的时候需要用到。  

## 5.制作booting.img镜像  

* 获取虚拟磁盘镜像  
  `git clone https://github.com/radxa/initrd.git`  
  `cd initrd`  
  `find . ! -path "./.git*"  | cpio -H newc  -ov > initrd.img`  
  此时，在当前文件夹下，就会生成一个initrd.img的文件  
  
* 获取打包工具mkbooting  
`wget  http://dl.radxa.com/rock/tools/linux/mkbootimg`  
`sudo apt-get install lib32stdc++6`  
`hmod +x mkbootimg`  

* 进行打包，具体路径根据你的当前路径确定  
  `./mkbootimg --kernel linux-rockchip/arch/arm/boot/Image --ramdisk initrd.img -o boot.img`  
  完成。现在已经生成了RR的“boot.img”文件了。 

此时生成的“booting.img”镜像，只是一个内核和虚拟磁盘的打包，是无法真正启动一块板子的。  

## 6.制作根文件系统rootfs  
  
* 首先选择下载一个根文件系统  
  http://releases.linaro.org/  
  
* 然后申请一块和下载的文件系统大小略多的空间  
  `dd if=/dev/zero of=rootfs.img bs=1M count=256`  

* 为这个文件系统分区命名为linuxroot  
  `mkfs.ext4 -F rootfs.img -L linuxroot`  
  （该名字被使用在内核开启时寻找文件系统使用，我们在使用upgrade_tool时，也可以只烧录文件系统，使用该分区名，如:
  `upgrade_tool di linuxroot rootfs.img`）  

* 把这个分区文件挂载到/mnt下  
   mount -o loop rootfs.img /mnt  

* 解压linaro的tar gz rootfs的所有内容到/mnt下，即不要带有解压包名字的文件夹。bin、tmp等文件夹应在PC的mnt文件夹下。  
  
* 然后取消挂载  
  `umount /mnt`  

这样，就制作好了一个Linux文件系统。  

## 6.完成固件制作的最后一步  

获取打包工具  
`git clone https://github.com/radxa/rockchip-pack-tools.git`  
  
文件夹下会得到一个package-file的文件  
  *{  
	# NAME			Relative path  
	#HWDEF		HWDEF  
	package-file		package-file*  
	bootloader		RK3188Loader(L)_V2.19.bin  
	**parameter		parameter**  
	**boot     			Linux/boot-linux.img**  
	*linuxroot			Linux/rootfs.img  
	backup			RESERVED  
	update-script		update-script  
	recover-script	recover-script  
  }*

  里面列出了一个完整的updata.img所需要的全部组件。  
  
其中，加粗的部分，是我们刚刚制作好的booting.img，以及文件系统rootfs。其他部分在该文件夹下都已提供。你可以选择把package-file文件中的相应部分改成你制作的文件名字，或把你制作的文件名字改成和package-file中一致，且路径相同，然后执行 该目录下的一个脚本  
  `./mkupdate.sh `  
  
即得到可烧录的updata.img镜像。  
