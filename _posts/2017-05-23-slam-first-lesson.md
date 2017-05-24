---
layout: default
title: "First lesson on SLAM"
date: 2017-05-23
---

# First lesson on slam
读书笔记《视觉SLAM十四讲》

## 双目相机的问题

* 配置和标定困难
* 量程和精度受基线和分辨率限制
* 视差的计算非常耗费计算资源



# SLAM框架

1. 视觉里程计

* 关心相邻图像之间的相机运动。
* 计算机是如何通过图像确定相机的运动呢？
* 问题: 累计漂移。 解决办法：后端优化和回环检测。
* 一般被称为前端，和计算机视觉研究领域更为相关，如特征提取和匹配

2. 后端优化
* 主要是处理SLAM过程中的噪声问题。
* 最大后验概率估计
* 一般被称为后端，与滤波及非线性优化比较相关。



3. 回环检测
* 解决的问题：位置随时间漂移的问题。
* 能力：机器人具有识别到过场景的能力。

4. 建图
* 分类：可以分为度量地图和拓扑地图两种。
** 度量地图
  强调精确表示地图中物体的位置关系，通常用稀疏和稠密对其分类。
  定位：稀疏度量地图
  导航：稠密矩阵

** 拓扑地图
  拓扑地图更强调元素之间的关系。 拓扑地图是一个图（graph），由节点和边组成，只考虑节点
  之间的连通性。
* 另一种分类方法：特征地图，几何地图，栅格地图。


## 最短路径
最短路径算法分为两类：
Dijkstra， A*属于静态地图最短路径算法
D* 属于动态地图最短路径算法

### 算法：Dijkstra

### 算法： A*

f(x) = g(x) + h(x)

x是当前点，g(x)是出发点到当前点的距离，h(x)是一个启发值，是当前位置到目标位置的估算代价。

### 算法：D*
* 用于动态环境中的路径规划
* 重点是处理出现的障碍物
* 当前主要使用的是D* Lite 算法
* Obstacle handling[edit]
When an obstruction is detected along the intended path, all the points that are affected are again placed on the OPEN list, this time marked RAISE. Before a RAISED node increases in cost, however, the algorithm checks its neighbors and examines whether it can reduce the node's cost. If not, the RAISE state is propagated to all of the nodes' descendants, that is, nodes which have backpointers to it. These nodes are then evaluated, and the RAISE state is passed on, forming a wave. When a RAISED node can be reduced, its backpointer is updated, and passes the LOWER state to its neighbors. These waves of RAISE and LOWER states are the heart of D*.

## SLAM问题的数学表述

两件事情描述机器人的运动：
1. 运动。k-1到k时刻，机器人的位置x是如何变化的。
  Xk = f(Xk-1, Uk, Wk)
2. 观测。机器人在k时刻位于xk出探测到某个路标yk，用数学语言来描述。
  Zk,j = h(Yj, Xk, Vk,j)


### SLAM问题的形式化
    当知道运动测量读数u，以及传感器读数z时，如何求解定位问题（估计x）和建图问题(估计y)？
    把SLAM建图问题建模成一个*状态估计问题*：如何通过带有噪声的测量数据，估计内部的、
    隐藏的状态变量？

    分类：按照两个方程是否为线性，噪声是否符合高斯分布进行分类，分为线性/非线性系统和
    高斯/非高斯系统。
    * 线性高斯系统（Linear Gaussian， LG）是最简单的，它的无偏最优估计可以由卡尔曼滤波器给出（KF）
    * 非线性非高斯系统（NLNG):用扩展卡尔曼滤波器EKF和非线性优化两大类方法求解。

    历史：
      1. 扩展卡尔曼滤波器：EKF， 线性化误差和噪声高斯分布假设
      2. 粒子滤波器（Particle Filter)
      3. 当前主流使用图优化（Graph Optimization）为代表的技术进行状态估计。
优化技术已明显优于滤波器技术，只要计算资源允许，一般都使用优化技术。
