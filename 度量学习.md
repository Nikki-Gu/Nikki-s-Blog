# 度量学习 Metric Learning

一种机器学习方法。通过定义相似性或者距离函数，用于表示不同样本之间的相似性或者差异，度量学习将学习样本特征映射到embedding space的方法。

度量学习的作用：降维、特征提取，预测（推荐系统中的BPR算法）

度量学习的方法主要在于损失函数和采样方法的设计，采样方法和特定的损失函数往往是相辅相成，配套使用的。

## 基础知识

### 损失函数

基本思路是选取标签不同的样本对使他们的距离增大，或者选取标签相同的样本对使他们的距离减小。首先需要定义距离函数或者相似度函数。如果使用距离函数，则函数值越大表示两个向量之间越不相似。如果使用相似度函数则恰恰相反，数值越大表示越相似。

常用的函数：

```
import numpy as np

# 随机定义A, B 两个向量
A = np.random.randn(10)
B = np.random.randn(10)

# 欧几里得距离(Euclidean distance)
dist = np.square(np.sum(A - B)**2)

# 曼哈顿距离(Manhattan distance)
dist = np.sum(np.abs(A - B))

# 切比雪夫距离(Chebyshev distance)
dist = np.max(np.abs(A - B))

# cosine距离
similarity = (np.sum(A * B))/(np.linalg.norm(A)) / (np.linalg.norm(A))
```

### 困难样本hard sample

但是在实际场景中，样本量通常都非常巨大，很难穷尽所有的样本，实际经验中不少样本信息量很少，对度量学习的准确性增益有限，因此要想实现有效的度量学习，除了设计损失函数外，还需要精心设计采样方法，尽可能保证使用信息量较高的样本(称为困难样本挖掘，hard sample mining)，也有文章称为informative sample

## 常用的损失函数和采样方法

### Contrastive loss 和 Triplet loss

两种最基本的损失函数和采样方法，优点应该在于简单便于理解，但是缺点也很明显：

- 单独使用收敛速度慢
- 学习后期大多数样本都能满足损失函数的约束条件，所以对进一步学习的贡献很小
- 主要用于二分类问题，对于多分类问题中作用有限

#### Contrastive loss

最基本的度量学习思路。采取的采样方法是随机采样，如果两者标签一致则减小两者间的距离，反正增大两者的距离。但需要用一个margin作为边界，避免正负样本间的距离过大。(这个margin在其他损失函数中也很常见，是为了保证模型的鲁棒性和泛化能力)

<img src="https://nikki-article-pic.oss-cn-beijing.aliyuncs.com/img/%E5%BA%A6%E9%87%8F%E5%AD%A6%E4%B9%A01.png" alt="image-20221212154123644" style="zoom:50%;" />

缺点：两个样本间的距离缺乏参照性，也就是说两个正样本之间的距离可能在优化后仍然大于正负样本间的距离。

#### Triplet loss

设计用于解决Contrastive loss的缺点。采样方法是每次选取三个样本——第一个样本作为锚点anchor，第二个样本和anchor标签一致，第三个样本和anchor标签不一致。损失函数保证锚点和负样本的距离大于锚点和正样本的距离。

<img src="https://nikki-article-pic.oss-cn-beijing.aliyuncs.com/img/%E5%BA%A6%E9%87%8F%E5%AD%A6%E4%B9%A02.png" alt="image-20221212154147224" style="zoom:50%;" />

这种设计保证了正样本间的距离与正负样本间的距离差大于m，考虑到了相对的距离关系。

## N-pair-ms loss 和 Lifted struct loss

### N-pair-ms

针对多分类，考虑到多分类问题中负样本不止一个问题，需要将所有标签不一致的负样本都纳入损失计算，从一个batch中增大x和所有对于x的负样本和对于x的正样本之间的距离差。

<img src="https://nikki-article-pic.oss-cn-beijing.aliyuncs.com/img/%E5%BA%A6%E9%87%8F%E5%AD%A6%E4%B9%A03.png" alt="image-20221212154222285" style="zoom:50%;" />

xi相对于x来说均是标签不一致的样本（负样本），f是样本到隐藏空间的映射函数

### Lifted struct loss

基于训练中每个batch动态生成三元组，这里考虑到了样本的信息量/困难度，认为距离正样本最近的负样本是最具信息量，最难学习的样本。选择的三元组是按照这种方法来选择的最具信息量的样本：

<img src="https://nikki-article-pic.oss-cn-beijing.aliyuncs.com/img/%E5%BA%A6%E9%87%8F%E5%AD%A6%E4%B9%A04.png" alt="image-20221212154236028" style="zoom:50%;" />

P是正样本对的集合，（i，j）是一对正样本对，L是希望最小化两者之间的距离

下面一个式子是对每一对正样本对，模型分别独立的去挖掘他们最困难的负样本（也就是距离他们最近的负样本），分别为k和l，然后选出里面距离最小的一个记作n。根据最困难的三元组计算三元组{i，j，n} 计算triplet loss函数。

