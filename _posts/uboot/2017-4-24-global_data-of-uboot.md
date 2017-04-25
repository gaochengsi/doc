---
layout: post
title: uboot global_data 介绍
category: Uboot
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

```c++
typedef struct global_data {
	bd_t *bd;
	unsigned long flags;
	unsigned baudrate;
	unsigned long cpu_clk;	/* CPU clock in Hz!		*/
	unsigned long bus_clk;
	/* We cannot bracket this with CONFIG_PCI due to mpc5xxx */
	unsigned long pci_clk;
	unsigned long mem_clk;
	unsigned long have_console;	/* serial_init() was called */
	unsigned long env_addr;	/* Address  of Environment struct */
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

```c++
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
```

* unsigned long have_console:是否已经调用初始化串口函数，可以使用串口
* unsigned long env_addr：环境变量的地址。
* unsigned long ram_top：RAM空间的顶端地址
* unsigned long relocaddr：UBOOT重定向后地址
* phys_size_t ram_size：物理ram的size
* unsigned long irq_sp：中断的堆栈地址
* unsigned long start_addr_sp：堆栈地址
* unsigned long reloc_off：uboot的relocation的偏移
* struct global_data *new_gd：重定向后的struct global_data结构体
* const void *fdt_blob：我们设备的dtb地址
* void *new_fdt：relocation之后的dtb地址
* unsigned long fdt_size：dtb的长度
* unsigned long malloc_base : malloc 的起始位置
* struct udevice *cur_serial_dev：当前使用的串口设备

## 4.global_data的分配和初始化

### global_data的空间分配

代码位于common/init/board_init.c中：

```c++
//传入的top为起始分配地址，由上而下分配，传入的地址不一定为内存顶部地址，函数在relocation之前调用
ulong board_init_f_alloc_reserve(ulong top)
{
	/* Reserve early malloc arena */
#if defined(CONFIG_SYS_MALLOC_F)
	//从顶部向下分配一块大小为CONFIG_SYS_MALLOC_F_LEN的区域给malloc
	// 关于CONFIG_SYS_MALLOC_F_LEN可以参考README
	// 这块内存是用于在relocation前用于给malloc函数提供内存池。
	top -= CONFIG_SYS_MALLOC_F_LEN;
#endif
	/* LAST : reserve GD (rounded up to a multiple of 16 bytes) */
	// 继续向下分配sizeof(struct global_data)大小的内存给global_data使用，向下16byte对齐
	// 这时候得到的地址就是global_data的地址。
	top = rounddown(top-sizeof(struct global_data), 16);
	//返回，也就是global_data的地址，真正的分配是在返回后调整指针位置
	return top;
}
```

### global_data的初始化

代码位于common/init/board_init.c中：

```c
// 这个函数用于对global_data区域进行初始化，也就是清空global_data区域
// 传入的参数就是global_data的基地址
void board_init_f_init_reserve(ulong base)
{
    struct global_data *gd_ptr;

    /*
     * clear GD entirely and set it up.
     * Use gd_ptr, as gd may not be properly set yet.
     */

    gd_ptr = (struct global_data *)base;
    /* zero the area */
    memset(gd_ptr, '\0', sizeof(*gd));
// 先通过memset函数对global_data数据结构进行清零

    /* next alloc will be higher by one GD plus 16-byte alignment */
    base += roundup(sizeof(struct global_data), 16);
// 因为global_data区域是16Byte对齐的，对齐后，后面的地址就是early malloc的内存池的地址，具体参考上述board_init_f_alloc_reserve
// 所以这里就获取了early malloc的内存池的地址

    /*
     * record early malloc arena start.
     * Use gd as it is now properly set for all architectures.
     */
#if defined(CONFIG_SYS_MALLOC_F)
    /* go down one 'early malloc arena' */
    gd->malloc_base = base;
// 将内存池的地址写入到gd->malloc_base中
    /* next alloc will be higher by one 'early malloc arena' size */
    base += CONFIG_SYS_MALLOC_F_LEN;
//加上CONFIG_SYS_MALLOC_F_LEN，获取early malloc的内存池的末尾地址，这里并没有什么作用，是为了以后在early malloc的内存池后面多加一个区域时的修改方便。
#endif
}
```

### 如何调用分配和初始化函数

代码如下，去除掉被宏定义包含的无关代码部分，位于arch/arm/lib/crt0.S：

```c++
ENTRY(_main)
/*
 * Set up initial C runtime environment and call board_init_f(0).
 */
    ldr sp, =(CONFIG_SYS_INIT_SP_ADDR)
@@ 预设堆栈指针为CONFIG_SYS_INIT_SP_ADDR
@@ 在warp7中初步设置为如下（include/configs/warp7.h）：
@@ #define CONFIG_SYS_INIT_SP_ADDR (CONFIG_SYS_INIT_RAM_ADDR + CONFIG_SYS_INIT_SP_OFFSET)
@@ #define CONFIG_SYS_INIT_RAM_ADDR	IRAM_BASE_ADDR
@@ #define IRAM_BASE_ADDR                  OCRAM_ARB_BASE_ADDR
@@ #define OCRAM_ARB_BASE_ADDR             0x00900000
@@ #define CONFIG_SYS_INIT_SP_OFFSET (CONFIG_SYS_INIT_RAM_SIZE - GENERATED_GBL_DATA_SIZE)
@@ #define CONFIG_SYS_INIT_RAM_SIZE	IRAM_SIZE
@@ #define IRAM_SIZE			0x00020000
@@ #define GENERATED_GBL_DATA_SIZE 256
@@ 最终可以得到CONFIG_SYS_INIT_SP_ADDR是0x00900000+0x00020000-256=0x91FF00

@@ 注意！！！这里只是预设的堆栈地址，而不是最终的堆栈地址！！！

    bic sp, sp, #7  /* 8-byte alignment for ABI compliance */
@@ 低三位置0，8byte对齐

    mov r0, sp
    bl  board_init_f_alloc_reserve
@@ 将sp的值放到r0中，也就是作为board_init_f_alloc_reserve的参数
@@ 返回之后，r0里面存放的是global_data的地址
@@ 注意，同时也是堆栈地址，因为堆栈是向下增长的，所以不必担心和global_data冲突的问题

@@ 综上，此时r0存放的，既是global_data的地址，也是堆栈的地址

    mov sp, r0
@@ 把堆栈地址r0存放到sp中

    /* set up gd here, outside any C code */
    mov r9, r0
@@ 把global_data的地址存放在r9中
@@ 此时r0存放的还是global_data的地址

    bl  board_init_f_init_reserve
@@ 调用board_init_f_init_reserve对global_data进行初始化，r0也就是其参数。
```
 __注意__：最终global_data的地址存放在r9中了。

## 5.分配和初始化global_data之后，内存的分布如下：
--------------------------------------------CONFIG_SYS_INIT_SP_ADDR:0x91FF00  
				early malloc arena  
--------------------------------------------malloc base  
				global_data  
--------------------------------------------global_data基地址(r9)，也是堆栈起始位置  
				堆栈空间  
--------------------------------------------堆栈结束  

## 6.global_data如何使用

前面我们一直强调了global_data的地址存放在r9中了。所以当我们需要global_data的时候，直接从r9寄存器中获取其地址即可。

uboot中定义了一个宏DECLARE_GLOBAL_DATA_PTR，使我们可以更加简单地获取global_data。

代码位于：arch/arm/include/asm/global_data.h
```c
#define DECLARE_GLOBAL_DATA_PTR		register volatile gd_t *gd asm ("r9")
```


