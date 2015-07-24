---
layout: post
title: HDMI分辨率调整
category: 硬件操作
tags: [HDMI]
keywords: HDMI
description: 如何调整HDMI分辨率？
---

说明： 

1）目前这种方法适用于rock2 square, rock pro 和 rock lite

2）方法适用于Android 和 rabian 

第一步：获取显示器的edid参数 （目前只找到windows下的工具）

下载工具
http://dl.radxa.com/users/yao/sm_setup.exe
http://dl.radxa.com/users/yao/Phoenix.zip

操作步骤
使用SoftMCCS 软件，打开之后，软件界面如下

![此处输入图片的描述][1]

1、打开 sm_setup.exe 软件

2、如果接了多台显示器，则在左上角栏中选择读取EDID的显示器型号，如"DELL E198WFP"

3、点击左上角“File -> Save EDID as”，将EDID信息保存在自己想要的位置，文件名可以自己指定，文件类型要选择“Raylar EDID file (*.dat)”
用文本编辑工具（如写字板）打开刚刚保存的EDID文件，如下图

![此处输入图片的描述][2]

第二步: 分析edid数据，得到相关参数
刚才我们已经通过“SoftMCCS”软件获取了EDID数据文件，下面要介绍另一个软件来分析这个数据，“Phoenix EDID Designer”(Phoenix.zip)。软件只有一个exe文件，不需要安装。点击打开按钮，然后找到并打开我们刚刚保存的EDID文件，打开后如下图：

![此处输入图片的描述][3]

选择 “Detailed Timings”（详细时序） 这一项

![此处输入图片的描述][4]

其中Timings 这一栏是我们需要的东西
从这个参数里面,我们可以得到显示器的
pixclk是 106500000
默认分辨率是：1440 x 900
这个对应要修改的文件是
drivers/video/rockchip/hdmi/hdmi-lcdc.c

其中：  
h_fp = H sync Offset = 80; h_pw = H sync width = 152;
h_bp + h_fp + h_pw = H Brank = 464,  所以 h_bp = 464 - 152 - 80 = 232。v_fp, v_bp,v_pw 和 h_fp,h_bp,h_pw 是同样的原理。至于后面的参数，可以直接使用下面的值（暂时还没有仔细研究区别，简单测试是可以用的）

```
/*  name  refresh xres   yres    pixclock   h_bp    h_fp    v_bp    v_fp    h_pw    v_pw   polariry      PorI    flag     vic      2ndvic      pixelrepeat     interface */

{ {"1440x900p@60Hz", 60, 1440,   900,    106500000, 232,    80, 5, 3, 152,    6, 0,  0, 0 },
2,  0,  1, OUT_P888},
```

修改后clean，然后重新编译内核

第三步： 刷修改后编译出的固件，设置分辨率

Rabian系统：

打开命令终端:  menu->System Tools-> MATE Terminal
或者通过串口执行以下命令：

```
#cat /sys/class/display/display0.HDMI/modes     
1920x1080p-60
..
1440x900p-60
..
```
查看hdmi支持的分辨率.

```
#echo 1440x900p-60 > /sys/class/display/display0.HDMI/mode1440x900p-60

```

Android 系统：

设置->display->HDMI Mode 选择 1440x900p-60，此时的HDMI分辨率 已经变成 1440x900@60Hz.


以上内容在我们这边的显示器上测试成功。也有几个客户通过此方法修改ok，但不确定是否全部都行的通，有问题大家可以在此反馈。

遇到的问题：

1) 设置分辨率后发现，刷新频率和设置的不同，比如 1360x768@60Hz, pixclock =85500000，但是结果显示刷新频率只有46，这是什么情况?

这是由于你设置的pixclock，系统采用，系统根据你设置的clock找到一个更好的clock值，这个值你可以在kernel的log中看到，大致如下:

```
[  119.205896] lcdc1: dclk:75000000>>fps:46 
```

为了达到60hz，你可以去调整 h_fp,h_bp,h_pw,v_fp,v_bp,v_pw的值(注意xres 和 yres不能改)。算法是（xres + h_fp + h_bp + h_pw） x (yres + v_fp + v_bp + v_pw) * fps = pixclock。其中fps 为刷新频率，比如  1360x768@60Hz 刷新频率就是60. 

2) 把分辨率 和 频率都改对了，但是画面显示的时候有雪花，为何？

原因： 和 h_fp,h_bp,h_pw,v_fp,v_bp,v_pw值的设置有关，合理调整后看看效果。

3）无论如何修改，都没有变化，为何？
请看修改是否生效，建议make clean ，然后重新编译内核。

关于EDID 的详细内容，可以参考：

 - http://blog.csdn.net/wangpeiyao5566/article/details/45270773
 - http://blog.csdn.net/wangpeiyao5566/article/details/45269379


  [1]: http://images.cnitblog.com/blog/152134/201301/30191707-565197d17e30422bac82f602e100a98b.jpg
  [2]: http://images.cnitblog.com/blog/152134/201301/30191853-e703dd60a80546f79fdfa39f8a96d919.jpg
  [3]: http://images.cnitblog.com/blog/152134/201301/30192346-393776216326489c8e66f39234d086dd.jpg
  [4]: http://cn.radxa.com/data/attachment/forum/201504/29/113412ip45ya4b01ps1abn.png
  




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


=======
>>>>>>> 0f5e364b66f0ab6ed04fe6799e937fb6c29c26fa
