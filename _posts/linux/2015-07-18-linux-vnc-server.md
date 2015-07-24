---
layout: post
title: 设置VNC服务器 
category: Linux
tags: [产品]
keywords: radxa
description: 
---

## 设置VNC服务器 

你可以通过设定VNC（Virtu Network Computing）服务器从网络上远程进入你的radxa rock桌面.  

* 如果你还没有在你的rock板上安装VNC服务器，需要用下面的命令安装它（前提是你的板子能连接网络）：  
	`sudo apt-get install tightvncserver`  

* 第一次运行时设定密码：  
	`root@radxa:~# vncserver`  

* 你需要设定一个进入桌面的密码：并且再次确认它  

> Password: *****
  Warning: password truncated to the length of 8.                                 
  Verify: *****    

* 询问你是否要设置一个查询密码时，选择n  

> Would you like to enter a view-only password (y/n)? n 
  xauth:  file /root/.Xauthority does not exist                                   
  New 'X' desktop is radxa:1       
  Creating default startup script /root/.vnc/xstartup                             
  Starting applications specified in /root/.vnc/xstartup                          
  Log file is /root/.vnc/radxa:1.log  

* 现在编辑 ~/.vnc/xstartup 文件，并在 x-window-manager 后面添加一行，就像：  

> \#!/bin/sh 
  xrdb $HOME/.Xresources  
  xsetroot -solid grey  
  \#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &  
  \#x-window-manager &  
  startlubuntu &  
  \# Fix to make GNOME work  
  export XKL_XMODMAP_DISABLE=1  
  /etc/X11/Xsession  

* 现在重启VNC服务：  
	`root@radxa:~# vncserver -kill :1`  
	`root@radxa:~# vncserver -geometry 1280x800 :1`  

现在Rock板子上运行的VNC服务器桌面分辨率是1280x800的。  

## 远程桌面

#### Linux  

* 这里我们使用Remmina Remote Desktop 作为VNC客户端，用于进入rock的远程桌面。 单击 'plus'按钮，创建一个新的VNC连接，并按照如下步骤配置它：  

	Name: 随便你起  

	Group: 可以留空  
 
	Protocal: 选择 "VNC - Virtual Network Computing"  

	Server: 输入你的rock板子的IP地址，后面加上 “:1”，比如 192.168.1.101:1  

	Color depth: 选择True color(24 bit)  

	Quality: 选择 best  

* 下面是配置的例子:  
  
![Remmina profile.png](http://radxa.com/mw/images/8/8b/Remmina_profile.png)    

现在单击 “connect”，输入我们刚才设定的密码就可以了。  

![Remmina pw.png](http://radxa.com/mw/images/4/47/Remmina_pw.png)  

#### Windows  

在Windows平台，我们使用VNC Viewr来作为VNC的客户端，使用它你可以远程查看并控制你的Radxa板子的桌面环境。 它的使用很简单，你可以按照下面的方法配置它：  

Server: 你的Radxa Rock板子的IP地址，后面加上":1"，比如 192.168.1.101:1 然后单击 'OK'，启动连接。 

![VNCConfig-1.jpg](http://radxa.com/mw/images/9/94/VNCConfig-1.jpg)  

下面输入我们之前设定的密码，并单击OK：  

![VNCConfig-2.jpg](http://radxa.com/mw/images/f/fd/VNCConfig-2.jpg)  





--------------------------------------------------------------------
* 如果需要更详细更全面的信息，请登陆  
	http://radxa.com  						官方网站  
	339567728         						QQ讨论群  
	http://cn.radxa.com/forum.php					中文论坛  
* 另外，本手册所使用的所有源码、固件、工具，都可以登陆以下地址下载  
	http://dl.radxa.com/                             	      国外服务器  
	http://pan.baidu.com/share/home?uk=3108273493#category/type=0    百度云  
* 手册内容经小编实际操作，均可正常使用，但因系统以及整理文档等原因，若出现错误，请谅解，并使用以下邮箱联系我们  
	kevin@radxa.com  

## Radxa团队  

### 2015年7月  
--------------------------------------------------------------------
