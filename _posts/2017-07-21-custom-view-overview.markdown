---
layout:     post
title:      "自定义View学习概览"
subtitle:   "跟着凯哥学android --- 最近凯哥发了几篇高质量的Android早定义View学习文章，看后受益匪浅。此篇对他的文章进行概括来记录我的学习过程"
date:       2017-07-21 9:14:00
author:     "刘蒙"
header-mask: 0.3
catalog:    true
tags:
    - android
    - 自定义View
---

# 首先总结一下视频中的关键点：

 - 自定义绘制的方式是重写绘制方法，其中最常用的是 onDraw()
 - 绘制的关键是 Canvas 的使用
    - Canvas 的绘制类方法： drawXXX() （关键参数：Paint）
    - Canvas 的辅助类方法：范围裁切和几何变换 
 - 可以使用不同的绘制方法来控制遮盖关系
---
 - 自定义绘制知识的四个级别
	- 1.Canvas 的 drawXXX() 系列方法及 Paint 最常见的使用
	- 2.Paint 的完全攻略
	- 3.Canvas 对绘制的辅助 -> 范围裁剪和几何变换
	- 4.使用不同的绘制方法来控制绘制顺序



