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
