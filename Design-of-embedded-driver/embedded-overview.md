# 嵌入式概述

## 课程目标

* Linux驱动程序的基本组成和结构
* 驱动程序的编写的基本原理和方法
* 一些常见的注意问题
* 掌握和了解驱动程序编写的特殊要求
* 对计算机整体体系结构有足够的了解和认识
* 对Linux的内核有一定的认识
* 驱动程序大致上有一定的结构，但是需要考虑的各方面的细节非常多，需要对软件和硬件有一定的认识

## 需要了解的知识：

* 内核相关

  内核处理系统调用，根据文件设备类型、主设备号、从设备号，调用设备驱动程序

* 文件系统相关

* 应用程序、库、内核、驱动程序的关系

* 设备类型

* 设备文件、主设备号与从设备号

* 驱动程序与应用程序的区别

* 用户态与内核态

  1. 驱动程序工作在内核态
  2. 应用程序工作在用户态

* Linux驱动程序功能

## Linux内核

1. 多任务管理
2. 文件系统管理
3. 设备管理
4. 内存管理
5. 网络管理

## linux设备类型

1. 字符设备[^1]

   [^1]: 是个能够像字节流一样被访问的设备

2. 块设备[^2]

   [^2]: 通过传输固定大小的数据来访问设备

3. 网络设备[^3]

   [^3]: 也就是一个能够和其他主机交换数据的设备

## linux设备管理

只通过设备类型来管理设备是不可能的，需要引入设备号的概念。

* 主设备号：标识同一驱动程序
* 从设备号：标识同一驱动程序的不同硬件设备

## Linux驱动开发-模块

模块特点：

1. 本身并不被编译进内核，从而控制了内核的大小模块一旦被加载，就和内核中的其他部分都一样

2. 需要注意的是，内核并不一定是模块，而模块儿也不一定是内核




**应用程序、库、内核、驱动的关系：**

1. 应用程序通过调用函数库，通过对文件的操作实现一系列的功能[^注释：]。  

[^注释：]: 应用程序以文件的方式访问各种硬件设备，这是Linux特有的抽象方式，把所有的硬件访问抽象成为对文件的读写，控制。

2. 函数库大致可以分为两部分，一种是涉及到硬件操作或内核的支持，有内核完成对应功能，称其为系统调用，另一种由库函数内部通过代码实现，直接完成功能。
3. 驱动程序是内核的一部分，但是内核不一定完全就是驱动程序。内核处理系统调用，根据设备文件类型，主设备号、从设备号，调用设备驱动程序。

**驱动程序的功能**

1. 对设备初始化和释放资源
2. 把数据从内核传送到硬件和从硬件读取数据
3. 读取应用程序传送给设备文件的数据和回送应用程序请求的数据
4. 检测和处理设备出现的错误(底层协议)

## linux驱动开发：

Linux内核整体结构非常庞大，其包含的组件也非常多。

有两种解决方案：

1. 将所有的驱动程序编译到Linux内核中去[^缺点]

[^缺点]: 内核庞大

2. 动态加载内核模块儿(Linux所使用的方案)[^特点]

   [^特点]: 编译出的内核本身并不具有所有功能，在这些功能需要被使用到的时候，其对应的代码可以被动态加载到内核中。

**模块儿**

1. 模块本身并不被编译入内核，从而控制了内核的大小。
2. 模块一旦被加载，他就和内核中的其他部分完全一样。
3. 注意：模块并不是驱动的必要形式：即：驱动不一定必须是模块，有些驱动是直接编译进内核的；同时模块也不全是驱动，例如我们写的一些很小的算法可以作为模块编译进内核，但它并不是驱动。

# 嵌入式驱动程序概述（二）

并发和并行：

并发：单核CPU，多个程序表面上同时运行，但任一时刻只有一个程序在处理机上运行。

并行：多核CPU，真正意义上的同时运行，同一时刻，不同程序运行在不同的CPU上。
