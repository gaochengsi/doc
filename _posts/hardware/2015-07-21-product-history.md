---
layout: post
title: 硬件版本信息
category: 硬件信息
tags: [产品]
keywords: radxa
description: 
---

# 2013.08.12 值得记住的日子

第一块radxa rock样板也叫radxa rock工程板，只生产了100片。几乎所有的东西都运行良好。但是还是有一些小问题：

- HDMI接口太靠近SPDIF 插孔，可能会与一些大头的HDMI线冲突  
- UART可以被一些5V TTL转USB损坏（警告：请务必使用3.3V TTL转USB线）  
- 复位键可以作为POWER halt功能，重启板子需要按重启键然后按电源键。  

![](http://radxa.com/mw/images/thumb/a/ad/Rock_es_front.png/1000px-Rock_es_front.png)

=

=


# 这是Radxa Rock的第一个MP版本。

这个版本修复了所有在2013.08.12版本已知的问题。因为HDMI接口的的位置移动了一点，这个版本的例子无法用在之前的版本上，除了一些简单的例子。软件和之前的版本是一样的。



## 电路方面的改变

## PCB方面的改变

![image](http://radxa.com/mw/images/4/47/Rock_front.jpg)  

![image](http://radxa.com/mw/images/7/71/Rock_back.jpg)  


=

=

#  2014-06-10,Rock Pro 发布
这个一款新的硬件版本，满足了社区的要求。

### 主要的改变是：

- 增加LVDS接口（FPC接口位于主板底部）

- 增加触控面板接口

- 增加连接摄像模块的CSI接口

- 主板多了扬声器驱动电路
- 没有Nand，1G DDR

### 详情  


- 处理器：ARM CORTEX-A9 QUAD CORE @1.6GHZ
- 图像处理器：MAIL400-MP4@533MHZ,OPENGL ES 2.0
- RAM:2GB DDR3,800MHZ
- 内存：8GB FLASH
- 视频输出
  - 数字输出：HDMI1.4，最大支持1080P
  - 模拟输出：AV端口输出
- 音频输出：
  - 3.5MM 音频口
  - S/PDIF
- 调试：支持串口调试
- 电源输入：5V DC接口
- USB：
  - 两个USB 2.0 HOST，TYPE A
  - 一个USB OTG，miro USB接口
- 串口：板载有串口引脚
- 以太网支持：10/100M bit/s RJ45接口
- SD/MMC卡：可外接micro SD卡，最大支持128G内存卡
- 按键输入：一个电源键，一个复位键，一个恢复键
- LED灯：三个可编程的LED灯
- 实时时钟：板载有实时时钟
- 红外：一个红外接收器
- 扩展：共有80个pins
  - 支持GPIO,I2C,SPI,line in，USB2.0 ，PWM,ADC,LCD,GPS等
-  重量：61克（仅主板） 
-  尺寸：100*80 MM
-  系统：支持Linux、Andriod、FreeBSD系统，默认搭载Android4.2.2
  
   

![image](http://radxa.com/mw/images/7/74/Rock_lite.jpg)

![image](http://radxa.com/mw/images/0/04/Rock_lite_back.jpg)

参考：

1. http://radxa.com/Rock/hardware_revision

=

=

#  2014-06-10,Rock lite 发布  

这个一款新的硬件版本，满足了社区的要求。

### 主要的改变是：

- 增加LVDS接口（FPC接口位于主板底部）

- 增加触控面板接口

- 增加连接摄像模块的CSI接口

- 主板多了扬声器驱动电路
- 没有Nand，1G DDR

![image](http://radxa.com/mw/images/7/74/Rock_lite.jpg)

![image](http://radxa.com/mw/images/0/04/Rock_lite_back.jpg)

### 详情  

Radxa Rock Lite是一款基于ARM Cortex-  

A9构架的开源四核开发板，搭载了国内首款采用28nm工艺瑞芯微RK3188芯片，主频最高达到1.6Ghz，比主流的32nm工艺三星Exynos 4412处理器，还有40nm工艺的Tegra3功耗更低，官方宣称运算速度比Cortex-A7提升35%。

拥有1G内存、板载wifi\RTC,外围接口丰富，新增LVDS 接口,触摸屏接口,camera 摄像头接口,板载扬声器驱动电路,板载80扩展PIN，支持GPIO, I2C, SPI, Line in, USB 2.0, PWM, ADC, LCD, GPS等，性价比超高，是一款理想的diy开发板。
  
## 产品配置   
                     
- 处理器：ARM Cortex-A9,四核RK3188处理器
- 支持wifi
- HDMI输出接口，1080p@60Hz
- 自带两个2.0 USB和一个OTG接口
- 1G-DDR3内存
- 支持RTC ，TF卡
- 支持Linux、Andriod、FreeBSD系统

## 硬件清单                        
1. Radxa Rock Lite开发板*1
2. 电源线*1
3. 可拆卸wifi天线*1
4. 亚克力外壳*1
5. 环保外包装*1


## 配件选购  
                      
1. 5V 2A电源（RADXA在负荷量较大的情况下，需要5V2A才能正常工作）
2. USB to TTL串口调试线 
3. 双头HDMI显示高清线
4. 音频视频AV连接线
5. Mirco USB刷机线
6. 11键IR遥控器
7. HDMI转VGA转接头
8. 带风扇散热器



=

=


# Oct 11, 2014 要开始进入Rock2的时代了  

第一块 PCB 样板到了

![image](http://radxa.com/mw/images/d/de/Rock2_module_pcb_top.png)


![image](http://radxa.com/mw/images/b/bf/Rock2_module_pcb_bottom.png) 

- Oct 11, 2014 
 
module 终于 SMT 了，现在等待 baseboard...

![image](http://radxa.com/mw/images/9/9d/Rock2_module_smt.jpg)

- Nov 2, 2014

base board 到了，酷吗？

![image](http://radxa.com/mw/images/b/b0/Rock2_base_top.jpg)

![image](http://radxa.com/mw/images/8/82/Rock2_base_bottom.jpg)

Bring up：

如何使 basic system 在 rock2 上运行？


[Rock2 bringing up](http://radxa.com/Rock2/bringup)



参考：

- http://radxa.com/Rock2/full_bb  
