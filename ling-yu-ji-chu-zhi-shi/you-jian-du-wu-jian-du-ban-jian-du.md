# SSL: 半监督学习

Semi-Supervised Learning， SSL是半监督学习的缩写。半监督学习不像强化学习一样依赖外界交互，而是通过利用未标记样本来提升模型的性能，主要思想在于关注如何利用大量无标注数据的信息和少量有标注数据的信息进行监督学习。训练数据中同时有有标签数据和无标签数据，一般假设，无标签数据比有标签数据多得多。

[综述](https://www.cnblogs.com/picassooo/p/14923333.html)

### 伪标签Pseudo label

基于伪标签的半监督学习算法，对标签预测错误则往往导致结果下降的问题：

* 标签去噪
* 对前期的无监督代价设置比较少的权重，到后面预测相对准确时慢慢增大权重（权重如何设置和更新？）
* 一定程度的标签噪声可能会引导网络跳出比较 sharp 的极小值点（或许）

### consistency learning

consistency learning是半监督学习常用的一种思想：同一样本经过不同的变换或扰动后，网络对变换前后的输出应该是相似的。

这种思想基于两个假设：

1. smoothness assumption：非常接近的样本点具有相同的分类标签
2. low-density assumption：好的决策边界应该尽可能通过样本稀疏区域（低密度区域）而不是样本密集区域

> 这个和domain adaption里面的consistency regularization的思想很像
>
> CR是数据增强前后？有什么区别？

## 相关论文

### Pseudo-Label : The Simple and Efficient Semi-Supervised Learning Method for Deep Neural Networks

是一篇发表在 ICML 2013 的文章，（可能）是最早的用伪标签Pseudo label方式半监督学习的方法。

### Semi-Supervised Learning with Ladder Networks

2015年诞生半监督 ladderNet，ladderNet是其他文章中先提出来的想法，但这篇文章通过 skip connection 使它work in semi-supervised fashion，而且效果非常好，达到了当时的 state-of-the-art 性能。

在这之前很多半监督深度学习算法是用无监督预训练这种方式对无标签数据进行利用的，但事实上，这种把无监督学习强加在有监督学习上的方式有缺点：两种学习的目的不一致，其实并不能很好兼容。这篇文章认为，无监督预训练一般是用重构样本进行训练，其编码（学习特征）的目的是尽可能地保留样本的信息；而有监督学习是用于分类，希望只保留其本质特征，去除不必要的特征。

### A Simple Semi-Supervised Learning Framework for Object Detection

google

问题：

1.  多类别和多标签的区别?

    multi-label是一张图片有多个label multi-class是一张图片一个类别，但是总的类别数量很多（超过2）
2.  半监督学习具体的训练流程？

    是同时对有标注的数据计算损失更新模型，然后对无标注的数据预测伪标签保存吗？

    还是先对有标注数据计算更新模型之后，再对无标注数据生成一批伪标签？

### ACPL

找hard sample：是度量学习的思想；如何找hard sample：信息量，距离（这个思想在度量学习里面很常规）

优先学习hard sample：anti-curriculum的思想

伪标签筛选：半监督，大部分论文都在回答如何选择可信的/打出可信的伪标签，而没有思考哪些样本最需要打伪标签

本文将hard sample和伪标签结合起来：给hard sample打伪标签

如何打出可信的伪标签：mixup



Anti-curriculum Pseudo-labelling for Semi-supervised Medical Image Classification（CVPR 2022）

[代码](https://github.com/FBLADL/ACPL)

医学图像分析中普遍存在两个问题，一个是多类别和多标签，另一个是类别不均衡。常用的半监督学习策略可以分为基于consistency learning的和自监督预训练的，还有伪标签的？

一般应用于医学图像的半监督学习策略是伪标签，这种策略存在如下问题：

1. 在面对多类别多标签和类别不平衡时存在的问题：分类器会偏向给出样本数量多的类别标签，会恶化样本数少的类别的分类精度。以往的论文对所有类都使用固定的伪标签选择阈值，显然这是不合理的，所以最好是有一种class-wise的阈值，来解决上述问题。但是在实际场景中这种class-wise的阈值很难估计；
2. 所有伪标签都存在的问题：错误的伪标签会增强模型对错误预测的可信度，降低模型精度

本文针对多分类多标签和类别不均衡的半监督医学图像分析提出一种新的方法，主要思想如下

1.  选择the most informative unlabeled images 来进行伪标记，对于如何选择informative unlabeled samples，提出一种信息度量方式：cross-distribution sample informativeness（CDSI）

    这基于之前的研究的观点：对于SSL，在 unlabeled and labeled samples 之间存在一个 distribution shift，在训练过程中，distribution shift会导致模型学不好，所以更有效的学习方法应该是关注学习尽可能远离labelled samples 分布的informative unlabeled samples，对它们打伪标签；同时这些样本也很可能属于样本数少的类别，对这些样本进行关注也就避免了估计class-wise的阈值的需要但还能解决类别分布不平衡问题。

    一个有效的 learning curriculum 必须关注于尽可能远离 labelled samples 分布的 informative unlabeled samples
2.  提出一种新的伪标签机制：informative mixup（IM）

    mixup模型给出的标签和KNN给出的标签（K近邻标签的均值）

    权重取计算出来的特征x的信息密度（一般大于0.5，算出来观察得到的）
3.  提出anchor set purification，ASP钝化方法

    选择 informative pseudo-labelled 样本，并将它们包含在 labelled anchor set，以提高KNN分类器的伪标记精度

创新：有研究使用Curriculum learning来研究pseudo-labelling SSL 方法，但是还没有人使用anti-curriculum learning来研究SSL方法。

```
CDSI: cross-distribution sample informativeness 度量样本信息量的方法
IM: informative mixup  给出伪标签的机制
ASP: anchor set purification 更新anchor set的方法
```

## 自监督系列学习

### 1.BERT：基于transformer的大型自监督预训练模型

### 2.MAE

提出一种非对称的encoder-decoder架构，encoder只对没有masked掉的patch子集进行特征提取，decoder对masked patch和提取的特征进行图像重建；proxy task：mask75%的图像重建

Encoder使用ViT架构，只作用于unmasked images，和bert不一样的是，BERT 使用对于 mask 掉的部分使用特殊字符，而 MAE 不使用掩码标记。

Decoder使用transformer架构

* 对大模型作为backbone（encoder）优化效果更好
* partial fine-tuning：**只训练最后模型的若干层的参数**
  * 之前常用的是：
    * **Linear Probing (只训练最后一层线性分类器的参数)**
    * **Fine-tuning (训练所有层的参数)**
