---
title: 【linux】进程
date: 2023-07-15 12:19:45
tags:
- linux
- 进程
categories:
- linux
cover: https://tuchuang-1317757279.cos.ap-chengdu.myqcloud.com/linux.jpg
ai: true 


---

# 操作系统
在了解进程前，还得介绍一下操作系统。

## 概念
操作系统是计算机系统中的一种重要软件，它是计算机`硬件和软件`之间的桥梁，负责管理计算机系统的各种资源，如CPU、内存、输入输出设备等。操作系统可以被看作是计算机系统的管理者，它控制和协调计算机系统中各个部件的工作，使得应用程序能够正确地运行。

操作系统通常包括以下几个组成部分：

1. 内存管理：负责管理计算机系统的内存资源，包括内存的分配、释放和保护等。
2. `进程管理`：负责管理计算机系统中的进程，包括进程的创建、调度、同步和通信等。
3. 文件系统：负责管理计算机系统中的文件和目录，包括文件的读写、创建、删除和保护等。
4. 输入输出管理：负责管理计算机系统中的输入输出设备，包括输入输出的缓存、设备的分配和释放等。

维基百科这样总结操作系统：`操作系统`（英语：Operating System，缩写：OS）是一组`主管并控制`计算机操作、运用和运行硬件、软件资源和提供公共服务来组织用户交互的相互关联的系统软件程序，同时也是计算机系统的`内核`与基石。操作系统需要处理如管理与配置内存、决定系统资源供需的优先次序、控制输入与输出设备、操作网络与管理文件系统等基本事务。操作系统也提供一个让用户与系统交互的操作界面。
操作系统的类型非常多样，不同机器安装的操作系统可从简单到复杂，可从移动电话的嵌入式系统到超级电脑的大型操作系统。许多操作系统制造者对它涵盖范畴的定义也不尽一致，例如有些操作系统集成了`图形用户界面`，而有些仅使用`命令行界面`，将图形用户界面视为一种非必要的应用程序。
<img src="../linux/../photo/linux/进程1.png">
那么总的概括下来呢，计算机系统中都包含且存在的一个基本的`程序集合`，就被称为操作系统。其根本目的就在于为用户`提供一个相对方便的操作环境，管理计算机的软硬件资源`。

## 发展

在20世纪40年代初期，计算机系统还处于非常初级的阶段，计算机的应用也非常有限。直到1945年，约翰·冯·诺伊曼提出了`冯洛伊曼体系`，这一体系彻底改变了计算机系统的结构和设计，开创了计算机技术的新时代。

随着计算机技术的不断发展，计算机系统的规模和复杂度也不断增加，操作系统的概念也随之出现。20世纪60年代初期，IBM公司发布了第一款商用操作系统——OS/360，这标志着操作系统开始进入商用化阶段。

从此以后，操作系统和冯洛伊曼体系的发展就开始了新的篇章。随着计算机技术的不断进步，操作系统和冯洛伊曼体系也不断更新和演化，逐步适应了现代计算机系统的各种应用场景和技术需求。
<img src="../linux/../photo/linux/进程2.png">[冯诺依曼体系结构设计概念]

## 内存
我们如今所能使用的`大部分计算机`，都遵循这冯诺依曼体系。但冯诺依曼结构并非这一节的重点，我们只需要知道，memory指的是存储器，也就是常谈的`内存`！

>所有程序都是必须放到内存中去进行的！

这是因为计算机的`CPU只能直接访问内存中的数据和指令`，而无法直接访问硬盘等外部存储设备中的数据和指令。因此，为了使程序能够被CPU执行，必须先将程序加载到内存中。
当程序被加载到内存中后，CPU可以通过`内存地址`来访问程序的指令和数据。而内存的访问速度比硬盘等外部存储设备的访问速度要快很多，所以将程序加载到内存中可以提高程序的运行效率。
程序加载到内存中运行是计算机系统必须遵循的`基本原则`，也是现代计算机系统高效运行的重要保障。这样做也可以提高程序间的并发能力，也提高了程序的安全性与稳定性。

>而内存的管理，又正好是操作系统的工作。

而要谈的主题——进程，简单的说就是`运行的程序`，也就是加载到内存中的程序，这是需要操作系统进行协调控制的。
计算机作为`管理者`，驱动程序就是`执行者`，各种软硬件资源就是`被管理者`。管理者和被管理者并不需要见面，就像学校里你不用与校长见面，在公司也大概率不会见老板，你们之间的协调是通过辅导员或组长来完成的，而在计算机系统中，这个过程就交由程序作为执行者来完成。
`系统调用`则作为操作系统与程序之间的`接口`，它将操作系统的底层功能暴露给程序，使得程序可以直接调用操作系统提供的服务和资源。系统调用是程序实现系统级别功能的重要手段之一。

操作系统在进程方面的作用可以简单的概括为：`先描述进程，再组织进程`。

# 进程

## 概念
何为描述？又何为组织？
>进程是指正在运行的程序实例，它包含了程序代码、数据和执行状态等信息。操作系统需要对进程进行管理，包括创建、调度、终止、通信等操作。为了对进程进行管理，操作系统需要`先对进程进行描述`，即确定进程的`属性和状态`，如进程ID、优先级、状态等。进程描述通常由`进程控制块（Process Control Block，PCB）`来完成。

在进程描述的基础上，操作系统需要对进程进行组织，以便`进行管理和调度`。进程可以组织成多种形式，如进程队列、进程树等。进程队列是指将同类进程组织到一起，如就绪队列、等待队列等。进程树是指将进程按照父子关系组织起来，形成树形结构。
举个例子，当一个进程需要被调度时，操作系统可以根据进程的属性和状态，选择合适的调度算法将进程调度到CPU上执行。同时，操作系统还可以通过进程间通信等机制，实现进程之间的数据共享和协作。

>一个进程等于`PCB`加上自己的`数据与代码`。

当一个进程需要被加载到内存中去时，首先会创建一个描述进程的结构体对象，也就是PCB。

PCB通常包含以下信息：

1. 进程状态：表示进程当前的状态，如就绪、运行、等待等。
2. 进程标识：标识进程的唯一标识符，如进程ID、父进程ID等。
3. 寄存器值：保存进程在执行过程中各个寄存器的值，如程序计数器、堆栈指针等。
4. 进程优先级：表示进程的优先级，用于调度器进行进程调度。
5. 进程资源：表示进程所占用的资源，如打开的文件、分配的内存等。
6. 进程调度信息：包含了进程的调度信息，如进程的调度时间片、已执行的CPU时间等。

//一个例子去讲标识符