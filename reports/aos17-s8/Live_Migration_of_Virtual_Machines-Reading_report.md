# Live Migration of Virtual Machines

## 概述

本文作者对虚拟机的实时迁移技术进行了讨论，实时迁移技术主要指在保证虚拟机上的服务程序持续工作的情况下进行将该虚拟机迁移至另外一个物理主机的技术。本文先介绍了几种实时迁移需要经历的不同阶段，作者通过论述不同阶段的优点缺点，最终提出了一种平衡的方法来控制不同阶段所占比重，使得迁移效率最高，同时还能够保持该虚拟机服务对外界的持续可访问性。


## Summary of major innovations

作者讨论不同虚拟机迁移方法的优点和缺点，提出了一种虚拟机高效迁移方法，该方法在实际测试中只有210毫秒的时间处于不可访问状态，在实时迁移上达到了一种较好的效果。

## What the problems the paper mentioned?

对于数据中心或者计算集群来说，他们需要一种技术来帮助他们能够在不关闭服务的前提下进行虚拟机的物理迁移，这样能够将软件硬件清晰的分离开来，便于故障管理、负载均衡等等。

## How about the important related works/papers?

Collective项目为用户提供了虚拟机迁移的技术，这一项目致力于为在多个地点工作的人提供便利，他们采取的策略是在传输时会关掉虚拟机，受限于个人用户的带宽，他们还在缩小镜像文件上做了努力，而本篇文章与此相反，本文面向的对象主要是大型计算机集群，在带宽足够的情况下，提供一种能够时虚拟机持续工作的实时迁移技术。Zap的研究中提出了部分虚拟化技术，他的迁移比Collective项目更快，但也没有做到实时迁移。本文介绍的技术有较大部分采用了之前NomadBIOS的工作“预拷贝迁移技术”。

## What are some intriguing aspects of the paper?

作者在制定迁移策略时，第二阶段循环拷贝这一方法较好，这一方法可以在循环时逐渐筛选出少量变化频率高的内存，这样在停机迁移时只需要将这少部分内存拷贝过去即可迅速在另一台物理机上启动虚拟机，提供服务。

## How to test/compare/analyze the results?

作者使用跟踪文件来评估循环预拷贝迁移的效率。详细来讲就是给定一个固定网络带宽用于内存页的传输，然后给定会被改变的页的数量，然后重复的进行循环，最后可以估计出在停机拷贝阶段所传输的数据量，也就可以根据此估计出停机时间。

## How can the research be improved?

我个人存在一下问题：在设计迁移过程时，为何不将循环拷贝和Demand-migration想结合，循环拷贝正好可以弥补目标机的“内存页前期不足时会影响性能”的缺点，如果将这两者结合是否可以实现0 Downtime？

## If you write this paper, then how would you do?

我会比较文中提到的方法和我上一个问题中提到的方法，通过不同角度来对这两种方法进行讨论。