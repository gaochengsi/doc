---
layout: post
title: An In-Depth Look into the Win32 Portable Executable File Format
category: 操作系统
tags: PE，Win32，exe，dll
description: PE文件介绍
---
>对原文的一个翻译，[原文地址](https://msdn.microsoft.com/en-us/magazine/bb985992.aspx)，其中有些翻译可能不是很准确，欢迎大家指正。

作者:Matt Pietrek  
本文假设你对C++和Win32平台熟悉

## 总结
对可移植执行体文件（PE）形式的好的一个理解会使得对操作系统有一个好的理解。如果你知道你的DLL和EXE文件中有些什么，那么你将是一个更有见识的程序员。这篇文章，也是两篇连载中的第一篇，着眼于在过去的几年间PE文件格式发生的改变，以及对其格式的一个概述。

这次更新之后，作者讨论了PE格式是如何满足.NET应用，PE文件部分，RVAs，数据路径，以及函数的引入。附录包含了一份相关镜像头结构和他们描述的清单。


## 正文
很久之前，在遥远的银河系，我为Microsoft Systems Journal (现在是MSDN® Magazine)写了我最早几篇文章中的一篇：["Peering Inside the PE: A Tour of the Win32 Portable Executable File Format,"](https://msdn.microsoft.com/en-us/magazine/ms809762.aspx),结果表民，这篇文章比我现象的更加受欢迎。至今，我任然能够从人们的口中听到）甚至在微软）在使用这篇文章，且它仍能从MSDN的数据库中获得。不幸的时，这篇文章的问题是它是一成不变的。Win32的世界在过去几年中已经改变了相当很多，但这个文章的却是严重过时的。从这个月开始，我将通过一篇两部分的文章来改善这个情况。

你或许会好奇你为什么应该关注可执行文件的格式。回答正如之前一样：操作系统的可执行文件格式和数据结构揭示了相当多的底层操作系统的信息。通过了解你的EXE和DLL文件中有什么，你将会发现你已经变成了一个更好的程序员。




















