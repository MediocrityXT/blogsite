---
title: YOLO笔记/MobileNet/pytorch CUDA踩坑
date: 2021-06-05 08:54:48
updated: 2021-06-05 08:54:48
categories: 知识
tags: 笔记
Math: true
---

# YOLO

CV三任务：分类、目标检测、区域分割

YOLOv1-v5是目标检测经典算法

目标检测分两类：

- Region-free, one stage
- Region-based, two stage

YOLO是前者，一次读取图片同时完成网格划分和分类。

YOLOv1思想是通过每个grid预测(可n个)bounding box来标出目标，每个box的信息由box中间点在grid内的x/y坐标、box的框的高/宽、以及目标置信度 五个量组成。（划分grid不能太大，因为目前是设计为grid里面最多识别一个）
每个grid在预测出的bounding boxes中选择，输出该类别的one-hot 编码。
最后通过非极大值抑制去掉邻近同类grid中所有置信度较差的bounding box

这个bounding box预测过程是一种用卷积网络实现的回归问题，需要ground-truth标注

NMS(非极大值抑制)适用于多目标检测，尤其是不同bounding box重合时，可以抑制掉非中心的框，每个区域内最大置信度的box代表一个目标。NMS不需要提前知道有多少个目标。

# 轻量化神经网络MobileNet

MobileNet大量使用深度可分离卷积，是常用于手机的轻量级网络。

## V1

深度可分离卷积减少了参数量，减少了计算量。https://blog.csdn.net/flyfish1986/article/details/94006907

我理解的深度卷积Depthwise Conv是指沿着深度方向（不同通道）依次卷积。

MobileNetV1的网络结构如上图所示。首先是一个3x3的标准卷积，s2进行下采样。然后就是堆积深度可分离卷积，并且其中的部分深度卷积会利用s2进行下采样。然后采用平均池化层将feature变成1x1，根据预测类别大小加上全连接层，最后是一个softmax层。整个网络有28层，其中深度卷积层有13层。

## V2

MobileNetV2：

![v2结构](https://pic1.zhimg.com/80/v2-367f4025a0d45fc8e2769db6a119a530_1440w.jpg)

参考ResNet中三层残差神经元结构，但先PW升维，DW在6维度空间卷积来过滤数据，再PW降维，这就叫Inverted bottleneck

> ResNet的思想:output(x) = f(x)+x，学习f(x)=0能更快收敛、output里的x解决了梯度消失问题

ReLU函数在低维会导致信息丢失问题，把最后一层低维状态的的ReLU换成线性激活函数。再加上shortcut，所以叫Inverted residual and linear bottleneck。

## V3

MobileNet V2之后谷歌在18年7月发布了MnasNet，思想是针对特定平台（比如Pixel手机）进行网络架构的搜索，使模型运行的延迟更低。它在ImageNet分类任务上是MobileNetV2的1.5倍快，在COCO目标检测任务上也比MobileNet更快且更准。（但是这个搜索时长以天计数）

受到这个模型中的[Squeeze And Excitation](https://muzhan.blog.csdn.net/article/details/108523659)部分（看做一种轻型的attention模块）的启发，MobileNet V3的block也将SE加在DWconv之后，与MnasNet不同的是它将sigmoid函数换成了hard swish激活函数

在用MnasNet和NetAdap搜索之后作者还主要对得到的网络进行了如下优化：

1. 原本最后几层的设计计算代价太高。原本在bottleneck和池化层之间还有一个1x1conv来生成高维特征；现在将这个1x1conv移到7x7池化层后面，在池化之后卷积的计算量从7x7倍减少到1倍。因为这个重要的特征生成层的计算量大大减小，那么前面的升维滤波的inverted bottleneck就多余了，直接删除。最后的结果就是在相似的准确度下减少了11%的Latency。
   ![](https://img-blog.csdnimg.cn/20190606161948714.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDQ3NDcxOA==,size_16,color_FFFFFF,t_70)

2. V3的模型后半部分使用h-swish激活函数。
   $$
   \text {h-swish}[x]=x\frac{\text {ReLU6} (x+3)} 6
   $$
   相较于普通的swish函数，用ReLU代替Sigmoid大大减少计算量，且由于使用ReLU6还减少了不同的sigmoid计算方法带来的数值误差，且这个可被当分段函数计算还减少了内存访问带来的延迟。
   ![](https://img-blog.csdnimg.cn/2019060616204688.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDQ3NDcxOA==,size_16,color_FFFFFF,t_70)

# Pytorch CUDA安装注意事项

为了GPU训练，折磨了很久，踩了一些坑。

1. 首先检查gpu支不支持cuda，一般起码是gtx1050以上
2. 保证显卡驱动不是太低（高版本驱动可兼容低版本）
3. Conda安装合适的pytorch（pytorch只兼容conda里特定版本的cudatoolkit，因为这里的toolkit是编译好的dll库）
4. 如果pip安装了torch需要uninstall，不然会重复导致冲突。(pip下载的包名好像是torch，conda下载的包名是pytorch）
5. cudnn库也要装
6. （可选）下载英伟达的cuda toolkit且安装。这是一个全面的安装包包含了许多其他工具，它安装出来的toolkit版本不重要。因为最终pytorch调用的库版本还是conda装的那个小运行库版本
