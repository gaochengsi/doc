---
layout: post
title: uboot global_data 介绍
category: uboot
tags: uboot,global_data,gd
description: 介绍uboot的gloabl_data。
---

## 1.global_data 介绍

global_data 在u-boot中是一个十分重要的数据结构，它主要存放了一些全局变量，用于在u-boot启动时候传递一些数据，这里介绍的内容为在u-boot重定位之前，对global_data分配，初始化的一些操作。
此时的global_data位于刚上电之后就能使用的储存设备上，比如imx7芯片的OCRAM。
在最后，u-boot会将global_data数据传递给Kernel使用。

## 2.global_data 数据结构

本文以imx7芯片为例，使用了Freescale的u-boot分支作为讲解。

global_data的定义位于 include\asm-generic\global_data.h中，使用typedef struct global_data结构体来定义了gd_t，所以可以使用gd_t来进行定义。
具体代码如下，根据使用的芯片，去掉了一些没使用的宏定义。

```c
typedef struct global_data {
	bd_t *bd;
	unsigned long flags;
	unsigned include/asm-arm/u-boot.hint baudrate;
	unsigned long cpu_clk;	/* CPU clock in Hz!		*/
	unsigned long bus_clk;
	/* We cannot bracket this with CONFIG_PCI due to mpc5xxx */
	unsigned long pci_clk;
	unsigned long mem_clk;
	unsigned long have_console;	/* serial_init() was called */
	include/asm-arm/u-boot.hunsigned long env_addr;	/* Address  of Environment struct */
	unsigned long env_valid;	/* Checksum of Environment valid? */
	unsigned long ram_top;	/* Top address of RAM used by U-Boot */
	unsigned long relocaddr;	/* Start address of U-Boot in RAM */
	phys_size_t ram_size;	/* RAM size */
	unsigned long mon_len;	/* monitor len */
	unsigned long irq_sp;		/* irq stack pointer */
	unsigned long start_addr_sp;	/* start_addr_stackpointer */
	unsigned long reloc_off;
	struct global_data *new_gd;	/* relocated global data */
#ifdef CONFIG_DM
	struct udevice	*dm_root;	/* Root instance for Driver Model */
	struct udevice	*dm_root_f;	/* Pre-relocation root instance */
	struct list_head uclass_root;	/* Head of core tree */
#endif
	const void *fdt_blob;	/* Our device tree, NULL if none */
	void *new_fdt;		/* Relocated FDT */
	unsigned long fdt_size;	/* Space reserved for relocated FDT */
	struct jt_funcs *jt;		/* jump table */
	char env_buf[32];	/* buffer for getenv() before reloc. */
#if defined(CONFIG_SYS_I2C)
	int		cur_i2c_bus;	/* current used i2c bus */
#endif
#ifdef CONFIG_SYS_I2C_MXC
	void *srdata[10];
#endif
	unsigned long timebase_h;
	unsigned long timebase_l;
#ifdef CONFIG_SYS_MALLOC_F_LEN
	unsigned long malloc_base;	/* base address of early malloc() */
	unsigned long malloc_limit;	/* limit address */
	unsigned long malloc_ptr;	/* current address */
#endif
	struct udevice *cur_serial_dev;	/* current serial device */
	struct arch_global_data arch;	/* architecture-specific data */
} gd_t;
```

## 3.成员说明

* bd_t *bd,开发板的一些信息，位于include/asm-arm/u-boot.h中，由u-boot传递给Kernel,具体代码如下，已去掉无用宏定义段。

	```c
	typedef struct bd_info {
	unsigned long	bi_memstart;	/* start of DRAM memory */
	phys_size_t	bi_memsize;	/* size	 of DRAM memory in bytes */
	unsigned long	bi_flashstart;	/* start of FLASH memory */
	unsigned long	bi_flashsize;	/* size	 of FLASH memory */
	unsigned long	bi_flashoffset; /* reserved area for startup monitor */
	unsigned long	bi_sramstart;	/* start of SRAM memory */
	unsigned long	bi_sramsize;	/* size	 of SRAM memory */
	unsigned long	bi_arm_freq; /* arm frequency */
	unsigned long	bi_dsp_freq; /* dsp core frequency */
	unsigned long	bi_ddr_freq; /* ddr frequency */
	ulong	        bi_arch_number;	/* unique id for this board */
	ulong	        bi_boot_params;	/* where this board expects params */
	#ifdef CONFIG_NR_DRAM_BANKS
	struct {			/* RAM configuration */
		phys_addr_t start;
		phys_size_t size;
	} bi_dram[CONFIG_NR_DRAM_BANKS];
	#endif /* CONFIG_NR_DRAM_BANKS */
	} bd_t;

