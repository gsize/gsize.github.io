---
layout: post
title: PET系统的时间刻度算法
description: PET系统的时间刻度算法
category: blog
---

# PET系统的时间刻度算法

本文讲述PET系统的coin数据集转换为统计成飞行时间谱。
原始coin事件中的event1比event2早被探测系统记录下来。
两者的飞行时间差为tdiff=time_event2-time_event1。

1. 判断coin的lor在限定的FOV内;
2. 判断coin的两个事件能量是否落在给定的能窗内;
3. 获取coin的lor长度,计算相应的实际飞行时间差值;
4. 计算timeOff=TLUT2-TLUT1;
5. 统计event1的时间谱timeBin =int(TimingBins/2-tdiff-timeOff-lor_dt+0.5);
6. 统计event2的时间谱timeBin =int(TimingBins/2+tdiff+timeOff+lor_dt+0.5);

得到系统的各个探测器的飞行时间谱后，使用重心法，计算出探测器系统的时间偏差值TLUT，再将其带入上述步骤。
经过多次迭代后，达到迭代终止条件，使各个探测器的飞行时间谱具有相同的形态，
从而确定了系统的时间补偿表，提高系统时间分辨率

期间可加入实际lor的飞行时间值补偿，使迭代过程中的时间谱更快收敛。


[Gsize]:    http://gsize.github.io  "Gsize"
