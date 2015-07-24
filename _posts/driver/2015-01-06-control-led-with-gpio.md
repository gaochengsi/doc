---
layout: post
title: 通过 GPIO 控制 Radxa 的 LED
category: 硬件操作
tags: [GPIO]
keywords: GPIIO
description: 
---

在 Radxa Rock 官方的配置说明中，Radxa 板子上带了三个可编程控制的 LED 指示灯，正式版中分别为红、绿、蓝三个，并依次排列在电源开关附近。在启动默认的 android 系统的时候，也会亮起蓝色的指示灯。那要如何在 Linux 下控制这些 LED 呢？

首先来看 Radxa 官方的 Wiki 给出的一段演示脚本，用于控制绿色 LED 闪烁：

```
#!/bin/sh
 
# enable the gpio 172 -> green led
echo 172 > /sys/class/gpio/export
 
# set the direction to output
echo "out" > /sys/class/gpio/gpio172/direction
while true;
do
    echo 0 > /sys/class/gpio/gpio172/value #led on
    sleep 1
    echo 1 > /sys/class/gpio/gpio172/value #led off
    sleep 1
done

```

从这个演示程序中可以看到 Radxa 的这个 LED 直接连接在了 GPIO 上，利用 Linux 系统下 /sys/class/gpio 这个文件接口进行操作就可以实现简单的控制功能。当然，如果有比较高级的需求的话，建议还是需要编写相关的驱动程序来操作 GPIO，不过这回就先不折腾驱动程序这部分了。

其实 LED 的控制非常简单，只需要控制对应的 GPIO 管脚电平高低即可控制其发光状态。为了控制这些管脚，需要了解的最主要信息无非就是 LED 对应的控制引脚编号。由于官方已经给出了这部分的设计图 http://dl.radxa.com/rock/docs/hw/RADXA_ROCK_schematic_20131025.pdf，所以要确定 LED 的控制引脚并不困难。

![image](https://blog.huhamhire.com/wp-content/uploads/2014/02/rk_led_gpio.png)

从设计图中，可以比较容易的找到，这些信息。绿色的 LED 连接在了 GPIO0_B4 位置，蓝色 LED 连接在了 GPIO0_B6，而红色的则由 GPIO0_B7 来控制。接下来就需要计算这些引脚在 Linux 系统下对应的编号，首先需要参考到 rk3188 内核源代码中关于 GPIO 这部分的头文件，https://github.com/Galland/Linux3188/blob/master/arch/arm/mach-rk30/include/mach/gpio.h。

![image](https://blog.huhamhire.com/wp-content/uploads/2014/02/rk_led_pin.png)

同样，可以很容易的找到这几个引脚的定义，以及它们编号的计算公式，然后只需要将 PIN_BASE = 160 以及 NUM_GROUP = 32 代入计算即可得出相应的引脚编号，这部分数据可以参考 http://hwswbits.blogspot.com/2013/10/bitbanging-radxa-rock-gpios.html。

简单的计算后，可以得出绿色、蓝色、红色 LED 的引脚编号分别为 172、174 以及 175。然后只需要写一个脚本就可以实现一些简单的控制，比如下面我就可以通过一个简单的脚本实现开机后自动熄灭红色 LED，并亮起绿色 LED 的功能。

首先创建脚本控制 LED 的脚本：

```
sudo vim /usr/local/bin/power-led-on.sh
```

写入下面的内容：

```
#!/bin/bash
 
for LED_IO in 172 175
do
    echo $LED_IO > /sys/class/gpio/export
    echo "out" > /sys/class/gpio/gpio$LED_IO/direction
    echo 0 > /sys/class/gpio/gpio$LED_IO/value
    echo $LED_IO > /sys/class/gpio/unexport
done
```

这里需要说明的是，由于连接线路上的不同，蓝绿 LED 与红色 LED 在发光控制的数值参数上正好相反。

![rk_led_gb](https://blog.huhamhire.com/wp-content/uploads/2014/02/rk_led_gb.png)
![
rk_led_red](https://blog.huhamhire.com/wp-content/uploads/2014/02/rk_led_red.png)

蓝绿 LED 使用 0 输出控制点亮，1 控制熄灭；而红色 LED 则使用 1 控制点亮，0 控制熄灭。因而这里的操作就可以很方便的写在同一段循环体内。

接下来，为脚本文件赋予执行权限：

```
sudo chmod +x /usr/local/bin/power-led-on.sh
```

然后编辑系统启动时的相关操作：

```
sudo vim rootfs/etc/rc.local
```

在 exit 0 之前添加以下内容：

```
/usr/local/bin/power-led-on.sh
```

在做完上面的这些设置以后，重启 Radxa 后，就可以看到板子上会亮起绿色 LED，并且红色 LED 熄灭。

参考：

1. https://blog.huhamhire.com/viewpost-1109.html




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


