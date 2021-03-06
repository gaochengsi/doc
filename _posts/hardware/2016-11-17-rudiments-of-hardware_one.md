---
layout: post
title: 硬件设计新手入门宝典之第一部
category: 硬件
tags: 硬件
description: 微信号超硬工程师整理的入门文章，对硬件入门有帮助作用。
---
>最近看微信号超硬工程师时，看到的一篇硬件入门的文章，觉得还挺有帮助作用的，故将其整理分享到网上。

## 第一章 电路设计常用图例和标识


### 1.常用图例
电路图中经常用到一些图例表示相应的元器件,相信大家应该很熟悉。比如下图就是常见的电阻的图例:

　　　　　　　　　　　  ![2016-11-17-1](/public/img/2016-11-17-1.png)

* R201是电阻的编号。一份复杂的电路图中会用到很多的电阻电容等元件,设计电路图的软件在最后编译的时候会给元器件编号,这样在调试的时候就可以很容易找到相关元件。
* 49.9ohm 是电阻的阻值。1%是电阻的精度,表示该电阻的阻值控制在49.9 *(1+/- 1%)围内,也就是49.401~50.399ohm之间。1%精度的电阻一般用在精确控场合,比如通过电阻分压产生精确参考电压的情况一般用1%精度的。非精确控制的场合一般用低精度比如5%精度的电阻。1%精度的电阻成本上要远高于5%精度 的电阻。
* R0402表示电阻的封装尺寸型号。在高速电路板设计中,常用的是贴片焊接的电阻。电阻的封装型号表示不同尺寸大小,其对应关系如下图所示。常用的封装型号有 0402,0603,0805等等。   
  
　　　　　　　　　　　    ![2016-11-17-2](/public/img/2016-11-17-2.png)

电容的图例如下图。相关参数的意义大致与电阻类似。

  　　　　　　　　　　　  ![2016-11-17-3](/public/img/2016-11-17-3.png)

* C202表示电容的编号。
* 0.1uf表示电容容值。16V表示电容所能承受的最高电压。考虑元器件的长期可靠性,一般选择电容的实际工作电压小于其最高耐压的75%。图中的电容,其实际工作电 压应小于 16*75%=12V。
* C0201 表示其封装尺寸。与电阻类似,常用封装尺寸有0402,0603 等等。

芯片的图例就是图中间的长方块,往两边伸出很多引脚。该图列是N450CPU芯片的一部分。这样的图例通常由建元器件库的CAD工程师建立,硬件设计工程师可从元件库中调用这些元件图例来完成相关电路设计。

　　　　　　　　　　　  ![2016-11-17-4](/public/img/2016-11-17-4.png)

* U201A表示该芯片的编号。一个设计中也会有多个不同的芯片,同样设计工具在编译的时候也会给不同的芯片编号,编号通常以U开头。A表示这个图例只是该芯片的一部分。有些功能简单的芯片,只用一个图例就可以 包含所有引脚,其编号就是Uxxx。比较复杂的芯片,需要有多个图例才能列出所有引脚,这些同一芯片的不同 图例就用 A/B/C ...来区分。这里的图例是U201A,实际电路图中下一页就是 U201B,后面还有U201C, U201D, U201E。这5个图例组合在一起,就是一个完整的N450 CPU芯片的图例,包含该芯片的所有引脚。

* 一般芯片图例两边列出其引脚,如下图。图外的字母及 数字比如 AH19/AC4等表示引脚的编号;图内的字母表示相关引脚的功能,比如AH19对应的功能表述是DDR_A_MA0,也就说明这个引脚是该 CPU 内部 DDR 内存总线A通道的0号地址线(MA 表示 memory address)。右边AC4对应的功能表述是DDR_A_DQ0,也就说明这个引脚是该CPU内部DDR内存总线A通道的0号数据线。现在弄不明白这些引脚述所表示的含义没关系,随着学习的深入,会逐步了解的。只要知道芯片外面的内容是引脚编号,内部的是相关引脚功能述就可以了。

### 2.信号连接标识
一个复杂的电路图,往往有很多页电路图。很多的信号 要跨页连接,也就是要与不同页(有时候是与好几页)的信号 /引脚向连接。这在电路图上如何表示呢?
有些信号是以总线的形式连接的，如下图。

　　　　　　　　　　　  ![2016-11-17-5](/public/img/2016-11-17-5.png)

* 这是一组DDR内存总线的地址信号线。右边的部分,前面讲过是CPU芯片的图例及引脚。中间的一根根红色的引线把不同引脚连接出来,其上的字母表示该引线的net名字。比如DDR_MA0表示连接AH19引脚的引线名字叫DDR_MA0。电路图最后编译时候,会产生网表,网表中就是用这个net名字来表示芯片引脚之间的连接线。

* 这一组地址线共有15根,编号从0到14。像这样的名字相同（DDR_MA）,编号依次递增的一组信号可汇总在一起以总线（左边蓝色的粗线）的形式连接到其他页。图标‘《’就表示信号是连到其他页的。最左边的文字DDR_MA[14:0]是这组总线名,表示这组总线共有15根,各自的net名分别是DDR_MA0,DDR_MA1,...,一直到DDR_MA14。数字‘7’表示这组线连接到电路图第7页的DDR_MA[14:0]总线。总线内的每个线比如DDR_MA0
就与第7页中该总线内同名字的连接线相连。

有些信号是以单根线的形式连接的，如下图：

　　　　　　　　　　　  ![2016-11-17-6](/public/img/2016-11-17-6.png)

* 图标‘《’就表示信号连接到其他页。最左边的数字表示信号连接到的下一个/几个原件所在的页数。这里表示这些信号连接到电路第7页的电路。
* 连接线旁边的字幕表示该连接线的名字。比如对应芯片AH22引脚的连接线名字是DDR_CS0#（DDR总线的0号片选信号），连接到第7页同名的连接线。

有些信号并不连到其他页,而是连接到同一页的其他元器件或电路,如下图。

　　　　　　　　　　　  ![2016-11-17-7](/public/img/2016-11-17-7.png)

* 右上芯片AL28引脚连接线名字是DDR_REF,但该线并没有‘《’,也没有对应其他页的页数,说明该连接线 并不连接到其他页,而是和本页的同名连接线相连。
* 左下可以看到连接电阻R201和R202中间的连接线名字也是DDR_REF。虽然这根线并没有延伸连接到上述的右上部的连接线,但它们是同一个net名,就表明它们是同一根连接线,实际上就是连接在一起。
* 电阻R201上面的小圆圈及下拉的短引线是电源的标识,其上字母VCC_1V8是电源信号名,表示电源电压是 1.8V。电路图中所有页中同样电源信号名的电源信号都是连接在一起的。比如图中R201上端的电源和R205 左端的电 源连接在一起。
* 电阻R202下端的那个‘倒过来的三’一样的标识表示地。电路图中所有的同标识的地都是连在一起的。比如R202下端的地和C202下端的地是连在一起的。
图例和标识的解析就到这里。留一个问题大家思考一下:电阻的参数除了文中解释过的,经常还看到另外一个参数, 1/16W 或 1/8W 或1/4W。。。,这个参数表示什么意思呢?了解该问题答案,请继续阅读后续内容。

## 第二章 常用元件之电阻电容和电感
在一份复杂的电路原理图中,除了实现各种功能的芯片器件外,还会看到很多的通用元件比如电阻,电容和电感等等。它们具体会有哪些不同的用途和使用方法呢?这一章就针对这部分内容做些梳理和汇总。
电路图常用图例和标识一篇中最后问道一个关于电阻的问题:电阻的参数除了文中解释过的,经常还看到另外一个参数, 1/16W 或 1/8W 或 1/4W。。。,这个参数表示什么意思呢?。 我们就先来说说电阻。

### 1.电阻及使用
电阻大家都应该很熟悉了,初中就开始接触了。我们不妨 百度一下,看看具体的定义:电阻的英文名称为 resistance, 通常缩写为 R,它是导体的一种基本性质,与导体的尺寸、材料、温度有关。欧姆定律指出电压、电流和电阻三者之间的关 系为 I=U/R,亦即 R=U/I。电阻的基本单位是欧姆,用希腊字母“Ω”来表示。通常“电阻”有两重含义,一种是物理学上的“电阻”这个物理量,另一个指的是电阻这种电子元件。电阻元件 的电阻值大小一般与温度,材料,长度,还有横截面积有关,衡量电阻受温度影响大小的物理量是温度系数,其定义为温度 每升高 1°C时电阻值发生变化的百分数。电阻的主要物理特征是变电能为热能,也可说它是一个耗能元件,电流经过它就产生内能。电阻在电路中通常起分压、分流的作用。对信号来说,交流与直流信号都可以通过电阻。

不得不说,互联网还真是个好东西。以前要费很多事儿才能获得的信息,现在很快地免费地可以从网上获取。以前要学个什么东东,或者学个什么技能,必须得找好师傅,或者花大价钱找培训机构,可能学得还未必怎么样。现在网上遍布免费的知识和技术分享,只要愿意花时间钻进去,掌握一些建功立业的技能,并不是难事。

扯得有点远了,我们言归正传。百度上搜索到的对电阻的定义还是很全面的,其中到电阻是耗能元件,电流经过它 就产生内能,其物理特征是变电能为热能。也就是说电流经过时,就会发热,温度会上升。用过电炉子的人其实都知道这个 道理,电炉就是利用里面电阻丝流过电流能发热的原理。但电炉子的发热也不是可以无限增加的,一般都有个额定功率的限 定。在额定功率的范围内,电炉可以工作很长时间(质量差的假冒伪劣产品除外 )。超过这个功率会怎么样呢?电炉就会亮光一闪,然后就寿终正寝,烧断了。电阻其实也是如此,上面问题中到的电阻的一个参数1/16W或1/8W或1/4W,也就是电阻的额定功率。电阻的功率消耗W=I2R,由于 R 是比较固定的,所以设定额定功率,其实也就限定了流过的电流。如果电阻使用时超过这个这个额定功率,就会怎么样呢?会冒烟,闻到焦味,然后电阻就烧坏了。一般情况下,电阻的过电流能 力跟其封装大小也有一定关系,封装尺寸越大,允许通过电流值越大。当电阻使用在一些大功率场合或大电流通过的电路设计时,一定要考虑电阻的额定功率和允许通过电流限定。

电阻有众多不同的使用场景。这里仅就常用的几种情况做些说明。
#### 上拉电阻
 在电路设计中,电阻的使用是非常频繁的。嵌入式或计算机硬件产品的电路设计中,电阻用的最多的情况可能要算上拉电阻。这种电阻的连接一般是一端接上拉的电源(一般与芯片信号的电压值相匹配),另一端连接芯片引脚所对应的信号, 如下图的R205 所示。

　　　　　　　　　　　  ![2016-11-17-8](/public/img/2016-11-17-8.png)

上拉电阻的作用也不尽相同。有些上拉电阻是给芯片的相关引脚供初始‘高’状态;漏极开路的芯片引脚则通过上拉从而供“高”状态的驱动电流;也有些引脚的上拉电阻是根据芯片的设计要求为芯片的相关功能接口 供偏置设定等等。

#### 下拉电阻
下拉电阻的连接一般是一端接地,另一端连接芯片引脚所对应的信号。下拉电阻多是为芯片的功能接口供偏置设定或补偿设定,这些都会在芯片的设计规范中有明确说明。如下图中R305,R307;也有些下拉是为芯片的某些引脚 供‘低’初始设定。

　　　　　　　　　　　  ![2016-11-17-9](/public/img/2016-11-17-9.png)

#### 分压电阻
有些情况下,有些芯片的总线接口需要一定的参考电压,其电压精度要求一般比较高。这种参考电压一般是通过两个高 精度电阻(1%精度)的分压而产生。比如下图中R201和R202就可将1.8V电压分压到0.9V,当作DDR总线的参考电压使用。

　　　　　　　　　　　  ![2016-11-17-10](/public/img/2016-11-17-10.png)

#### 终端（termination）电阻
在高速信号设计中,有时候需要在信号的源端或终端加一些电阻进行阻抗匹配（阻抗匹配的概念请参考高速信号设计的相关技术书籍)。在源端往往是串连22-47欧姆的电阻(如下图)。

　　　　　　　　　　　  ![2016-11-17-11](/public/img/2016-11-17-11.png)

远端的阻抗匹配电阻往往像上拉电阻一样,将一定阻值的电阻上拉到终端匹配电压。如下图中所有的电阻就 DDR 总线上地址信号线的终端信号匹配电阻。

　　　　　　　　　　　  ![2016-11-17-12](/public/img/2016-11-17-12.png)

#### 电源检测电阻
电源设计中电阻的使用很复杂，不少电阻的选择与电源的补偿设计算法相关。有一类电阻是用来检测通过的电流大小的，如下图中R2305和R2301。其工作原理是检测电阻两端的电压差，除以其电阻值，芯片就可以计算出通过的电流大小。由于 希望该电阻上的压降尽量小,从而不影响电源电压的传输,所以一般这些电阻的阻值会选得很小。下图的设计中,该电阻的 值是0.027欧姆,并且用两个并联,实际最终阻值是0.0135欧姆。同时由于实际流过的电路比较大,需要选额定功率较大的电阻。该图中电阻的额定功率是0.75W,根据公司W=I2R,允许通过电流是5.27A。两个电阻和在一起共可通过的最大电流是2*5.27=10.54A,这对于电路中12v的电源输出而言，应该是有足够余量的。

　　　　　　　　　　　  ![2016-11-17-13](/public/img/2016-11-17-13.png)

### 2.电容及使用
下面再来说说电容。先来看看百度对电容的定义:是由两块金属电极之间夹一层绝缘电介质构成。当在两金属电极间加上电压时,电极上就会存储电荷,所以电容器是储能元件。任何两个彼此绝缘又相距很近的导体,组成一个电容器。平行板电容器由电容器的极板和电介质组成。
电容的特点：

* 它具有充放电特性和阻止电流通过，允许交流电流通过的能力。
* 在充电和放电过程中，两极板上的电荷有累计过程，也即电压有建立过程，因此，电容器上的电压不能突变。电容器的充电：两板分别带等量异种电荷，每个极板带电量的绝对值叫电容器的带电量。电容器的放电:电容 器两极正负电荷通过导线中和,在放电过程中导线上有 短暂的电流产生。
* 电容器的容抗与频率、容量之间成反比。即分析容抗大小时就得联系信号的频率高低、容量大小。
电容的英文名称:capacitor,它是电子设备中大量使用的电子元件之一,广泛应用于电路中的隔直通交,耦合,旁路, 滤波,调谐回路,能量转换,控制等方面。在网络上搜索一下,就会看到很多文章介绍不同电容的工作原理及作用,众说 纷纭。网络有它的好处,就是信息的传播更加高效和广泛;然而也会带来一些问题:这么多的信息,如何去伪存真,如何不 被众多说法不一的观点所困扰?看了网络上的相关分析文章后,个人觉得各有各的道理,不过各自的分析都适用于不同的应用环境,如果把那些分析拿到你自己的电路中,不一定能够套得上。就好比有些视频网站做得风生水起, 播得快的却身陷囹圄。任何事情,看法,观点都有特定的适用范围。下面结 合实际电路总结一下电容在数字硬件电路设计中的常规用途。

#### 退耦电容
退耦电容电容应该是在数字硬件电路设计中用得数量最多的一种电容。几乎所有芯片的电源和地之间都会放置一些电容,这样连接的电容叫退藕电容,其在现代硬件系统设计中非常常见。特别是在工作在高频条件下的芯片电源引脚附近,一般都会放一些退藕电容, 高相关电源的完整性设计。
现在的芯片内部集成很多晶体管。当晶体管导通时,便有电流流过;而当晶体管关闭时,电流就会断开。因而,晶体管工作时产生的电流不像传统阻性负载一样是静态的,而是形成动态的电流。当很多晶体管一起工作时,就会产生与晶体管工作频率对应的较大的动态电流(一定频率的脉冲电流)。现在的电源方案(线性电源或开关电源）都无发响应如此高频的动态电流要求,因而就通过在芯片周边靠近电源引脚的地方放一些电容(一般用陶瓷电容),来供这样的高频动态电流。如下图,当负载芯片晶体管关断时,电容被充电;当晶体管导通时,最近的退藕电容就会放电,从而产生相应的瞬态电流,保证相应芯片功能的实现。这也是退藕电容最主要的功能。当然这些电容放在芯片附近的电源通路上,也会有一定的滤波作用。如果电源通路上有一些高频的噪声,会通过这些电容而被过滤掉,从而不会进入芯片内部影响芯片的正常功能。

　　　　　　　　　　　  ![2016-11-17-14](/public/img/2016-11-17-14.png)

#### 退耦电容的选择
这其实是一个非常复杂的电源完整性分析的话题,要讲得透彻的话,需要一个专业的电源完整性或高速信号完整性设计工程师开一门课程来分析。这里只能简单定性地作些解释。前面说过,芯片内众多晶体管一起工作时,就会产生动态的跳变电流。而该动态的跳变电流流过退藕电容所形成的回路时,由于回路及退藕电容具有一定的阻抗,从而产生动态的跳变电压,反应在电压波形上就会看到高频的纹波。为了降低高频纹波,就需要控制回路及退藕电容的阻抗。控制退藕电容的阻抗,一般可以:

* 多个退耦电容并联。这有点象电阻并联可以降低阻值一样,多个退藕电容并联,也可明显降低整体阻抗,从而有效降低电源纹波。这就是为什么会在芯片附近经常有 多个 1uf 或者 0.1uf 电容并联,如下图。

　　　　　　　　　　　  ![2016-11-17-15](/public/img/2016-11-17-15.png)

* 不同值得电容并联。电容工作在高频状态下,其阻抗和工作的频率有关系。当在某个频率点电容的阻抗最低时, 这个频率叫做该电容的谐振频率。下图中,不同电容的阻抗曲线中的最低点所对应的频率就是谐振频率。一般会尽量选择谐振频率和芯片工作频率接近的电容作退藕电容,这样电容的阻抗很低。但芯片往往不会工作在一个静态的频率点上,而是工作在一个频率范围。这时就可选择不同容值的电容并联在一起,由于其有不同的谐振频率,从而从此不同的谐振频率间的频率范围内都可形成低阻抗路径。

　　　　　　　　　　　  ![2016-11-17-16](/public/img/2016-11-17-16.png)

#### 退耦电容的放置
前面到退藕电容可使用多个电容并联,那么这些电容在PCB板设计时该如何放置呢?现在大多高功率高频芯片 都有众多的电源引脚。每个电源/地引脚应该都对应芯片内部的一些晶体管。因此,退藕电容也应当分散放置在不同的电源引脚附近,尽量让每对电源/地引脚附近都有退藕电容,从而供不同部分的晶体管对工作电流的要求。退藕电容应尽量离电源/地引脚近,越近越好,这样可缩短晶体管工作形成的动态电流流过的回路,而回路越短,回路的阻抗也就越小,有利于控制电压的高频纹波。

#### 耦合电容
在高速串行差分信号中,比如在现在的电脑设计中用的很多的PCI Express总线的差分信号中往往会串接电容,这种电容根据其功能,有个名称叫交流耦合电容(ACcoupling)。比如下图中英特尔南桥芯片ICH8连接电路中,串接在PCI Express以及SATA差分信号中的所有电容都是交流耦合电容。

　　　　　　　　　　　  ![2016-11-17-17](/public/img/2016-11-17-17.png)

交流耦合电容作用主要是阻断直流分量的通过,而允许高于某个频率的高频交流分量通过。这样的电容串接在 DMI, PCIe 这样的高速差分总线中,总线传输的高速信号能够顺利通过电容,传输到接收端;而直流分量以及一些低频干扰则被过滤掉。这样做的好处:

* 可以降低共模干扰，提高传输到输入端的幸好质量
* 该电容阻断输出和输入的直流通道，便于有需要的电路在输出端和输入端设置不同的直流偏置。
至于交流耦合电容的选择，这其实是一个比较复杂的话题,需要丰富的高速信号完整性知识才能进行更深的解读。这里就暂作这些初步的解释吧。

#### 旁路电容
旁路电容(英文叫 bypass capacitor)也是大家会经常在数字电路设计听到的,其主要作用是把输入信号的一些干扰以及不需要的频率分量给过滤掉,从而让需要的频率分量以及干净的信号进入到输入电路,确保输入电路稳定工作。打个比方, 大家都知道现在空气质量PM2.5经常报表,家里都会用空气净化器。空气净化器里有过滤网,空气的原始成分顺利通过,并输出到屋子里;但空气中的一些大颗粒杂质就被过滤网给吸附了,从而起到净化空气的作用。旁路电容就好比过滤网,把信号中的干扰成分过滤到,从而净化输入信号的质量。
下面两电路中的电容C1001及C1105都是旁路电容的典型应用。其中上面的电路时通过两个电阻的分压产生一个参考电压(reference voltage)输入给南桥芯片。既然是参考电压,当然要干净没有干扰。VCC_3V3S 电源在其他的电路以及芯片供电过程中,可能会产生各种各样的干扰,C1001就可以把高频的干扰给过滤掉,净化参考电压的质量。第二个电路是产生复位信号（RTCRST#）,以复位南桥内部的相关逻辑电路。电容C1105可以起到过滤该信号电路前端电源中的各种高频干 扰,从而保证复位信号的质量。

　　　　　　　　　　　  ![2016-11-17-18](/public/img/2016-11-17-18.png)

#### 滤波电容
滤波电容,其主要作用就是利用电容充放电原理,把交流的输出波形处理成直流输出波形。在数字硬件电路设计中,用途较多的场合是开关电源输出的滤波电路以及芯片锁相环输入电源引脚的滤波电路。下图就是南桥 SATA 功能模块的锁相环输入电源的电路。电容C1222和C1216与电感L1205一起组成滤波电路。VCC_1V5 虽然已经经过其电源电路自身的滤波电路变成直流输出,但其中还是有一定的低频的纹波存在。这样的低幅度的纹波叠加到 PLL 模拟电路中,就会对相关电路的功能产生较大的影响。通过这里的滤波电路后,可以让该PLL电源引脚的输入信号(VCC_SATAPLL)更加光滑。原理很简单:当电压变化时,就需要对的滤波电容进行充放电,从而缓解电压波形的变化,让输入电压更加平稳干净。

　　　　　　　　　　　  ![2016-11-17-19](/public/img/2016-11-17-19.png)

#### 晶振起振电容
还有一种电容通常会在晶体振荡器电路中被使用,叫晶体振荡器的起振电容。该电容一般与晶振的引脚并联,其容值一般根据晶体振荡器厂家供负载电容参数选定。下图电路中 C1102/C1103就是晶体振荡器Y1101 的起振电容,它们与芯片内部的放大器合在一起就可产生等同该晶体正当频率的正弦波波形，可以作为芯片的参考时钟信号。

　　　　　　　　　　　  ![2016-11-17-20](/public/img/2016-11-17-20.png)

然电容的功能并不止这些,其他功能的电容使用不像上述几种那么广泛,以后有机会结合相关的电路进行具体解析,这里就不一一介绍。

### 3.电感及使用
电感也是常用的元器件,不过在嵌入式或计算机产品电路设计中没有电阻和电容那么广泛。百度对电感的定义:电感 器(Inductor)是能够把电能转化为磁能而存储起来的元件。电感器的结构类似于变压器,但只有一个绕组。电感器具有一定 的电感,它只阻碍电流的变化。如果电感器在没有电流通过的状态下,电路接通时它将试图阻碍电流流过它;如果电感器在有电流通过的状态下,电路断开时它将试图维持电流不变。电感器又称扼流器、电抗器、动态电抗器。
电感器的特性与电容器的特性正好相反,它具有阻止交流电通过而让直流电顺利通过的特性。直流信号通过线圈时的电阻就是导线本身的电阻压降很小;当交流信号通过线圈时,线圈两端将会产生自感电动势,自感电动势的方向与外加电压的 方向相反,阻碍交流的通过,所以电感器的特性是通直流、阻交流,频率越高,线圈阻抗越大。电感器在电路中经常和电容 器一起工作,构成LC滤波器、LC振荡器等。另外,人们还利用电感的特性,制造了阻流圈、变压器、继电器等。

* 通直流：指电感器对直流成通路状态，如果不计电感线圈的电阻,那么直流电可以“畅通无阻”地通过电感器,对直流而言,线圈本身电阻很对直流的阻碍作用很小, 所以在电路分析中往往忽略不计。
* 阻交流:当交流电通过电感线圈时电感器对交流电存在着阻碍作用,阻碍交流电的是电感线圈的感抗。

电感器在电路中主要起到滤波、振荡、延迟、陷波等作用,还有筛选信号、过滤噪声、稳定电流及抑制电磁波干扰等作用。 由于电容具有“阻直流,通交流”的特性,而电感则有“通直流,阻交流”的功能,因此电感在电路最常见的作用就是与电容一起,组成LC滤波电路。在计算机或嵌入式硬件设计中,电感使用最多的可能也就是 LC滤波电路了。很多芯片的电源输入, 特别是一些对电源质量要求很高的 PLL 电源往往都会加 LC滤波电路。比如下图是南桥ICH8的电源引脚输入。图中电感L1201/1202/1203/1204就与相关的电容组成不同的 LC滤波电路,连接到不同的电源引脚。

　　　　　　　　　　　  ![2016-11-17-21](/public/img/2016-11-17-21.png)

另外一个经常会用到电感的场合就是开关电源的设计电路中。计算机或嵌入式产品设计中,对于功率较大的电源,通常 会采用降压的开关电源设计方案。比如下图就是一个典型的开关电源电路。由于电源设计比较复杂,需要更大的篇幅和时间慢慢分析,这里我们就不花很多时间详细分析电源电路了。电路中电感L2001以及其后面的电容 C2008/C2009/C2010 等等组 成 LC 滤波整流电路,保证输出电压的稳定。电感L2001主要 利用其自身的储能功能,和阻止电流变化的特性,从而供电源电路的带负载能力。当上功率MOS管Q2002导通时,电源VCC_12V供电流,电感L2001及其后的电容就储存能量;当上管关端,下功率管Q2001导通时,电感 L2001及后面电容中储存的能量就可以释放,从而给负载电路继续供电流,保持电源电压的稳定及负载电流的供应。开关电源的细节和更多内容,今后有机会再出专题讲解。

　　　　　　　　　　　  ![2016-11-17-22](/public/img/2016-11-17-22.png)


## 第三章 二极管，三极管和MOS馆

### 1.二极管及使用
除了前面讲过的电阻，电容，电感外，在嵌入式或计算机产品硬件设计中常用的分立元器件还有二极管，三极管，MOS管。在数字电路硬件设计中，这些元件的功能相对比较单一。为了便于大家理解后面的功能电路分析，这里也用点篇幅总结一些这些‘管’元件在硬件产品数字电路设计中常用功能。首先来看看二极管。

二极管，（英语：Diode），电子元件当中，一种具有两个电极的装置，只允许电流由单一方向流过，许多的使用是应用其整流的功能。大部分二极管所具备的电流方向性我们通常称之为“整流（Rectifying）”功能。二极管最普遍的功能就是只允许电流由单一方向通过（称为顺向偏压），反向时阻断（称为逆向偏压）。

晶体二极管为一个由p型半导体和n型半导体形成的pn结，在其界面处两侧形成空间电荷层，并建有自建电场。外加正向电压时，在正向特性的起始部分，正向电压很小，不足以克服PN结内电场的阻挡作用，正向电流几乎为零，这一段称为死区。这个不能使二极管导通的正向电压称为死区电压或门坎电压。当正向电压大于死区电压以后，PN结内电场被克服，二极管正向导通，电流随电压增大而迅速上升。在正常使用的电流范围内，导通时二极管的端电压几乎维持不变，这个电压称为二极管的正向电压。门坎电压:硅管约为0.5V，锗管约为
0.1V；正向导通压降硅管约为0.6~0.8V，锗二极管约为0.2~0.3V。二极管种类有很多，按照所用的半导体材料，可分为锗二极管（Ge管）和硅二极管（Si管）。根据其不同用途，可分为检波二极管、整流二极管、稳压二极管、开关二极管、隔离二极管、肖特基二极管、发光二极管、硅功率开关二极管、旋转二极管等。下图为二极管的常用标识。

　　　　　　　　　　　  ![2016-11-17-23](/public/img/2016-11-17-23.png)

#### 1) 开关二极管
在数字电路设计中,最常用的功能是二极管的开关功能,利用其单向导电的特性。比如下图的电路就是一个例子。一个 二极管芯片 BAT54 中集成两个二极管。

　　　　　　　　　　　  ![2016-11-17-24](/public/img/2016-11-17-24.png)

该电路是计算机硬件设计中常用的 RTC 电源电路。RTC 是 real time clock,就是说这是一个一直在运行的时钟,不管 系统电源是开电还是关电,它都在运行,从而给系统供准确 实时的时钟。我们家里用的电脑,即使你关电一段时间后,再次重新开机时,里面的时间信息还是能维持最新的状态,而不 是关机时的时间,就是这个 RTC 电源及相关逻辑功能一直在起作用的结果。VCC_3V3_BAT 是电路板上一个圆形的纽扣电池所连出的电源信号。当系统关电时,该二极管芯片D1101的上面一个管就导通,从而向 VCC_RTC供电,供给RTC相关电路和逻辑功能。一般纽扣电池的供电电压是3V,前面说过锗二极管的正向导通电压为 0.2V,因此此时 VCC_RTC 电压是 3-0.2=2.8V。

大家都知道,电池的容量是有限的,如果RTC电路一直由电池供电的话,这个纽扣电池的使用寿命就比较短。因此, 当计算机开电工作时,希望由系统的主电源给这部分RTC电路供电,这样电池的容量在系统电源工作时可以保存,从而  高电池的使用寿命。那么这个 RTC 电源供电电路切换是如何 实现的呢?

当系统开电时,系统主电源就供VCC_3V3S(在系统关电时,这个电源为0V),电压是3.3V。前面说过电池供电时,VCC_RTC电压是2.8V。这样两边的电压差是0.5V,大于锗二极管的死区电压0.1V,所以D1101的下面一个二极管导通, 其导通电压为0.2V,这样VCC_RTC电压就上升到3.3-0.2=3.1V。这时,对上面一个二级管而言,一边是电池电压 3.0V,另一 边电压上升到3.1V,因此是反向电压差,根据二极管单向导通的特性,上面一个二极管就关断了。就是通过二极管的单向导通作用,实现了 VCC_RTC 供电电源的切换。

#### 2）发光二极管
在硬件设计中,另外一个在电路中经常看到的是发光二极管,利用发光二极管的发光的颜色或闪烁的频率表示不同的含义。下图是一个非常典型的发光二极管的应用电路

　　　　　　　　　　　  ![2016-11-17-25](/public/img/2016-11-17-25.png)

图中 EC_LED1 信号由系统嵌入式系统管理控制器芯片驱 动。当该信号为‘高’时,三极管 Q1812 工作于饱和导通状 态(下一节会作详细介绍),其引脚 2 和 3 直接导通,从而 发光二极管 LED1 在电源 VCC_3V3_AUX 驱动下就会正向导通, 并发光;当 EC_LED1 信号为‘低’时,三极管 Q1812 工作于 截止关断状态,其引脚2和3直接关断,从而发光二极管LED1两端没有正向压降而处于关断状态,光亮熄灭;当 EC_LED1 信号为一定频率的方波时,三极管Q1812和发光二极管LED1也就以相同的频率导通和关断,从而该二极管就以 相应的频率闪烁。系统管理者/设计者可以给这三种发光二极 管状态(常亮,常暗,闪烁)定义不同含义,从而让使用者可 以据此判断系统的工作状态。

### 2.三极管的使用
三极管,全称应为半导体三极管,也称双极型晶体管、晶体三极管,是一种控制电流的半导体器件其作用是把微弱信号放大成幅度值较大的电信号,也用作无触点开关。晶体三极管,是半导体基本元器件之一,具有电流放大作用,是电子 电路的核心元件。
晶体三极管(以下简称三极管)按材料分有两种:锗管和硅管。而每一种又有NPN和PNP两种结构形式,但使用最 多的是硅NPN和锗PNP两种三极管,其中,N表示在高纯度硅中加入磷,是指取代一些硅原子,在电压刺激下产生自由电子导电,而p是加入硼取代硅,产生大量空穴利于导电)。 两者除了电源极性不同外,其工作原理都是相同的。对于NPN 管,它是由 2 块 N 型半导体中间夹着一块 P 型半导体所组成,发射区与基区之间形成的PN结称为发射结,而集电区与 基区形成的 PN 结称为集电结,三条引线分别称为发射极 e (Emitter)、基极 b (Base)和集电极 c (Collector)。如下图所示

　　　　　　　　　　　  ![2016-11-17-26](/public/img/2016-11-17-26.png)

晶体三极管最大的特性是具有电流放大作用,其实质是三极管能以基极电流微小的变化量来控制集电极电流较大的变化 量。这是三极管最基本的和最重要的特性。我们将 ΔIc/ΔIb的比值称为晶体三极管的电流放大倍数,用符号“β”表示。电流放大倍数对于某一只三极管来说是一个定值,但随着三极管工作时基极电流的变化也会有一定的改变。
晶体三极管的工作状态有以下几种：

* 截止状态：当加在三极管发射结的电压小于PN结的导通电压,基极电流为零,集电极电流和发射极电流都为零,三极管这时失去了电流放大作用,集电极和发射极之间相当于开关的断开状态,我们称三极管处于截止状态。
* 放大状态：当加在三极管发射结的电压大于PN结的导通电压,并处于某一恰当的值时,三极管的发射结正向偏置,集电结反向偏置,这时基极电流对集电极电流起着控制作用,使三极管具有电流放大作用,其电流放大倍 数β=ΔIc/ΔIb,这时三极管处放大状态。
* 饱和导通状态：当加在三极管发射结的电压大于PN结的导通电压,并当基极电流增大到一定程度时,集电极电 流不再随着基极电流的增大而增大,而是处于某一定值附近不怎么变化,这时三极管失去电流放大作用,集电极与发射极之间的电压很小,集电极和发射极之间相当于开关的导通状态。三极管的这种状态我们称之为饱和导通状态。
三极管的放大特性在模拟电路设计中使用比较多,一般讲模拟电子的书中都会详细讲解,这就不多费笔墨了。在计 算机或嵌入式硬件设计中,一般都使用三极管的开关特性,也就是在这些电路设计中,三极管大都工作于饱和导通状态或截止状态。前面二极管部分曾经讲解过一个发光二极管电路,其二极管发光与熄灭的控制就是利用晶体三极管 Q1812 的饱和导通和截止关断的功能来实现的。该三极管以一定的频率导通和关断就产生发光二极管以同样频率闪烁的效果。
下图电路也是一个典型的利用三极管开关功能的设计。

　　　　　　　　　　　  ![2016-11-17-27](/public/img/2016-11-17-27.png)

这个电路的功能是实现信号电平的转换,这在数字硬件设计中是常用的电路。我们来简单分析一下图中三极管的作用。CPU_THERMTRIP#是CPU芯片的一个输出信号,用于过温保护。这个信号的幅度与CPU的核心电压相同,1V多一点。这个幅 值的信号不能直接输入给系统管理芯片,因为一系统管理芯片的电源是3.3V,其内部逻辑电路对输入信号幅度是有要求的,一般不低于2.0V。1V多的信号输入不能被系统管理芯片正确辨别,因此需要把这个幅度1V多的信号CPU_THERMTRIP#转换成3.3V电平的信号。这就好比你买了一部普通家用轿车, 平时以车代步,可以很好地满足要求。如果有一天你要用这部车去飙车,其性能就可能要让你大失所望了。如果要期望这部 车能够达到飙车的性能,就必须要做些改装,通过更换一些元件,从而高其动力性能,操纵性能,安全性能等等。上述电路图中两个三极管Q1803/Q1804就类似于改装车的更换元器件,经过这两个三极管的改装后,该信号就升级成3.3V信号EC_CPU_THERMTRIP#。具体工作原理如下:
* 当CPU_THERMTRIP#为高时(前面讲过,其电与为1V多一点),其信号电压就会驱动三极管Q1803产生基极流向发射极的电流,从而使Q1803 处于饱和导通状态。 Q1803 的集电极(引脚 3)与发射极(引脚 2)电压相同。 由于发射极(引脚 2)连到地,所以此时集电极(引脚 3) 的电压为 0。另一三极管 Q1804 的基极(引脚 1)与Q1803的集电极(引脚3)连接在一起,所以也是0V。这样Q1804的基极与发射极就没有压降,也就没有电流,从而Q1804处于截止关断状态。所以Q180(引脚2),也就是信号EC_CPU_THERMTRIP#此时通过电阻R1815与电源VCC_3V3S(电压 3.3V)连接,因而也是 3.3V 信号电压。

* 当CPU_THERMTRIP#为低时,Q1803的基极与发射极就没有压降,也没有电流,Q1803工作于截止关断状态。这 时 Q1804 三极管的基极(引脚 1 )通过电阻 R1814 与电 源 VCC_ 3V3S(电压 3.3V)连接。在电源VCC_3V3S的驱动作用下,Q1804产生基极流向发射极的电流,从而使Q1804处于饱和导通状态,Q1803的集电极(引脚 3)与 发射极(引脚 2)电压相同。由于发射极(引脚 2)连到 地,所以此时集电极(引脚3),也就是信号 EC_CPU_THERMTRIP#电压为 0。
综上所述，
* CPU_THERMTRIP#为高时,EC_CPU_THERMTRIP#也是高,信号电平为 3.3V。
* CPU_THERMTRIP#为低时,EC_CPU_THERMTRIP#也是低,信号电平为 0V。
* 三极管 Q1803/Q1804 的导通关断实现该信号幅度从1V 多到 3.3V 的转换
除了信号幅度的转换,三极管还可以用来设计其他很多不同功能的电路,但工作原理类似,都是利用其饱和导通和截止关断状态实现,这里就不一一详述了。

### MOS管及使用
除了上篇介绍的有电流放大作用的晶体三极管,还有另一种晶体管,叫做场效应管(FET),把输入电压的变化转化 为输出电流的变化。FET的增益等于它的transconductance,定义为输出电流的变化和输入电压变化之比。市面上常有的一般为 N 沟道和P沟道,标识见下图。

　　　　　　　　　　　  ![2016-11-17-28](/public/img/2016-11-17-28.png)

下图是典型平面 N 沟道增强型NMOSFET的剖面图。它用一块P型硅半导体材料作衬底,在其面上扩散了两个 N 型区, 再在上面覆盖一层二氧化硅(SiO2)绝缘层,最后在N区上方用腐蚀的方法做成两个孔,用金属化的方法分别在绝缘层上及两个孔内做成三个电极:G(栅极)、S(源极)及 D(漏极), 如图所示。

　　　　　　　　　　　  ![2016-11-17-29](/public/img/2016-11-17-29.png)

从图中可以看出栅极G与漏极D及源极S是绝缘的,D与S之间有两个PN结。一般情况下,衬底与源极在内部连接在 一起,这样,相当于D与S之间有一个PN结。
要使增强型N沟道MOSFET工作,要在G、S之间加正电压VGS及在D、S之间加正电压VDS,则产生正向工作电流 ID 改变 VGS 的电压可控制工作电流 ID。如下图所示。

　　　　　　　　　　　  ![2016-11-17-30](/public/img/2016-11-17-30.png)

以 NMOS 举例,MOS 管具体工作原理如下:

* 若先不接VGS(即VGS=0),在D与S极之间加一正电 压 VDS,漏极D与衬底之间的PN结处于反向,因此漏源 之间不能导电。如果在栅极G与源极S之间加一电压VGS。此时可以将栅极与衬底看作电容器的两个极板,而氧化物绝缘层作为电容器的介质。
* 当加上VGS时,在绝缘层和栅极界面上感应出正电荷,而在绝缘层和P型衬底界面上感应出负电荷。这层感应的负电荷和P型衬底中的多数载流子(空穴)的极性相反,所以称为“反型层”,这反型层有可能将漏与源的两N型区连接起来形成导电沟道。
* 当VGS电压太低时,感应出来的负电荷较少,它将被P型衬底中的空穴中和,因此在这种情况时,漏源之间仍 然无电流 ID。
* 当VGS增加到一定值时,其感应的负电荷把两个分离的 N 区沟通形成 N 沟道,这个临界电压称为开启电压(或称阈值电压、门限电压),用符号 VT 表示(一般规定在ID=10uA 时的 VGS 作为 VT)。
* 当VGS继续增大,负电荷增加,导电沟道扩大,电阻降低,ID 也随之增加,并且呈较好线性关系,如下图所示此曲线称为转换特性。因此在一定范围内可以认为,改变VGS来控制漏源之间的电阻,达到控制ID的作用。

　　　　　　　　　　　  ![2016-11-17-31](/public/img/2016-11-17-31.png)

简言之，MOS也起到类似上节晶体三极管的开关作用。

* 当VGS小于MOS的门限电压时,MOS管不导通,漏极 和源极间(D-S 间)关断;
* 当VGS大于MOS的门限电压时,MOS管导通,漏极和 源极间(D-S 间)直连,电压几乎相同。
那么,MOS 与晶体三极管的开关功能有什么不同和优势呢?

* 场效应晶体管是电压控制元件,而双极结型晶体管是电流控制元件。在只允许从取较少电流的情况下,应选用场效应管;而在信号电压较低,又允许从信号源取较多电流的条件下,应选用双极晶体管。
* 有些场效应管的源极和漏极可以互换使用,栅压也可正可负,灵活性比双极晶体管好。
* 场效应管是利用多数载流子导电,所以称之为单极型器件,而双极结型晶体管是即有多数载流子,也利用少数载流子导电。因此被称之为双极型器件。
* 场效应管能在很小电流和很低电压的条件下工作,而且它的制造工艺可以很方便地把很多场效应管集成在一块硅片上,因此场效应管在大规模集成电路中得到了广泛 的应用。

MOSFET 在 1960 年由贝尔实验室(Bell Lab.)的 D. Kahng 和 Martin Atalla首次实作成功,这种元件的操作原理 和 1947 年萧克莱(William Shockley)等人发明的双载流子 结型晶体管(Bipolar Junction Transistor,BJT)截然不同,且因为制造成本低廉与使用面积较小、高整合度的优势,在 大型集成电路(Large-Scale Integrated Circuits,LSI)或是超大型集成电路(Very Large-Scale Integrated Circuits,VLSI)的领域里,重要性远超过BJT。
在计算机/嵌入式设计中,比较常用的是 MOS 管的导通 和关断功能。下图就是一个典型的使用 MOS 管导通/关断实 现一定设计功能的电路。

　　　　　　　　　　　  ![2016-11-17-32](/public/img/2016-11-17-32.png)

这个电路是计算机系统中经常看到的主板电源开关控制电路。右边的器件J4G1是24脚的电源连接器。计算机系统里 的电源的直流输出线就是插到主板上的这个连接器里。该连接 器的第 16 脚信号名叫PS_ON#,是电源开关机的输入控制信号。当该信号为高时,电源关电;当该信号为低时,电源开电。如果没有外界的控制,该信号通过电阻 R4H8 上拉到 5V,为 高,系统处于关电状态。下面分析一下该电路工作原理:

* 当系统的开机按钮被按下时,主板的南桥芯片接受到开机按钮的信号后,就驱动一个叫S3#的信号为高,也就是 电阻 R4H3 左边的那个信号。这样 MOS 管的门极(引脚 1)也就是高,一般为 3.3V。而源极(引脚 2)的电压 在 CPU 装上后直接连到地,为 0V。故而该MOS管门极与源极的电压差为3.3V,MOS管导通,其漏极(引脚 3) 与源极电压相同,也为0V。MOS管漏极又与电源连接器第16脚PS_ON#相连,所以PS_ON#信号为低,电源开电。
* 当系统的要关电时,主板的南桥芯片就驱动S3#的信号为 低。这样 MOS 管的门极(引脚 1)也就是低,一般为0V。而源极(引脚 2)的电压在CPU装上后直接连到地,为0V。故而该MOS管门极与源极的电压差为0V,MOS管关断。MOS管漏极与电源连接器第 16 脚 PS_ON#相连, 通过 R4H8 上拉到 5V,所以 PS_ON#信号为高,电源关电。
MOS 管开关原理及应用就是这样的。当然利用该工作 原理可以设计很多不同功能的电路,包括多种多样的逻辑 门电路以及集成电路芯片,以后再慢慢讲述。


## 第四章 硬件设计功能模块框图

拿到一份原理图后,第一时间看到的往往都是该设计的 功能模块框图(英文叫 Block Diagram)。硬件设计模块框图 一般会列出设计中所用到的主要芯片及元器件,以及它们之间的连接总线,也包含设计所支持的主要功能特征。要想看懂电路的功能模块框图,就不得不先简单讲一讲硬件系统架构。功能模块框图实际可以看作硬件系统架构在某一硬件产品上的具体展示。

### 1.硬件系统架构简介

#### 1）冯诺依曼体系架构
说到硬件系统架构,必须要到经典的计算机系统架构-冯诺依曼架构。冯诺依曼体系结构的要点是:计算机中的信息 (程序和数据)以二进制方式表示。程序预存储,机器自动执行。计算机通过执行预存储在存储器中的程序来完成预定的运算。程序由计算机的指令序列构成,计算机在处理器的控制下,首先从存储器读取一条待执行的指令到处理器中,接下来分析 这条指令,而后发出该指令对应的电平脉码序列,即执行该指令。并以此递归运行程序。根据该架构体系设计的计算机系统由运算器、控制器、存储器、输入设备和输出设备五大部分组成,如下图所示。

　　　　　　　　　　　  ![2016-11-17-33](/public/img/2016-11-17-33.png)

计算机系统的五大部分(运算器、控制器、存储器、输入设备和输出设备)并不都是完全集成在一起的。一般,运算 器,控制器,内部存储器(内部缓存,英文叫 cache)集成在一个芯片,就是我们常说的中央处理器 CPU。外存储器也就是 内存(英文叫 memory)。输入输出设备是指系统和外界进行 交互作用的设备或接口,比如网络控制器和接口,键盘/鼠标,显示控制器和接口等等。这些不同的设备/芯片之间是通过总 线(英文叫 bus)连接在一起的。

上面那段话好像不是那么容易理解。打个比喻可能会更容
易弄明白。如果把CPU比作人的大脑的话,内存就好比人的记忆系统,输入输出则可看作人的其他器官比如手脚耳眼嘴巴 等等,而总线则可看作不同器官之间连接的通道比如气管,食道,神经等等。人通常会对自己的活动作些规划。举个例子,假设一个人规划上午9:30要参加一个会议。这个活动安排计划就会放在他的记忆系统里(就像硬件系统指令存放在内存 中)。快到 9:30 的时候,他的大脑会从记忆系统中获取这个会议安排的计划(就好像 CPU 从内存中读取指令)。然后 大脑开始执行这个行动计划(就像CPU执行指令),会把这个行动分解成很多子行动,比如确认并定位会议室,走到会议室,参加会议等等。在执行的过程中大脑会通过神经系统和自 己的器官进行各种信息沟通比如控制脚和腿的动作以到达会议室等。这就好比CPU执行指令的过程中会通过总线和不同的 设备进行数据交换。从这个比喻可以看出,就像人的大脑和器 官一样,计算机硬件系统的CPU,内存,输入输出是不可或缺的关键设备,是计算机系统能够正常运行的重要保证。任何一个按照冯诺依曼体系结构设计的计算机都必须包含这些关键设备。

正因为如此,在计算机硬件系统的文章或讨论中,经常 会见到这些词:CPU,内存(memory),输入输出(I/O), 总线(bus)等等。由于众多芯片公司研制这些设备,这些设 备又对应不同的品牌和型号:CPU有英特尔公司的奔腾,赛扬, 酷睿等;AMD 公司也有众多 CPU型号;除此外还有嵌入式ARM处理器,以及其它集成嵌入式处理器。内存随着技术的发展,由早期的DRAM,经历SDRAM,DDR SDRAM,DDR2 SDRAM,一直到现在的 DDR3 SDRAM(通常说的DDR,DDR2,DDR3其实都是SDRAM,只是速度和性能不断在升)。输入输出芯片就更多了,不一一列举。总线也有很多:比如内存DDR总线;连接 CPU 的前端总线(Front side bus),现在已被串行的QPI总线取代;输入输出的PCI总线,现在已被串行的PCIExpress总线取代;连接硬盘的SATA总线;连接外部USB 设备的 USB 总线等等。

#### 2)桥设备级系统架构演变
不同设备的运行速度往往不一样。比如CPU和memory的运行及传输速度都比较快。一些输入输出设备比如鼠标键盘 是靠人工手动操作的,光驱是依靠机械器件读取光盘上的信息, 它们的数据传输速度远不能和 CPU/memory 相比。为了让这 些不同运行速度的设备互不影响,就出现了多总线系统。高速设备连接到高速总线,低速设备连接到低速总线。当然不同总 线之间需要靠设备连接在一起,这样的连接不同总线的设备叫 桥(英文叫 bridge)。系统结构如下图所示。

　　　　　　　　　　　  ![2016-11-17-34](/public/img/2016-11-17-34.png)

随着硬件系统设计越来越复杂,桥不仅仅是连接高速总线和低速总线之间的设备。在高速总线多样化的时代,不同的 高速总线之间也会用桥进行连接。我们常说的南桥北桥就是桥 设备的一种。比如下图中的G2815EGMCH是北桥,82801BAICH2是南桥。G2815EGMCH北桥把连接CPU的前端系统总线,连接到内存插槽的内存总线,以及连接到显示卡的 AGP 总线 桥桥接起来。82801BA ICH2则把连接到北桥的Hub接口总线,输入输出扩展槽及网络控制器的 PCI 总线桥接起来。


自从 PC 问世以来,计算机系统的架构,CPU, 内存,显示接口,网络技术,以及各种各样的总线发生很大的变化。有些技术以及退出历史舞台,有些技术不断更新发展。

　　　　　　　　　　　  ![2016-11-17-35](/public/img/2016-11-17-35.png)

### 2.硬件设计功能模块框图
讲完硬件系统架构,相信大家对计算机的组成结构及工作原理有了大致的了解,也熟悉了相关的一些术语。下面我们就来利用前面的架构知识分析一下后面要详细分析的嵌入式X86硬件设计的模块框图。该框图如下图所示。

　　　　　　　　　　　  ![2016-11-17-36](/public/img/2016-11-17-36.png)

从这份框图可以看出,该系统其实就是一个典型的按照冯诺依曼体系架构设计的嵌入式硬件系统,运算器和控制器集成到 CPU 当中(英特尔凌动处理器 N450 或 D510);内存是一根 DDR2 SDRAM 内存(插在主板上的内存插槽中);一 个桥英特尔 ICH8M(俗称南桥);以及一些输入输出设备比如网络控制器,USB接口连接外部USB设备,显示接口连接显示设备;数据存储硬盘接口等等。

* __1__该嵌入式设计的主要元器件有基于 Atom 的 CPU N450或D510,南桥ICH8以及网络控制器。主板上还有一颗嵌入系统控制器EC,供系统管理功能,比如电源电压监控, 温度监控,风扇速度调控等等。
* __2__整个系统用到的总线:CPU 连接内存插槽的 DDR2 总线; 处理器通过 DMI 总线和 ICH8(南桥)连接;南桥ICH8通过PCI Express总线和网络芯片82574相连;网络phy82567则通过GLCI总线与南桥内部的网络MAC控制器连接。此外还有连接硬盘的 SATA 总线,连接一些外设 的 USB 总线,显示总线 VGA 等等。
* __3__该嵌入式设计主板上供的主要功能接口有:显示用途的VGA/LVDS接口(连接到CPU处理器);总共的 6个SATA 硬盘接口(连接到南桥ICH8);PCI Express扩展槽以及多个USB 接口(连接到南桥ICH8);两个千兆网络接口(连接到相关的网络控制器)。系统中也包含一根DDR2的SODIMM内存插槽,连接到处理器。
* __4__南桥 ICH8 还引伸出 LPC 总线以及声音信号接口。LPC 总 线可用来连接外部的LPC设备,比如 Super IO 芯片以及 显示 80 口诊断码的 PLD 等等。
* __5__框图右上角的注释表明系统支持两种不同的处理器,而且两种不同的处理器需要不同的电源设计:D510处理器需要符合VR11标准的电源方案;而如果用的是N450处理器,主板上需要启用IMVP6的电源方案。VR11和IMVP6是降压直流开关电源的方案的不同类型,其工作原理都是开关电源的工作原理,只不过所支持的性能效率等有所区别。

















