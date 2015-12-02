---
layout: page
title: 使用板子上的红外
category: 硬件操作
tags: [红外]
keywords: 产品, 配置
description:
---

# 使用板子上的红外  

## 通过修改内核方式  

目前的SDK提供的驱动只支持NEC编码格式的红外遥控器，下面介绍如何调试来支持不同品牌的遥控器。 

只需要修改一个文件 "kernel/drivers/input/remotectl/rkxx_remotectl.c"  

步骤如下：  

* 增加一个数组  

	static struct rkxx_remote_key_table remote_key_table_41C8[] = {  
		{0x38, KEY_VOLUMEUP},  
		{0xb8, KEY_VOLUMEDOWN},  
		{0x58, KEY_MENU},  
		{0xd0, KEY_REPLY},  
		{0x48, KEY_BACK},  
		{0x98, KEY_BACK},  
		{0x50, KEY_UP},  
		{0x30, KEY_DOWN},  
		{0xc8, KEY_LEFT},  
		{0xc0, KEY_RIGHT},  
		{0x40, KEY_REPLY},  
		{0x80, KEY_SEARCH},   
	};  

该数组将红外键值ircode映射为Linux标准键盘扫描码scancode  


* 增加一项,这里的每一项对应一种遥控器  

>  	static struct rkxx_remotectl_button remotectl_button[]  
   For example：  
      {  
          .usercode = 0x41c8, /* 用户码 */  
          .nbuttons = 12,    /* 按键个数 */  
          .key_table = &remote_key_table_41C8[0], /* 键盘表 */  
      }  

第一个参数是用户码,每一种遥控器都有一个用户码用来和其他遥控器做区分.第二个参数对应遥控器的按键个数,第三个参数对应   步骤一增加的数组  

## 如何获得用户码和遥控器键值  

修改的地方在对应的函数 remotectl_do_something 中

>   	case RMC_USERCODE:  
	    {  
	        ddata->scanData <<= 1;  
	        ddata->count ++;  
	        if ((TIME_BIT1_MIN < ddata->period) && (ddata->period < TIME_BIT1_MAX)){  
	            ddata->scanData |= 0x01;  
	        }  
	        if (ddata->count == 0x10){//16 bit user code  
	            // printk("u=0x%x\n",((ddata->scanData)&0xFFFF));  
	            if (remotectl_keybdNum_lookup(ddata)){  
	                ddata->state = RMC_GETDATA;  
	                ddata->scanData = 0;  
	                ddata->count = 0;  
	            }else{  
	               ddata->state = RMC_PRELOAD;  
	            }  
	        }  
	    }  

放开注释的printk的代码,可以得到用户码,接下来在第二步 remotectl_button array我们添加的数组中填上对应的按键个数,  放开下面代码中的printk代码, 得到键值  

>        case RMC_GETDATA:  
	     {  
	          ddata->count ++;  
	          ddata->scanData <<= 1;  
	           if ((TIME_BIT1_MIN < ddata->period) && (ddata->period < TIME_BIT1_MAX)){  
	              ddata->scanData |= 0x01;  
	          }	  
	          if (ddata->count == 0x10){  
	             // printk(KERN_ERR "d=%x\n",(ddata->scanData&0xFFFF));  
	              if ((ddata->scanData&0x0ff) == ((~ddata->scanData >> 8)&0x0ff)){  
	                  if (remotectl_keycode_lookup(ddata)){  
	                       ddata->press = 1;  
	                                       ...  
	                                        }  
	                                     ...  
	                                   }  
	                                ...  
				}  
	                   }  

注意:  

**从log中获得的键值是16进制的,前两位是我们需要的,后面的是前两位的反码,用于校验,直接忽略.
不要留太多的log,获得用户码和键值后把log关闭,否能可能导致遥控器无法使用**  
