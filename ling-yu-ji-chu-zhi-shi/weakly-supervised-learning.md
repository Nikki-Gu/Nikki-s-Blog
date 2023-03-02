# 弱监督学习

网络上关于弱监督学习的介绍大多来源于对周志华发表的综述《A brief introduction to weakly supervised learning》的归纳总结。

本文主要总结一些系统性的信息，不对细节进行介绍。

## 为什么需要**弱监督学习**

- 现实的数据往往缺乏标签
- 数据标注过程的高成本
- 很多任务很难获得如全部真实标签这样的强监督信息

## 弱监督学习分类

- 不完全监督：训练集中部分数据有标签
- 不确切监督：只有粗粒度的标签（MIL多实例学习？）
- 不准确监督：标签和数据存在噪声

## 不完全监督

主要包含两个主动学习和半监督学习两个方向。

半监督学习

- 纯半监督学习pure SSL
- 直推学习（transductive learning）
  - 直推学习和和归纳学习（Inductive Learning）属于两种相对的学派
  - 直推学习是封闭世界；归纳学习是开放世界

![image-20221223194459669](https://nikki-article-pic.oss-cn-beijing.aliyuncs.com/img/image-20221223194459669.png)