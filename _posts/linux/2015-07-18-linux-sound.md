---
layout: post
title: Linux 声音设置 
category: Linux
tags: [linux]
keywords: radxa
description: 
---

# Linux 声音设置 

## HDMI/AV 输出音频  

* 你可以使用下面的命令去测试Radxa Rock上的音频：  

	`root@radxa:~# speaker-test -t wav -c 2`  

* 会输出以下信息：  

> speaker-test 1.0.25  
  Playback device is default  
  Stream parameters are 48000Hz, S16_LE, 2 channels  
  WAV file(s)  
  Rate set to 48000Hz (requested 48000Hz)  
  Buffer size range from 48 to 32768  
  Period size range from 16 to 3072  
  Using max buffer size 32768  
  Periods = 4  
  was set period_size = 2979  
  was set buffer_size = 32768  
  0 - Front Left  
  1 - Front Right  
  Time per period = 2.061311  

* 此时你会从HDMI或者使用耳机（插在AV输出端子上）听到声音。  

## SPDIF  

如果你设置成默认从SPDIF输出音频的话，可以创建 /etc/asound.conf 并增加以下内容：  

> pcm.!default {  
     type hw  
     card 1  
     device 0  
}  
ctl.!default {  
     type hw  
     card 1  
 }  

## 在Rock上使用 USB DAC 的 Squeezelite  

* 在Logitech媒体服务器上使用USB DAC的squeezelite播放是可以实现的。没有Logitech媒体服务器的话，squeezelite是没有任何价值的。在我的附近有一些LMS的实例.  

* 从这里下载[Squeezelite AMRV6hf](https://code.google.com/p/squeezelite/downloads/detail?name=squeezelite-armv6hf&can=2&q=)  
   `sudo mv ./squeezelite-armv6hf /usr/bin/squeezelite`  
   `sudo chmod ug+x /usr/bin/squeezelite`  

* 把Dac插到USB中，并输入：  
   `squeezelite -l`  

* 会列出所有的音频设备，我通常使用前端设备。然后输入：  
   `squeezelite -o front:CARD=DAC,DEV=0 -n ANYNAMEYOUWANT -s YOUR . SERVER . ADDRESS -a ::16:`  

* 例如：  
   `squeezelite -o front:CARD=DAC,DEV=0 -n RadxaRock -s 10.0.1.99 -a ::16:`  

然而不幸的是我没办法让它连续工作24小时这么长。现在主要在监测一些爆破音，并想方设法去获取这些爆破音。如果你有好的想法请告诉我。

