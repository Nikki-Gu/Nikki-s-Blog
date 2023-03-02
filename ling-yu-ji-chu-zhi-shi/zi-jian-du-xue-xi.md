# 自监督学习

[知乎专栏](https://zhuanlan.zhihu.com/p/563288232)

自监督学习Self-supervised learning，属于无监督学习的一种方法。“标注”来自数据本身，主要通过各种“auxiliary task、proxy tasks”来提高feature representation的质量，希望模型的backbone能够学到一种通用的特征表达用于下游任务，从而提高下游任务downstream tasks的质量。

> 和representation learning区别？

下面介绍一些常用的代理任务：

* 图像重组Jigsaw Puzzles
  * 预测不同patch的相对位置信息
  * 提高局部特征提取和全局空间信息提取能力
* 图像上色Colorization
* 旋转角度预测
* 修复In-painting
* 多任务：上述几种辅助任务一起对模型进行训练



自监督系列学习

### 1.BERT：基于transformer的大型自监督预训练模型

### 2.MAE

提出一种非对称的encoder-decoder架构，encoder只对没有masked掉的patch子集进行特征提取，decoder对masked patch和提取的特征进行图像重建；proxy task：mask75%的图像重建

Encoder使用ViT架构，只作用于unmasked images，和bert不一样的是，BERT 使用对于 mask 掉的部分使用特殊字符，而 MAE 不使用掩码标记。

Decoder使用transformer架构

* 对大模型作为backbone（encoder）优化效果更好
* partial fine-tuning：**只训练最后模型的若干层的参数**
  * 之前常用的是：
    * **Linear Probing (只训练最后一层线性分类器的参数)**
    * **Fine-tuning (训练所有层的参数)
