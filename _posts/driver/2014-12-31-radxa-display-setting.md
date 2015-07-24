---
layout: post
title: 关于显示
category: 硬件操作
tags: [显示, display, 分辨率, 资源]
keywords: redxa,rock，display
description: 
---

##关于显示

Radxa Rock- RK3188有两个LCD控制器，LCD0和LCD1。在RR上，LCD1连接HDMI，LCD0连接AV输出，LCD0的信号也可以在扩展口上输出。

有一些sysfs接口会输出控制显示相关的东西。**下面代码应该可以在Android和Linux都运行很好**

## 设置显示比例 

设置一个变量，方便操作  

  root@radxa:# export SCALE=/sys/class/graphics/fb0/scale

对于新的Ubuntu系统，你需要使用下面的名面替代上面那条  

   root@radxa:# export SCALE=/sys/class/display/HDMI/scale

查看当前的设置  
   root@radxa:# cat $SCALE  
   xscale=95 yscale=95

改变设置  
   root@radxa:# echo 100 > $SCALE
   root@radxa:# cat $SCALE
   xscale=100 yscale=100


你也可以为X和Y设置不同的比例：

   root@radxa:# echo xscale=80 > $SCALE  
   root@radxa:# echo yscale=100 > $SCALE  
   root@radxa:# cat $SCALE  
   xscale=80 yscale=100  


## 设置HDMI输出模式 

你可以输入下面的命令来查看支持的输出模式：

```
   root@radxa:# cat /sys/class/display/display0.HDMI/modes
   #For the new Ubuntu 14.04 image, the path is changed to
   root@radxa:# cat /sys/class/display/HDMI/modes
   1920x1080p-60
   1920x1080p-50
   1280x720p-60
   1280x720p-50
   720x576p-50
   720x480p-60
```

查看当前的显示模式：

```
   root@radxa:# cat /sys/class/display/display0.HDMI/mode
   #For the new Ubuntu 14.04 image, the path is changed to
   root@radxa:# cat /sys/class/display/HDMI/mode
   1280x720p-60
```

说明当前处于720p@60HZ。改变为1080p@60HZ模式：

```
   root@radxa:# echo 1920x1080p-60 > /sys/class/display/display0.HDMI/mode
   #For the new Ubuntu 14.04 image, the path is changed to
   root@radxa:# echo 1920x1080p-60 > /sys/class/display/HDMI/mode
```

## 设置X和终端的字体大小 

改变DPI来增大X的字体大小

```
rock@radxa:~$ sudo nano ~/.Xresources
```

增加下面的内容：

```
! Xft settings 
Xft.dpi: 120
Xft.antialias: true
Xft.rgba: rgb
Xft.hinting: true
Xft.hintstyle: hintslight
```

改变终端的设置：

```
rock@radxa:~$ sudo dpkg-reconfigure console-setup
```

然后一步一步往下：回车，回车，选择字体，然后回车，回车，选择大小，然后回车。

按ctrl+alt+f1切换终端，然后验证你的设置：

```
root@radxa:~# setupcon
```

你可以重新设置终端，知道找到最好的解决方案。。。

## 设置全彩色深度 

为了在Radxa rock linux镜像上使用24/32全彩色深度：

```
apt-get install fbset
```

```
cat <<eof>> /etc/init/fbset.conf
# fbset - run fbset to set truecolor fbmode
description "run fbset ASAP"
start on startup and started udev or starting lightdm
task
script
    echo 0 > /sys/class/display/HDMI/enable
    [ -x /bin/fbset ] && /bin/fbset -a -nonstd 1 -depth 32 -rgba "8/0,8/8,8/16,8/24"
    echo 1 > /sys/class/display/HDMI/enable
end script
eof
```

```
cat <<eof>> /etc/X11/xorg.conf
Section "Screen"
    Identifier "Default Screen"
    DefaultDepth 24
EndSection
eof
```

在重启之后，你应该不会看到绿色或者紫色的屏幕了。如果你看到了，说明出了些问题。你可以使用串口终端或者另一台电脑的SSH手动提交下面的命令：

```
service lightdm stop
fbset -a -nonstd 1 -depth 32 -rgba 8/0,8/8,8/16,8/24
service lightdm start
```

为了让这个改变成为永久的，在启动时的时序是很严格的。fbset需要在X/lightdm启动之前被调用。在Linaro/Ubuntu上，需要一个upstart脚本：fbset.conf。"startup and started udev"的机制会尝试尽早的启动fbset。在我的rk3188上，这是可靠的，但是如果你有问题，你最好祈求修补“start on”机制。不要使用/etc/rc.local，因为这个脚本在lightdm启动（已经被upstart启动之后）后很久才启动。

## 设置帧缓冲分辨率 

设置帧缓冲分辨率为720*480像素：

```
service lightdm stop
fbset -a -xres 720  -yres  480 -vxres 720  -vyres  480
service lightdm start
```

如果你得到一个空白的屏幕，尝试重新激活屏幕：

```
echo 1 > /sys/class/graphics/fb0/enable
```

一些其他分辨率设置的例子：

```
fbset -a -xres 720  -yres  576 -vxres 720  -vyres  576
fbset -a -xres 1024 -yres  600 -vxres 1024 -vyres  600
fbset -a -xres 1280 -yres  720 -vxres 1280 -vyres  720
fbset -a -xres 1920 -yres 1080 -vxres 1920 -vyres 1080
```



--------------------------------------------------------------------
* 如果需要更详细更全面的信息，请登陆  
  http://radxa.com              官方网站  
  339567728                     QQ讨论群  
  http://cn.radxa.com/forum.php         中文论坛  
* 另外，本手册所使用的所有源码、固件、工具，都可以登陆以下地址下载  
  http://dl.radxa.com/                                    国外服务器  
  http://pan.baidu.com/share/home?uk=3108273493#category/type=0  百度云  
* 手册内容经小编实际操作，均可正常使用，但因系统以及整理文档等原因，若出现错误，请谅解，并使用以下邮箱联系我们  
  kevin@radxa.com  

## Radxa团队  

### 2015年7月  
--------------------------------------------------------------------


