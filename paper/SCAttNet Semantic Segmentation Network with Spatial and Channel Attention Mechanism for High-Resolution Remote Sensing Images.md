# Title

SCAttNet: Semantic Segmentation Network with Spatial and Channel Attention Mechanism for High-Resolution Remote Sensing Images

## Summary

采用了两个轻量级注意力模块分别对空间和通道信息进行增强

## Research Objective

针对高分辨率的遥控传感数据提出一个更高效的图像语义分割网络

## Problem Statement

HRRSIs数据集存在种类多，背景复杂，类内差别大，类间差别小的干扰。

## Method(s)

1. 提出注意力机制

   以前的研究者采用注意力机制主要在两个方面起作用：一个是空间方面，一个是通道方面；但是这些方法的模型复杂，效率低，对于高分辨率的HRRSIS数据集来说不适合，因此本论文提出==两个轻量级的注意力机制：spatial attention and channel attention==。

   * The spatial attention mechanism decides where to enhance, and the channel attention decides what to enhance

## Evaluation

* datasets

  ISPRS Vaihingen and ISPRS Potsdam datasets

* metrics

  mean inter-section over union (MIoU), average F1-score (AF) and overall accuracy (OA)

## Conclusion

propose a new semantic segmentation network that can adaptively refine features based on the attention mechanismNotes



## Reference

（optional）列出相关性高得文献，以便之后可以继续track