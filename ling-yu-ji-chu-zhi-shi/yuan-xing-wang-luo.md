# 原型网络

[基本介绍](https://xiaobaiha.gitbook.io/tech-share/machinelearning/yuan-xing-wang-luo)

prototypical network，一种用于解决小样本学习（few shot learning）或者零样本学习（zero shot learning）的方案

前身算是matching networks：通过从训练一个matching网络提取训练集图片特征，在测试时将新图片提取特征后计算其与已有图片特征之间的余弦距离，从而做出类别判断。

> 度量学习和原型网络，和representation learning的区别？
