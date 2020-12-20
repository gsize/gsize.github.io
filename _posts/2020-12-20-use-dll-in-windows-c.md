---
layout: post
title: PBS集群系统中增加计算节点
description: 在原有的PBS集群系统中增加计算节点
category: blog
---

# 编译调用dll库 #

最近写一个利用LYSO-PET系统的本底放射性实现时间自刻度的算法，算法调参、代码实现结束，编译成dll库，方便同事集成到软件流程里。
虽然编译dll成功，但总是调用不成。
最终发现在.h接口文件中的预处理有问题，头文件代码块如下：
```c
#ifndef LIBTIMECALIBRATION_H
#define LIBTIMECCALIBRATION_H 1

#define DLL_MSVC 1 //之前这个预编译变量未定义，所以函数接口编译输出没实现，但是能生成dll
#ifdef DLL_MSVC
#define DLL_TEST_APIEXPORT _declspec(dllexport)
#else
#define DLL_TEST_APIEXPORT 
#endif

extern "C" DLL_TEST_APIEXPORT int TimeCalibration(const char **, const int , const char *, const char *, int );

#endif
```
