# Title

BiSeNet: Bilateral segmentation network for real-time semantic segmentation

## Summary

提出一个全新的分割网络，用来在不损失速度的情况下，提升准确性。该网络主要包括四个模块：SP、CP、FM、ARM。对于训练则采用了辅助损失函数帮助训练各个模块。

## Research Objective

分割网络实时性和准确性的平衡

## Problem Statement

* 空间信息的损失
* 感受野的增加

## Method(s)

* 针对空间信息的损失

  前人通过Encoder-decoder恢复空间细节，但是回复的细节有限并且引入了高分辨率的额外特征提取层。该论文提出通过构建单独的空间提取模块，通过==三个stride为2的卷积核==构建空间信息模块，对空间信息编码，保存足够的空间细节。
  
* 感受野的大小问题

  前人通过大卷积核、全局池化、空洞卷积、空间金字塔池化等方式解决，但都存在参数量大等缺点。本论文通过构建一个单独的上下文信息提取模块，通过一个轻量级的网络，并且在该网络后面额外添加一层全局池化进行融合。

* 额外引入注意力模块

  注意力模块对特征进行引导

* 特征融合模块

  对于上下文信息和空间信息模块进行融合，不是单纯的sum或者concat。而是通过权重参数进行引导。

## Evaluation

作者如何评估自己的方法，实验的step是什么样的，有没有问题或者可以借鉴的地方

## Conclusion

本论文主要提出了在不损失速度下，提升准确性的分割网络，该网络为了保持足够的空间信息以及感受野，提出了两个模块：SP、CP；并且为了提高整体网络的分割精度，额外增加了注意力模块以及特征融合模块

## Notes

可选的，不符合此框架，但是需要额外记录的笔记

## Reference

（optional）列出相关性高得文献，以便之后可以继续track