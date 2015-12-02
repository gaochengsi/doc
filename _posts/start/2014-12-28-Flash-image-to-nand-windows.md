---
layout: post
title: 将image烧写到NAND（Windows）
category: 上手指南
tags: [入门, Flash, image, NAND]
keywords: image,NAND
description: 
---

>这篇文章描述了windows平台下，如何在Radxa Rock的nand flash上安装镜像，比如，如何烧写开发板。你可以选择烧写整个nand镜像（比如update.img）到开发板或者仅仅烧写选定的分区。你可以从我们的服务器下载prebuilt镜像或者编译你自己的镜像。

##  在你开始之前

记住：

```
你可以一直烧写rock，它不会变成板砖，除非一些硬件损坏了。
```

烧写开发板你需要做的是：

- Radxa Rock(检查硬件版本，有Pro 和Full/Lite版，固件是不同的)
- 一台电脑windows(win xp 32/64bit,win7 32//64bit或者linux 32/64bit)
- 一个micro USB线-一端插在Radxa Rock的OTG口，另一端插在PC的USB口

## windows平台

烧写镜像的Windows工具由Rockchip提供。为了烧写update.img，你需要使用RKBath Tool然后烧写到分区，你需要使用RKDevelop Tool。RKBath Tool和RKDevelop Tool依赖于RK USB驱动，所以你需要首先安装一下：

### 安装驱动

1. 使用RKDriver Assistant tools-这是简单的方式（仅支持winxp,Vista,win7，不支持 win8）
2. 手动安装（支持winxp,vista,win7,win8）

如果你已经安装了USB驱动，请跳过这一步。

#### a.使用RDKriver Assistant tools

注意：这个方式仅支持winxp,vista,win7,不支持win8。

在这个过程中仅仅下载和解压[RKDriverAssistant.zip](http://dl.radxa.com/rock/tools/windows/RK_DriverAssitant.zip)，没必要连接你的Rockchip设备。然后双击 DriverInstall.exe 启动安装。如果你已经事先已经安装过了Rockchip USB 驱动，请先确保你卸载了。

![image](http://radxa.com/mw/images/b/ba/RK_Driver_Assistant_Install_Uninstall.jpg)

点击“Install Driver”。当驱动安装完了，然后关掉Radxa开发板，然后连接Radxa Rock到你的电脑，让开发板进入[loder 模式](http://radxa.com/Rock/Loader_mode)，然后你的电脑会检测开发板，你会看到它在设备管理器里面了：

![image](http://radxa.com/mw/images/2/2a/RK_Driver_Assistant_Install_Usb_driver.png)

这时，驱动应该已经成功安装了，恭喜你！

#### b.手动安装

先下载驱动。如果你使用winxp,win7系统，从这里下载[usb_driver_v3.5.zip](http://dl.radxa.com/rock/tools/windows/usb_driver_v3.5.zip)。 win8系统呢，从这里下载[usb_driver_v3.7.zip](http://dl.radxa.com/rock/tools/windows/Rockusb%20Driver%20v3.7.zip)。关闭Radxa Rock开发板，然后连接Radxa Rock到电脑，让开发板进入到loder模式。在你的电脑检测到开发板后，你会看到设备管理器是这样的：

![image](http://radxa.com/mw/images/4/44/Flash_image_1.jpg)

选择未识别的设备，右击，然后选择“更新软件驱动”。

![image](http://radxa.com/mw/images/a/a5/Flash_image_2.jpg)

选择“浏览电脑查找驱动”

![image](http://radxa.com/mw/images/4/43/Flash_image_2.5.jpg)

找到你解压好的驱动文件（usb_driver_v3.5.zip），然后选择32/64位文件夹，然后点击“确定”安装。

![image](http://radxa.com/mw/images/e/e9/Flash_image_3.jpg)

当驱动正确安装了，你会在设备管理器看到Rock USB设备。

![image](http://radxa.com/mw/images/6/66/Flash_image_6.jpg)

驱动搞定了！

### 烧写镜像

有两种方法烧写镜像：

1. 使用RKBatch Tool（工厂更新固件工具，只烧写update.img，它会擦出所有的东西）
2. 使用RKAndroid Tool(开发工具，可以烧写分区)

####  使用RKBatch Tool来烧写update.img

从[这里](http://dl.radxa.com/rock/tools/windows/RK_BatchTool_V1.7.zip)下载RKbatch Tool。解压并运行RKBatch Tool.exe，你会看到下面的界面：

![image](http://radxa.com/mw/images/1/18/Flash_image_7.jpg)

选择镜像（rockdev/update.img)。可以查看[这里](http://radxa.com/Rock/update.img)来看如何制作nand 镜像。

![image](http://radxa.com/mw/images/c/ca/Flash_image_8.jpg)

关闭Radxa Rock开发板，然后将它连接到电脑上，让开发板进入[loder模式][1]，此时程序应该检测到了设备。

![image](http://radxa.com/mw/images/5/59/Flash_image_9.jpg)

点击“Update”按钮开始烧写，当烧写结束，你应该看到下面的界面：

![image](http://radxa.com/mw/images/9/9a/Flash_image_10.jpg)

如果烧写中断，它会导致update.img出现错误，你可以重新操作以上步骤或者重新制作update.img。

#### 使用RKAndroid Tool来烧写到分区

从[这里](http://dl.radxa.com/rock/tools/windows/RKDevelopTool_v1.37.zip)下载RKAndroid Tool。解压，然后运行RKAndroidTool.exe，你会看到：

![image](http://radxa.com/mw/images/2/28/Rkandroidtool_flash_image_1.png)

关闭Radxa Rock开发板，然后将它连接到电脑上，让开发板进入loder模式，此时程序应该检测到了设备。

![image](http://radxa.com/mw/images/6/6d/Rkandroidtool_flash_image_2.png)

有八个选择可以选，所以选择正确的行，然后选中行左侧的复选框。然后你必须点击右侧的列来选择你要烧写的文件。你可以选择一个或者多个文件，然后一次性烧写进去。最终，点击“Run”来开始烧写。

![image](http://radxa.com/mw/images/c/ce/Rkandroidtool_flash_image_3.png)

如果成功了开发板会从电脑断开，然后启动系统。

请注意：

1. 当你通过`./mkimage.sh ota`命令打包镜像，内核会在boot.img里面，你应该看[Rock/Android_Build ](http://radxa.com/Rock/Android_Build)得到更多信息。
2. 你可以按照需求一次性烧写一个或多个镜像。

### 常见问题

#### 问题1:  recovery模式下 RKBatchTool 检测不到开发板

情景1：

我已经手动安装了驱动，并且Recovery模式的USB为PID_310B，“Rockusb device”驱动在Vista上安装成功了，但是RKBatchTool连接不到RK3188的板子，导致无法烧写镜像。

解决办法:

RK3188板通过USB3.0 PMMCIA卡连接到笔记本电脑。这是一个USB2.0到USB3.0的兼容性问题，所以USB3.0端口换成USB 2.0端口，然后RKBatchTool应该可以连接到RK3188板子来烧写镜像了。

说明:
1.Recovery 模式 : USB 设备硬件:  VID_2207&PID_310

2.引导进Android:  USB 设备硬件: VID_2207&PID_0006

情景2：

驱动安装正常，PC可以识别到设备，确定进入了recovery模式，但是一直BatchTool一直无法识别设备。

原因分析：使用了自己从网上下载的刷机工具，比如BatchTool 1.5.

解决办法：使用教程中指定的BatchTool工具

#### 问题2: 烧写失败
情景1：烧写后，串口打印：

```
DDR Version 1.04 20130517 In DDR3 300MHz Bus Width=32 Col=10 Bank=8 Row=15 CS=2 Die Bus-Width=16 Size=2048MB Memory OK OUT BUILD=====6 SdmmcInit=0 20 F:32 1061 0 0 40 FlashReadRetry error!!,row = fff000 FlashReadRetry error!!,row = fff000   
 ```
同时无法引导。


解决办法:

如果烧写失败，尝试在烧写前格式化NandFlash。

RKAndroidTool - 单击 “Erease IDB”按钮，格式化NandFlash。
RKBatchTool    - 单击 “Restore”按钮格式化，然后烧写镜像。

情景2: 刷机的时候 遇到 Preparing IDB failed
这有两种情况 

1）USB线供电不足，此时建议更换质量好的USB线 或者在进入刷机模式后插上电源

2 ) bootloader 版本不同导致，如果你之前刷的固件的bootloader版本是高版本，然后刷回低版本的固件，那么你可能会遇到这个问题。板子这是“变砖”了。。。按照这个 [解砖](/2015/07/20/umbrick-rock.html) 解决它。


[1]:/2014/12/31/enter-recovery-mode.html  

