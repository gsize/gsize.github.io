---
layout: post
title: 分析DigiGate的实现机制
description: 想了解DigiGate的实现机制，做到Hits的离线数字化过程中不同参数的变化，使singles发生怎么样的变化
category: blog
---

## 概述DigiGate的实现

在做大量模拟的时候，Gate可以先保存Hits数据到Root文件中，
在改变mac脚本中关于信号数字化的参数，同时调用已经保存的的Hits数据，
对同一Hits数据做不同的数字化处理，这样可以实现数字化参数的优化，
同时，保证模拟的底层hits数据是来自同一个物理模拟过程，使不同数字化可比性更一致。

## 实现过程

Gate在开始时先调用GateHitFileReader类，读取已保存的Hits数据文件
（默认的文件名为gate.root，但可以通过“/gate/hitread/setFileName filename”的命令更改调用的hits数据文件，
filename不含有.root后缀）。

GateHitFileReader类提供四个函数给Gate调用，
分别为：PrepareAcquisition()、PrepareNextEvent(G4Event*)、PrepareEndOfEvent()、TerminateAfterAcquisition().

1 PrepareAcquisition()函数为GateOutputMgr开始数据采集时调用，执行Hits数据的root文件打开，同时压入第一个Event的数据转到hitBuffer；
2 PrepareEndOfEvent（）函数把Event的hits导入GateOutputMgr类的CrystalHitCollection之中，用于数字化处理。
3 TerminateAfterAcquisition()函数为GateOutputMgr结束采集数据之前关闭之前打开的Hits数据文件。

4 PrepareNextEvent(G4Event *)函数用于把hits数据以一个event在所有Crystal之中产生的hits为单元循环的读取出来，供GateDigi类数字化。

以上前三个函数由GateOutputMgr类调用，最后一个函数由GatePrimaryGeneratorAction类调用，
但在此类的代码中发现为调用该函数。因此，DigiGate模式运行无法离线处理Hits数据。

控制ROOT格式输出功能的类文件分别是源代码文件夹general中GateRootDefs文件和digits_hits文件夹中的GateToRoot文件



[Gsize]:    http://gsize.github.io  "Gsize"
