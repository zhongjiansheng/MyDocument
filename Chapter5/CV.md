# 第五章:计算机视觉

## 一、数据增强

1. **概念**

   * 数据增强也叫数据扩增，意思是在不实质性的增加数据的情况下，让有限的数据产生等价于更多数据的价值。

   

2. **分类**

   * **有监督的数据增强**

     > 单样本数据增强
     >
     > 多样本数据增强

   * **无监督的数据增强**

     > 生成新的数据
     >
     > 学习增强策略

3. **有监督的数据增强**

   3.1 **单样本数据增强**

   * **概念**
     * 所谓单样本数据增强，即增强一个样本的时候，全部围绕着该样本本身进行操作，包括**几何变换类，颜色变换类**等。
   3.2 **多样本增强**
   
   * **概念**
     * 不同于单样本数据增强，多样本数据增强方法利用多个样本来产生新的样本
   * **方法**
     * SMOTE:人工合成新的样本
     * SamplePairing:从训练集中随机抽取两张图片分别经过基础数据增强操作(如随机翻转等)处理后经像素以取平均值的形式叠加合成一个新的样本，标签为原样本标签中的一种。这两张图片甚至不限制为同一类别，这种方法对于医学图像比较有效。
     * mixup:基于邻域风险最小化原则的数据增强方法，它使用线性插值得到新样本数据。
   
4. **思考**

   > (1) 权重衰减，dropout，stochastic depth等方法，是专门设计来限制模型的有效容量的，用于减少过拟合，这一类是显式的正则化方法。研究表明这一类方法可以提高泛化能力，但并非必要，且能力有限，而且参数高度依赖于网络结构等因素。
   >
   > 
   >
   > (2) 数据增强则没有降低网络的容量，也不增加计算复杂度和调参工程量，是隐式的规整化方法。实际应用中更有意义，所以我们常说，数据至上。

## 二、池化

1. 总结

   > 带步长的卷积虽然不需要池化，却没有了灵活的激活机制。平均池化稳扎稳打，却丢失了细节。最大池化克服了平均池化的缺点，却打断了梯度回传。

## 三、卷积神经网络设计

1. 分组卷积（Group Convolution）

   > group conv本身应该就大大减少了参数，比如当input channel为256，output channel也为256，kernel size为3*3，不做  group conv参数为256*3*3*256，若group为8，每个group的input channel和output channel均为32，参数为8*32*3*3*32，是原来的八分之一

2. 3x3卷积核

   > 利用2个3×3卷积核的组合比1个5×5卷积核的效果更佳，同时参数量（3×3×2+1 VS 5×5×1+1）被降低，因此后来3×3卷积核被广泛应用在各种模型中。

3. Inception结构

   > 个输入的feature map分别同时经过1×1、3×3、5×5的卷积核的处理，得出的特征再组合起来，获得更佳的特征。但这个结构会存在一个严重的问题：参数量比单个卷积核要多很多，如此庞大的计算量会使得模型效率低下。这就引出了一个新的结构：

4. Bottleneck结构

   > Inception结构中加入了一些1×1的卷积核

5. ResNet残差网络

   > 传统的卷积层层叠网络会遇到一个问题，当层数加深时，网络的表现越来越差，很大程度上的原因是因为当层数加深时，梯度消散得越来越严重，以至于反向传播很难训练到浅层的网络。为了解决这个问题，何凯明大神想出了一个“残差网络”，使得梯度更容易地流动到浅层的网络当中去，而且这种“skip connection”能带来更多的好处

6. 深度分离卷积（DepthWise）

   > 首先对每一个通道进行各自的卷积操作，有多少个通道就有多少个过滤器。得到新的通道feature maps之后，这时再对这批新的通道feature maps进行标准的1×1跨通道卷积操作。这种操作被称为 “**DepthWise convolution”** ，缩写“DW”。

7. 随机分组（ShuffleNet）

   > ShuffleNet在每一次层叠这种Group conv层前，都进行一次channel shuffle，shuffle过的通道被分配到不同组当中。进行完一次group conv之后，再一次channel shuffle，然后分到下一层组卷积当中，以此循环

8. 通道间的特征都是平等的吗？ -- SEnet

   > ImageNet 2017夺冠架构SENet

9. 空洞卷积（dilated convolution）
10. 可变卷积核













# 语义分割**

## 一、评估指标

1. 语义分割常见指标

   > FN：False Negative,被判定为负样本，但事实上是正样本。
   > FP：False Positive,被判定为正样本，但事实上是负样本。
   > TN：True Negative,被判定为负样本，事实上也是负样本。
   > TP：True Positive,被判定为正样本，事实上也是正样本。

2. 查准率（precision）

   * 指的是预测值为1且真实值也为1的样本在预测值为1的所有样本中所占的比例。以黄瓜问题为例，算法挑出来的黄瓜中有多少比例是好黄瓜。

   * $$
     P=\frac{T P}{T P+F P}
     $$

3. 召回率

   * 指的是预测值为1且真实值也为1的样本在*真实值*为1的所有样本中所占的比例。所有的好西瓜中有多少比例被算法挑了出来。

   * $$
     r=\frac{T P}{T P+F N}
     $$

     

4. IOU

   * 计算真实值和预测值集合的交集与并集之比。

   * $$
     r=\frac{T P}{T P+F N+F P}
     $$

     

