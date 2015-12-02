---
layout: page
title: 如何启动 FreeBSD
category: Linux
tags: [产品, 配置]
keywords: 产品, 配置
description:
---
# 如何启动 FreeBSD  

Ganbold，一个FreeBSD开发者，现在搞定了在Rockchip平台上的FreeBSD引导工作。Ganbold 告诉我们只有基本模块、GPIO、DWC   

USB主机模式可以工作。但这只是个开始，如果你感兴趣，在#radxa的免费IRC频道联系Ganbold，以获取更详细的资料。  

请看 FreeBSD ARM wiki的介绍:  

https://wiki.freebsd.org/FreeBSD/arm/Radxa%20Rock  

使用Ubuntu 13.10 amd64 并且使用rkflashtool 编译的请注意：  

* 通过 sudo apt-get -y install libusb-1.0-0-dev来安装 libusb.h   

* 拔掉板子电源。然后按住Recovery按键，再连接USB线（连接到OTG接口的）到计算机，等3秒钟松开Recovery按键。 如果此时RK3188没有进入Recovery模式，你在运行 ./rkflashtool p > param.txt 时，会出现："rkflashtool: fatal: cannot open device"。  

* 使用下面的Makefile：  

> #  
 all: rkflashtool  
 
 rkflashtool: rkflashtool.c  
 	gcc -o rkflashtool rkflashtool.c -I/usr/include/libusb-1.0/ -lusb-1.0 -O2 -W -Wall  
   
 clean:  
 	rm -f rkflashtool  
 
 param:  
 	sudo ./rkflashtool r 0x0000 0x2000 > /tmp/parm  
   
 #  


# 启动日志  

 >  KDB: debugger backends: ddb    
	KDB: current backend: ddb  
	Copyright (c) 1992-2014 The FreeBSD Project.  
	Copyright (c) 1979, 1980, 1983, 1986, 1988, 1989, 1991, 1992, 1993, 1994  
		The Regents of the University of California. All rights reserved.  
	FreeBSD is a registered trademark of The FreeBSD Foundation.  
	FreeBSD 11.0-CURRENT #0: Wed May 28 15:59:10 UTC 2014  
	  root@beaglebone:/usr/src/sys/arm/compile/RADXA arm  
	FreeBSD clang version 3.4 (tags/RELEASE_34/final 197956) 20140216  
	CPU: Cortex A9-r3 rev 0 (Cortex-A core)  
	Supported features: ARM_ISA THUMB2 JAZELLE THUMBEE ARMv4 Security_Ext  
	WB disabled EABT branch prediction enabled  
	LoUU:2 LoC:1 LoUIS:2   
	Cache level 1:   
	32KB/32B 4-way data cache WB Read-Alloc Write-Alloc  
	32KB/32B 4-way instruction cache Read-Alloc  
	real memory  = 2147483648 (2048 MB)  
	avail memory = 2095329280 (1998 MB)  
	FreeBSD/SMP: Multiprocessor System Detected: 4 CPUs  
	random: <Software, Yarrow> initialized  
	ofwbus0: <Open Firmware Device Tree>  
	simplebus0: <Flattened device tree simple bus> on ofwbus0  
	gic0: <ARM Generic Interrupt Controller> mem 0x1013d000-0x1013dfff,0x1013c100-0x1013c1ff on simplebus0  
	gic0: pn 0x390, arch 0x1, rev 0x2, implementer 0x43b sc->nirqs 160  
	rk30_pmu0: <RK30XX PMU> mem 0x20004000-0x200040ff on simplebus0  
	rk30_grf0: <RK30XX General Register File> mem 0x20008000-0x20009fff on simplebus0  
	mp_tmr0: <ARM MPCore Timers> mem 0x1013c200-0x1013c2ff,0x1013c600-0x1013c61f irq 27,29 on simplebus0  
	Timecounter "MPCore" frequency 148500000 Hz quality 800  
	Event timer "MPCore" frequency 148500000 Hz quality 1000  
	k30_wd0: <Rockchip RK30XX Watchdog> mem 0x2004c000-0x2004c0ff on simplebus0  
	gpio0: <Rockchip RK30XX GPIO controller> mem 0x2000a000-0x2000a0ff irq 86 on simplebus0  
	gpioc0: <GPIO controller> on gpio0  
	gpiobus0: <GPIO bus> on gpio0  
	gpio1: <Rockchip RK30XX GPIO controller> mem 0x2003c000-0x2003c0ff irq 87 on simplebus0  
	device_attach: gpio1 attach returned 6  
	gpio1: <Rockchip RK30XX GPIO controller> mem 0x2003e000-0x2003e0ff irq 88 on simplebus0  
	device_attach: gpio1 attach returned 6  
	gpio1: <Rockchip RK30XX GPIO controller> mem 0x20080000-0x200800ff irq 89 on simplebus0  
	device_attach: gpio1 attach returned 6  
	dwcotg0: <DWC OTG 2.0 integrated USB controller> mem 0x10180000-0x101bffff irq 48 on simplebus0  
	usbus0 on dwcotg0  
	dwcotg1: <DWC OTG 2.0 integrated USB controller> mem 0x101c0000-0x101fffff irq 49 on simplebus0  
	usbus1 on dwcotg1  
	uart0: <16650 or compatible> mem 0x20064000-0x200643ff irq 68 on simplebus0  
	uart0: console (115200,n,8,1)  
	Timecounters tick every 10.000 msec  
	usbus0: 480Mbps High Speed USB v2.0  
	usbus1: 480Mbps High Speed USB v2.0  
	ugen1.1: <DWCOTG> at usbus1  
	uhub0: <DWCOTG OTG Root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus1  
	ugen0.1: <DWCOTG> at usbus0  
	uhub1: <DWCOTG OTG Root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus0  
	random: unblocking device.  
	Release APs  
	Root mount waiting for: usbus1 usbus0  
	uhub0: 1 port with 1 removable, self powered  
	uhub1: 1 port with 1 removable, self powered  
	ugen1.2: <vendor 0x05e3> at usbus1  
	uhub2: <vendor 0x05e3 USB2.0 Hub, class 9/0, rev 2.00/85.36, addr 2> on usbus1  
	Root mount waiting for: usbus1  
	uhub2: 4 ports with 4 removable, self powered  
	Root mount waiting for: usbus1  
	ugen1.3: <Realtek> at usbus1  
	usbd_set_config_index: could not read device status: USB_ERR_SHORT_XFER  
	Root mount waiting for: usbus1  
	ugen1.4: <BUFFALO> at usbus1  
	umass0: <MSC Bulk-Only Transfer> on usbus1  
	umass0:  SCSI over Bulk-Only; quirks = 0x4100  
	umass0:0:0: Attached to scbudsa00  
	at umass-sim0 bus 0 scbus0 target 0 lun 0  
	da0: <WDC WD16 00BEVS-22RST0 > Fixed Direct Access SCSI-2 device   
	da0: Serial Number 00001504B59E  
	da0: 40.000MB/s transfers  
	da0: 152377MB (312069552 512 byte sectors: 255H 63S/T 19425C)  
	da0: quirks=0x2<NO_6_BYTE>  
	Root mount waiting for: usbus1  
	ugen1.5: <vendor 0x0b95> at usbus1  
	axe0: <vendor 0x0b95 product 0x7720, rev 2.00/0.01, addr 5> on usbus1  
	Trying to mount root from ufs:/dev/da0s2 []...  
	warning: no time-of-day clock registered, system time will not be set accurately  
	miibus0: <MII bus> on axe0  
	kphy0: <Generic IEEE 802.3u media interface> PHY 16 on miibus0  
	ukphy0:  none, 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, auto  
	ue0: <USB Ethernet> on axe0  
	ue0: Ethernet address: **:**:**:**:**:**  
	Spurious interrupt detected [0x000003ff]  
  
