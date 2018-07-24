
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

---
layout: post
title: 堆溢出+ROP
category: 信息安全
tags: 堆溢出,ROP
---
>体系结构安全课程的一个大作业，利用了堆溢出攻击和ROP，实验平台为64位，ubuntu16.04,ALSR打开，栈溢出保护打开。

## 主要思想和结果
主要利用了 The House of Force；通过堆溢出，改写 top chunk 的size，然后再通过分配设定size 的 heap，使 top chunk 跳转到 free_got – 0x10，然后再进行 malloc 适当大小的heap，返回的堆地址就是free_got，通过往堆里写内容，从而改写free_got，达到劫持控制流。最终结果是程序调用 free，返回一个 shell。

$$F(\omega) = \frac{1}{\sqrt{2\pi}} \int_{-\infty}^{\infty} f(t) \, e^{ - i \omega t}dt$$

## 背景知识
从04 年开始，glibc中的malloc进行了改进，对size进行了大小检查，以及对unlik时p->bk->fd和p->fd->bk是否和p相等进行了检查，所以这之后与原先unlink的使用已经废弃，一时找不到攻击的方法了，直到05年末， Phantasmal Phantasmagoria公布了一些新的技巧：

* House of Prime  
* House of Mind  
* House of Force  
* House of Lore  
* House of Spirit

但在后来不断的改进中，目前在这5中的攻击方法中，现在主要还能用的还有The House of Force和The House of Spirit，本文主要用了The House of Force，所以只介绍该方法。
The House of Force，这个技巧中，攻击者通过堆溢出修改 top chunk的大小，并欺骗 glibc malloc 使用 top 块来服务于一个非常大的内存请求（大于堆系统内存大小）。现在当新的 malloc 请求产生时，就会返回攻击地址，比如free的GOT，因此可以覆盖为shellcode，当程序调用free时，就会执行shellcode。House of Force成功执行至少需要三个malloc调用：

• Malloc 1：用于初始化堆，分配的这个快放在top chunk之下，用于堆溢出，改写top chunk的大小。    
• Malloc 2：攻击者应该可以控制该malloc请求的大小，使得top chunk跳转到相应的地址。   
• Malloc 3：返回被攻击地址，攻击者用于输入shellcode，实现攻击。    

具体流程在堆中的情况如下：

<div align="CENTER">
    <img src="/public/img/2018-7-23-1.png" alt="2018-7-23-1.png"/>
    <p>阶段一：malloc1，以及堆溢出改写top chunk size</p>
    <img src="/public/img/2018-7-23-2.png" alt="2018-7-23-2.png"/>
    <p>阶段二 malloc2 使top chunk跳转到目的地址</p>
    <img src="/public/img/2018-7-23-3.png" alt="2018-7-23-3.png"/>
    <p>阶段三：malloc3 分配适当大小，返回target地址，写入shellcode</p>
</div>


## 实验过程

### 漏洞程序
在漏洞程序中，为了满足了以上的需求，设计了三次malloc，且第二次和第三次允许用户定义长度，此外，为了能实现打开一个shell，在漏洞程序中使用了栈用于存放参数，并且在漏洞程序中添加了一段用于辅助ROP的ROPgadget，具体代码见heap_overflow.c:
漏洞程序的安全选项：

<div align="CENTER">
    <img src="/public/img/2018-7-23-4.png" alt="2018-7-23-4.png"/>
</div>

实验环境：ubuntu16.04 + ASLR ON

### 攻击过程
阶段一：
运行漏洞程序，首先会要求输入name，然后是log，name和log都是存放在栈上的数组，之后，程序就会分配第一个heap p0，并将得到的返回地址拷贝到p0_a的一个数组中，然后会将name中的数据打印出来，并要求往p0中写入内容，分析漏洞程序的反汇编，得到栈上的空间顺序分配如下：

<div align="CENTER">
    <img src="/public/img/2018-7-23-5.png" alt="2018-7-23-5.png"/>
</div>

由图可知，所以为了能够得到p0的地址，可以通过对name进行溢出，然后在之后程序打印name中的中的数据时，便可以得到p0的地址，通过反汇编，得到在栈中name到p0_a的实际长度为208，所以往name中输入了208个字节，然后当漏洞程序打印出name中的内容时，就会由于过读，将p0的地址也返回，通过p0的地址，便可以计算出top chunk的头地址ptr_top，计算方法是p0地址 + p0长度，得到的地址是top chunk头中pre_size的地址，在漏洞程序中p0的长度为256字节，pre_size所占空间为8字节。
为了能够实现在malloc2中使用top chunk分配堆块，使top chunk跳转到指定位置，需要在这之前将top chunk的大小修改到足够大，原因是因为如果top chunk的size不是足够大，那么在malloc2中，当申请的size大于top chunk的size时，就会调用mmap，而不会从top chunk中分配，因此达不到目的，所以为了修改top chunk的size，可以通过溢出p0，修改top chunk的size字段，前面已经得到p0地址到top chunk的pre_size的长度，所以构造输入用于溢出修改top chunk的size:

<div align="CENTER">
    <img src="/public/img/2018-7-23-6.png" alt="2018-7-23-6.png"/>
</div>

阶段二：
由阶段一已经得到了top chunk的地址ptr_top，并且通过溢出修改了top chunk的size字段，于是在漏洞程序要求输入p1大小时，通过输入适当evil_size，便可以将top chunk跳转到指定的地址，在本次攻击中，选定的攻击地址是free_got，用于实现劫持free函数，由于堆块head的存在，所以需要在分配完evil_size之后，ptr_top能指向free_got – 0x10的地方，这样才能在第三次malloc返回时，p2指向的就是free_got的地址，计算过程如下：

<div align="CENTER">
    <img src="/public/img/2018-7-23-7.png" alt="2018-7-23-7.png"/>
</div>

然后将得到的evil_size用于请求malloc大小，并输入任意内容到得到的堆中。
阶段三：
此时请求合适大小的堆块，然后漏洞程序就会调用malloc函数分配合适大小，然后将分配的堆栈地址返回给p2，等待用户输入，此时返回的p2就是目的地址，在本文中为free_got的内容，需要注意的是由于malloc的对齐问题，所以存在返回的p2是free_got-0x8，此时往p2中写入内容，覆盖原有free_got达到劫持free函数的目的。
通过前文的方法，可以实现劫持free函数，为了实现新建一个shell，需要能够控制漏洞程序执行system(“/bin/sh”)，由于ubuntu 16.04是x64架构，/bin/sh这个参数是通过寄存器rdx传递的，所以需要解决的问题是参数传递，以及获取system函数地址。

参数传递：
为了解决参数传递的问题，可以通过栈来完成，现将需要的参数压入栈中，然后通过ROPgadget将参数pop到指定寄存器中，由前文漏洞程序在栈中的分布可以知道，log数组是离栈顶最近的，如果可以通过一段连续pop或者rsp增加的代码，则可以让rsp移动到log数组中，从而达到控制参数传递的目的，通过汇编的查找，在__libc_csu_init中发现了一段符合要求的ROP代码：

<div align="CENTER">
    <img src="/public/img/2018-7-23-8.png" alt="2018-7-23-8.png"/>
</div>

从0x400986开始，rsp一直在增加，经过计算，发现ret指令执行时rsp刚好指向log数组的开始，于是将合适ROPgadget地址写入到log数组开始的位置，然后就执行完这段代码就可以跳转到ROPgadget去执行，通过ROPgadget将参数传递到寄存器中，于是完成了参数传递，因此需要往p2中写入这段代码的地址。

获取地址：
由于漏洞程序中没有使用system函数，因此是没有system_plt的地址的，无法直接跳转到system_plt执行，而且由于系统开启了ASLR，所以无法确定libc加载地址，但是由于libc中各个函数之间的相对位置是不变的，所以一旦知道了libc中某一个函数的加载地址，就能通过偏移量求得system的地址，本文的方法是通过泄露gets函数的地址，从而得到system地址，然后再将system地址写入到free_got中，再跳转到free_plt去执行，从而达到目的，在这个过程中，需要借助函数输出gets_got的内容以及输入system的地址，分析漏洞程序可知，通过gets和puts两个函数来输出和输入，分析整个流程可知，攻击的顺序是控制漏洞通过puts函数输出gets_got内容，然后攻击程序由此计算出system地址，然后再控制漏洞程序接收system地址，用于覆盖free_got，再次调用free_plt，攻击完成，为了实现不断的控制执行流程，需要借助ROPgadget，在漏洞程序中为了方便，写入了一个ROPgadget：

<div align="CENTER">
    <img src="/public/img/2018-7-23-9.png" alt="2018-7-23-9.png"/>
</div>

这个ROPgadget的主要功能是一个循环劫持控制流，通过前文的log，就可以达到不断调用相应的函数，所以在前文输入log时，就需要将设计好的payload输入。具体如下：

<div align="CENTER">
    <img src="/public/img/2018-7-23-10.png" alt="2018-7-23-10.png"/>
</div>

前文提到计算system的地址，方法是首先确定gets在libc中偏移的地址，先使用命令ldd 获取libc位置：

<div align="CENTER">
    <img src="/public/img/2018-7-23-11.png" alt="2018-7-23-11.png"/>
</div>

在使用readelf -s /lib/x86_64-linux-gnu/libc.so.6 | grep gets得到gets偏移量：

<div align="CENTER">
    <img src="/public/img/2018-7-23-12.png" alt="2018-7-23-12.png"/>
</div>

得到0x6ed80就是gets在libc中的偏移地址，同样得到system偏移量：

<div align="CENTER">
    <img src="/public/img/2018-7-23-13.png" alt="2018-7-23-13.png"/>
</div>

所以得到计算方法：

<div align="CENTER">
    <img src="/public/img/2018-7-23-14.png" alt="2018-7-23-14.png"/>
</div>

此外，由于system需要参数/bin/sh，因此需要将这个字符串写入到漏洞程序的某个地址，这个地址必须是漏洞程序的可写地址，使用命令objdump -h heap_overflow：

<div align="CENTER">
    <img src="/public/img/2018-7-23-15.png" alt="2018-7-23-15.png"/>
</div>

可以看到没有READONLY标识的就是可写入地址，本文选取了设置writeable = 0x601060。
    至此，所有的攻击流程都已讲完，完整代码见附件，需要注意的就是一些格式转换等小问题。

### 攻击程序依赖
攻击程序采用了python2，主要的依赖库有pwntools，struct

### 攻击结果
运行hack_heap.py，得到：

<div align="CENTER">
    <img src="/public/img/2018-7-23-16.png" alt="2018-7-23-16.png"/>
</div>

攻击成功，获得shell。